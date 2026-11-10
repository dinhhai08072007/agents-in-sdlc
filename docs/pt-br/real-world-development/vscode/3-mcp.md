---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: modo agente][previous-lesson] · [Próxima lição: agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo agente. Antes de abrir um pull request, confirme que ele funciona no navegador. Em vez de navegar pelo aplicativo manualmente, você conectará o **servidor MCP do Playwright** e permitirá que o Copilot controle um navegador real para testar o recurso. Depois, publicará a branch e abrirá o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que o use para testar manualmente o recurso de filtragem em um navegador.
- publicar a branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo agente fica mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é o padrão que permite ao Copilot se comunicar com ferramentas e serviços externos.

![Diagrama da arquitetura do modo agente e da interação com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome por meio de ferramentas.

Os servidores MCP funcionam como pontes entre os agentes de IA e as ferramentas e os serviços externos. Eles gerenciam a comunicação com ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes NPM. Cada servidor MCP oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios do GitHub, criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com Playwright para navegar por páginas web, preencher formulários e selecionar botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e a contribuição para o ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise cuidadosamente o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações sensíveis.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Observe que o servidor `github` já está configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada vem do modelo do projeto. Por isso, o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é preciso instalar nada localmente: o VS Code se conecta por HTTP e você se autentica na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicionar o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, permitindo testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, o `playwright` é um servidor **local**: o VS Code o inicia no computador executando `npx @playwright/mcp@latest`. A opção `--headless` faz o Playwright executar o navegador sem uma janela visível, o que é necessário em um codespace, onde não há uma área de trabalho para exibição.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia os servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e escolha a opção para confiar nele.

Iniciar o servidor manualmente é opcional. Se você não fizer isso, o VS Code iniciará o servidor e pedirá sua confirmação na primeira vez que o Copilot precisar usar as ferramentas.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, e não capturas de tela. Assim, o agente analisa elementos estruturados e rotulados — botões, links e itens de lista — da mesma forma que as tecnologias assistivas. Uma verificação funcional rápida também pode funcionar como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso de filtragem que você acabou de criar:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, executará cada etapa e apresentará os resultados. Compare o resumo com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas de acompanhamento ou peça que o Copilot corrija o código antes de abrir um pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` durante o teste. Se você parou o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Agora que você confirmou que o recurso funciona, pode abrir um pull request (PR) para que a equipe o revise. Primeiro, publique a branch `filtering-vscode`.

1. Abra o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias formas de criar um pull request, inclusive pelo github.com e pela interface de linha de comando do GitHub. Como você já está usando o GitHub Copilot, vamos pedir a ele que crie o PR e o associe à issue relevante.

1. Abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma sessão.
2. No menu de agentes, confirme que **Agent** está selecionado para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot obtenha informações e execute operações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar. Siga as instruções para permitir o acesso.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o PR.
6. Selecione o link exibido pelo Copilot para revisar o pull request, mas **não faça merge ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou o servidor para testar manualmente o recurso de filtragem em um navegador antes de publicá-lo.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das formas de ampliar os recursos do Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
