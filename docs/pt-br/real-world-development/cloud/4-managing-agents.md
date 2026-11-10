---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel de controle, você vê todas as tarefas do agente com pull requests abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que seria um bom momento para adicionar também um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar uma opção de alternância, pareceu fazer sentido incluir essa funcionalidade. Você orientará o trabalho do agente para garantir que ele adicione o modo claro junto com o modo de alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos verificar o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento. Revise as operações já executadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull request (PR) à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você viu as tarefas ativas, vamos pedir ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à criação do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você saberá que isso aconteceu quando vir mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt a seguir:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente de nuvem na página de agentes com a caixa de diálogo Steer active session while Copilot is working destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

Assim como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa que estava executando quando você enviou a mensagem.

Como antes, isso levará alguns minutos. É um bom momento para fazer uma pausa e refletir sobre o que você aprendeu e explorou até agora.

## Resumo e próximos passos

Este exercício apresentou a página de agentes do Copilot, seu centro para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Nela, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho e até redirecionar uma sessão em andamento para ajustar o escopo ou fornecer outras orientações.

Você explorou estes conceitos:

- usar o painel de controle e a página de agentes para monitorar tarefas do agente de nuvem.
- redirecionar uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: agentes personalizados][previous-lesson] | [Próxima lição: iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
