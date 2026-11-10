---
title: "Lição 1 - Instalar o GitHub Copilot CLI"
description: "Instale e autentique o Copilot CLI no Codespace, conheça o ambiente e localize a issue de filtragem criada pelo modelo."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O [GitHub Copilot CLI][about-copilot-cli] é um assistente de programação baseado em agentes que funciona no terminal e permite explorar bases de código, gerar código, executar comandos e interagir com ferramentas externas — tudo pela linha de comando. Você pode delegar tarefas, solicitar alterações e manter o foco. Como você pode imaginar, o primeiro passo é instalar a ferramenta. Felizmente, isso pode ser feito com ferramentas que você já conhece.

Nesta lição, você vai:

- instalar o GitHub Copilot CLI com npm.
- autenticar-se com sua conta do GitHub.
- confiar no repositório do workshop e iniciar uma conversa.
- encontrar a issue de filtragem usando o servidor MCP integrado do GitHub.

## Cenário

Sua equipe está começando a usar agentes de IA para trabalhar em um backlog crescente. O Copilot CLI leva esses recursos ao terminal, onde muitas pessoas desenvolvedoras já trabalham. Nesta lição, você instalará e autenticará a ferramenta para usá-la no restante do workshop.

## Instalar o Copilot CLI

Você pode instalar o Copilot CLI com [npm][install-cli], WinGet ou Homebrew. Como o GitHub Codespaces já inclui o Node.js, você usará npm.

1. Volte ao Codespace e abra um terminal.
2. Confirme se o Node.js está instalado e atende ao requisito de versão:

   ```bash
   node --version
   ```

   A versão deve ser 24 ou posterior.

3. Instale o Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Verifique a instalação:

   ```bash
   copilot --version
   ```

   O número da versão deverá ser exibido.

## Autenticar com o GitHub

Na primeira vez que iniciar o Copilot CLI, você precisará autenticar sua conta do GitHub.

1. Inicie o Copilot CLI:

   ```bash
   copilot
   ```

2. Se solicitado, siga as instruções do código de dispositivo para se autenticar e autorizar o Copilot CLI.
3. O Copilot CLI exibirá este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirme que o caminho corresponde ao repositório Tailspin Toys e selecione **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Em um Codespace, talvez você já esteja autenticado por meio da sessão do GitHub. Se o Copilot CLI iniciar sem pedir autenticação, está tudo certo.

## Conhecer o ambiente

Os comandos digitados no prompt normal do shell são executados diretamente no Codespace. Depois que o Copilot CLI iniciar, o texto em linguagem natural será enviado ao agente, e os comandos de barra controlarão a conversa.

1. Digite `/model`, use as setas para selecionar **Auto**, pressione <kbd>Enter</kbd> e, em seguida, pressione <kbd>Enter</kbd> novamente para confirmar.
2. Digite `/help` para ver os comandos disponíveis na versão instalada e pressione <kbd>Esc</kbd> para fechar a tela de ajuda.
3. Faça uma pergunta simples ao Copilot para confirmar que tudo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Leia a resposta e observe como o Copilot explora o repositório antes de responder.
5. Digite `/mcp list` e confirme que o servidor MCP integrado do GitHub está disponível.
6. Peça ao Copilot que encontre a issue de filtragem:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abra a URL e leia a issue. Você voltará a ela depois de concluir uma primeira alteração rápida.

> [!TIP]
> Uma sessão normal do Copilot CLI trabalha na branch selecionada no terminal; ela não cria um worktree automaticamente. Você criará uma branch de recurso antes de cada alteração.

## Usar o atalho do workshop

Normalmente, o Copilot CLI pede autorização antes de usar ferramentas fora das permissões já concedidas. Neste workshop, você o iniciará novamente com `--yolo`, um atalho autorizado pelo usuário que remove esses pedidos de confirmação dentro do Codespace, permitindo que você se concentre nos exercícios.

> [!CAUTION]
> `--yolo` habilita permissões automáticas completas (`--allow-all-tools`, `--allow-all-paths` e `--allow-all-urls`). Use-o somente em um ambiente isolado, como um Codespace ou uma VM, e nunca o defina como alias padrão para o desenvolvimento diário. Consulte [Permitir e negar o uso de ferramentas][allow-all-warning] para saber mais.

Neste workshop, `--enable-all-github-mcp-tools` habilita as ferramentas MCP do GitHub com permissões de leitura e gravação, usadas em lições posteriores para trabalhar com issues e pull requests. O Codespace limita o acesso ao computador local, mas os recursos autenticados do GitHub continuam sendo reais. Revise as alterações antes de publicá-las ou fazer merge.

1. Saia do Copilot CLI com `/exit`.
2. Reinicie-o na raiz do repositório:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Faça outra pergunta rápida sobre o projeto para confirmar que a conversa funciona e, depois, saia com `/exit`.

O Copilot salva as conversas automaticamente. Mais adiante, depois de alterar instruções ou adicionar um agente, você usará `copilot --resume` para voltar à mesma conversa e branch do recurso.

## Resumo e próximos passos

Parabéns! Nesta lição, você:

- instalou o GitHub Copilot CLI com npm.
- autenticou-se com sua conta do GitHub.
- confiou no repositório do workshop e iniciou uma conversa.
- encontrou a issue de filtragem usando o servidor MCP integrado do GitHub.

Em seguida, você [fará sua primeira alteração específica][next-lesson] e usará o Copilot CLI para exibir uma avaliação por estrelas nos cards dos jogos.

## Recursos

- [Instalar o GitHub Copilot CLI][install-cli]
- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
