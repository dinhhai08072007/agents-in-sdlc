---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: agentes personalizados][previous-lesson] |
|:--|

No modo agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real no seu computador, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo à medida que ocorre. Você pode revisar cada diff, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design faz outra solicitação: também quer uma opção de modo claro para as pessoas que acham o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Monitorar o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, role a conversa para acompanhar as chamadas de ferramentas. Você verá leituras e edições de arquivos e comandos do terminal que o agente solicitar para executar.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - selecione **Keep** para aceitar a alteração.
   - selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados na sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão — por exemplo, para executar um comando do terminal — o agente fará uma pausa e mostrará a opção **Allow**. Aprove somente os comandos com os quais você se sente confortável.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Ele deverá apresentar uma mensagem final resumindo as alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar uma opção de modo claro. Como continuará na mesma sessão de chat, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilos e o uso do armazenamento local.

1. No campo de entrada do Copilot Chat, envie o prompt de acompanhamento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo atualizações para o modo claro.
3. Revise cada nova edição quando aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** para cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface de alternância de uma forma que você não prefere — interrompa-o com o botão **Stop** ao lado do campo do chat e envie um prompt de esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir a direção ("não adicione uma terceira opção; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para as duas opções").

## Revisar o conjunto de alterações

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se seguem os padrões de acessibilidade do agente personalizado: atributos ARIA, navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- monitorou o trabalho de um agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo agente][agent-mode]

---

| [← Lição anterior: agentes personalizados][previous-lesson] | [Próxima lição: iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
