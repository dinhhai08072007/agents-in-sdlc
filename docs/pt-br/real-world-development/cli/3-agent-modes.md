---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o modo Plan para combinar uma abordagem, o Autopilot para implementar a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Mudanças maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo de uma organização e ajudar a construir os recursos certos da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes: começará com uma issue para criar um recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, fará o merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch para continuar o fluxo de trabalho do recurso. Normalmente, você usaria branches ou PRs diferentes para os vários tipos de arquivo, mas neste workshop vamos simplificar para destacar os conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar o recurso usando o modo Autopilot.
- revisar o código.
- validar manualmente o recurso em um navegador encaminhado.

À medida que continuar desenvolvendo o recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e distribuidora. A issue do backlog descreve o recurso, mas alguns detalhes — como a combinação de categorias — precisam ser decididos antes de começar a programar. Você usará o modo Plan para chegar a essas decisões e, em seguida, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

A introdução de agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! Muitos desenvolvedores seguem um fluxo semelhante a este:

1. Começar com uma issue registrada que descreva o trabalho necessário.
2. Criar um plano do que precisa ser implementado.
3. Programar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, fazer o merge.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria segue uma variação desse processo.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Começar pela issue

Antes de começar a filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR de avaliação por estrelas foi integrado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para mostrar todas as issues.
8. Use as setas para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt começa com `#7` (ou um número semelhante). O caractere `#` permite adicionar ao contexto uma issue ou um pull request do GitHub.

## Planejar o recurso de filtragem

O planejamento permite definir a abordagem para implementar um recurso ou realizar uma tarefa antes de delegá-la ao Copilot. Para tarefas complexas, é uma boa ideia reservar algum tempo para planejar. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a elaborar o plano. Primeiro, explorará o projeto e depois determinará a melhor abordagem.

3. Durante esse processo, o Copilot pode fazer perguntas sobre o funcionamento da filtragem. Responda conforme suas preferências — não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role a página para cima e para baixo para revisar o plano.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar no modo Autopilot.

O Autopilot permite que o Copilot continue iterando sobre o problema até considerar que a tarefa foi concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou uma opção com nome semelhante na versão instalada.
2. Confirme que o indicador de modo abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Assim que o código for gerado, ele precisa ser revisado antes do merge, como qualquer outro código. Vamos revisar o código e iniciar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador de modo não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois da revisão do código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija eventuais falhas.

## Explorar a nova funcionalidade

O código parece correto — mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste a filtragem por categoria, por distribuidora e as combinações definidas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou o recurso usando o modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, vamos aprofundar como gerar código que siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
