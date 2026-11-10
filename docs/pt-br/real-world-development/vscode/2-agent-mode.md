---
title: "Exercício 2 - Adicionar funcionalidades com o modo agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma alteração simples em um aplicativo costuma envolver vários arquivos e tarefas, como executar testes. Em geral, uma pessoa desenvolvedora precisa localizar os arquivos relevantes, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando, fazer outra alteração e assim por diante.

É aí que entra o modo agente do Copilot.

O modo agente foi criado para trabalhar com mais autonomia no IDE. Ele age de forma semelhante a uma pessoa desenvolvedora: explora a estrutura existente do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos ver como usá-lo para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo agente e o agente de nuvem oferecem experiências diferentes. O modo agente executa tarefas no IDE e permite ciclos de feedback e interação rápidos. O agente de nuvem funciona de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo agente antes de fazer merge na base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e distribuidora. Você já adicionou um helper de distribuidoras no exercício anterior; agora, concluirá o restante do trabalho na camada de dados, na interface e nos testes com o modo agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para conhecer a funcionalidade existente.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza as páginas como HTML estático durante o build. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM. Não há uma API de backend separada nem um framework de interface do lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` do Astro que usa as APIs padrão do DOM.

### Iniciar o site

O script `npm run dev` inicia o servidor de desenvolvimento do Astro. Siga estas etapas para executá-lo no GitHub Codespaces:

1. Volte ao codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá uma mensagem com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir `http://localhost:4321` em uma janela do navegador, feche-a selecionando **x**.

5. Para abrir o site, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link para a URL local no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse endereço é um túnel privado para o codespace, que agora hospeda o servidor web.

### Explorar o site

Com o site em execução, explore suas funcionalidades. As principais são:

- **Página inicial**: mostra uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você abre uma página com mais informações, como título, descrição, distribuidora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indicará o que selecionar.

A implementação inicial do site funciona, mas queremos adicionar novos recursos. Vamos começar revisando o backlog. A configuração do projeto a partir do modelo criou automaticamente um conjunto de issues; peça ao GitHub Copilot para mostrá-las.

1. No menu de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos do terminal e invoca ferramentas.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo em particular. Durante a criação do workshop, usamos Claude Sonnet 4.5 e o incluímos nas instruções para tornar as sugestões de código mais consistentes. Como os LLMs são probabilísticos, suas sugestões podem ser diferentes das mostradas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas deverá conseguir concluir a tarefa.

3. Envie o prompt a seguir para perguntar ao Copilot sobre o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que o comando liste todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens conforme a utilidade que parecem ter para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto. Você usará o arquivo de instruções da [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos, incluindo os de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Assim, é possível reutilizar orientações, dividir conjuntos complexos de instruções em partes menores e fornecer exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações no aplicativo:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser aprovados antes do merge. O modo agente do Copilot pode executar essas tarefas.

1. Continue na conversa atual ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. No menu de agentes da exibição Chat, confirme que **Agent** continua selecionado.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada no backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorando o projeto e localizando os arquivos relacionados à funcionalidade, incluindo os helpers da camada de dados, a interface e os testes. Em seguida, ele alterará os arquivos e executará os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e executar testes. Isso pode levar alguns minutos, conforme a complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer todas as alterações. Não é necessário selecionar esses botões enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e alguns testes podem falhar durante o processo. Tudo bem: o Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar erros.

   ![Sessão completa do Copilot no modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, vimos como usar o modo agente do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Aprendemos como:

- o modo agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- o modo agente pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes gerados pelo modo agente antes do merge.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra — implementar paginação

À medida que a lista de jogos crescer, será necessário habilitar a paginação. Use o que aprendeu neste exercício e peça ao Copilot que implemente esse recurso. Considere:

- seguir as boas práticas existentes, incluindo os arquivos de instruções.
- decidir como a paginação deve funcionar, por exemplo, se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- fornecer no prompt as orientações necessárias para obter a implementação desejada.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
