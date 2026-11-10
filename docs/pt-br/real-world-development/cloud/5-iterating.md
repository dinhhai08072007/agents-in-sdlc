---
title: "Exercício 5: iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo opções de alto contraste e modo claro. Vamos revisar as alterações e, se necessário, dar feedback ao Copilot antes de habilitar a revisão para o restante da equipe.

### Cenário

Como já foi destacado várias vezes, os fundamentos do design de software e do DevOps não mudam com a inclusão da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, existem restrições de segurança para manter o ambiente seguro. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para suas alterações.
- O agente de nuvem executa o trabalho em um ambiente efêmero e separado, dentro do GitHub Actions.
- Uma pessoa precisa aprovar a execução de qualquer workflow do GitHub Actions.
- Por padrão, o [acesso a recursos externos é limitado](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revisar a documentação gerada

Vamos começar examinando o primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot — o que adiciona documentação ao código. Para isso, usaremos a interface padrão de PRs no GitHub.com.

> [!NOTE]
> Ao abrir o PR, você talvez veja um aviso de que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos, incluindo servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com título semelhante a **Add missing documentation** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos, então você pode fazer uma pausa ou refletir sobre o que aprendeu até aqui.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui os comentários TSDoc e outras documentações. As alterações exatas variam.

   Ao revisar o código, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles seguem os arquivos de instruções personalizadas que você examinou no início do Exercício 2.

6. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencioná-lo em comentários do PR — como faria com outra pessoa da equipe — ou em comentários de linhas de código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não podemos fornecer um texto exato para solicitar alterações. Você pode pedir, por exemplo, que o Copilot:

- adicione cabeçalhos de comentário ao início de cada arquivo de código, descrevendo brevemente sua finalidade.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar as alterações, um novo commit deverá aparecer no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar o PR como pronto para revisão e fazer o merge na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Vamos voltar ao PR que o Copilot criou para resolver a issue de exibir jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page** ou outro mais específico.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode trocar para essa branch no codespace para testar manualmente o recurso de jogos relacionados. Acesse o codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados! Assim como antes, você pode iterar com o agente de nuvem do Copilot para solicitar atualizações. Por exemplo, peça que ele altere a quantidade de jogos relacionados ou adicione cabeçalhos de comentário e comentários TSDoc (lembre-se: esse trabalho foi atribuído **antes** de atualizar as instruções personalizadas). Para isso, adicione um comentário na guia **Conversation**; o Copilot verá o comentário e iniciará outra sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Esse PR deve incluir o modo de alto contraste que você solicitou no Exercício 3 e o modo claro pedido no painel de controle do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website** ou outro mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Examine a implementação, prestando atenção especial a:
   - componentes de interface para alternar entre os modos.
   - uso do armazenamento local para manter as preferências.
   - alterações de CSS ou estilos para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que controla a alternância entre os modos.

6. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode trocar para essa branch no codespace para testar os recursos de acessibilidade manualmente. Acesse o codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme que funcionam e continuam selecionados depois de recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar a implementação segundo boas práticas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria, você pode mencionar **@copilot** em um comentário e solicitar atualizações, como fez nos PRs anteriores.

## Exercício opcional — continuar delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e se dedicar a outras atividades. Para desenvolver esse hábito, crie algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página de lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Examinou os arquivos de instruções personalizadas do repositório** para ver como influenciam o trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando opções de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão de acessibilidade.
- **Revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar aprendendo, explore os outros percursos:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas no terminal com os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, Playwright MCP, Agent Merge e uma integração opcional `/delegate` com o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- atribua ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- crie um agente personalizado para outro domínio.
- configure `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
