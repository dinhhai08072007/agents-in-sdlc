---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: pré-requisitos][previous-lesson] · [Próxima lição: modo agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou se houver informações de contexto que o Copilot deva conhecer — você precisa disponibilizar essas informações. Há várias formas de fornecer contexto ao Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que orientam a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo por caminho.
- gerar a primeira parte dos dados da filtragem (um helper de distribuidoras) com as instruções *atuais*.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- executar o mesmo prompt novamente e observar o código gerado adotar o novo padrão.
- fazer commit das instruções e da primeira parte da filtragem em `main`, para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos, pois o Copilot é não determinístico. O objetivo é observar a *tendência* de mudança de comportamento depois de atualizar as instruções, não obter exatamente a mesma saída caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- A documentação deve ser adicionada ao código na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever o que ele faz.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Você pode especificar convenções de programação, bibliotecas e tipos de comentários, além de criar instruções para todo o repositório ou para tipos específicos de arquivo.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações feitas ao Copilot por chat ou CLI, como a stack, uma visão geral do projeto, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md`: arquivos com diretrizes para linguagens específicas, como TypeScript ou Astro, ou para tarefas, como criar um componente de UI ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados apenas para geração de código no Copilot Chat — não para autocompletar nem para sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` (com frontmatter `applyTo`) ao gerar código.
>
> O Copilot também oferece suporte a outros formatos de instrução, incluindo `AGENTS.md` e `CLAUDE.md`. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support].

### Boas práticas para gerenciar arquivos de instruções

Uma discussão completa sobre como criar arquivos de instruções está além do escopo deste workshop. Ainda assim, os exemplos do projeto mostram uma abordagem representativa. Em alto nível:

- Mantenha `copilot-instructions.md` focado em orientações no nível do projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões globais de programação.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve — e não deve — ser escrito.

Não há uma única forma de criar arquivos de instruções, assim como não há uma única forma de usar IA. Com a experimentação, você descobrirá o que funciona melhor para o seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção robusta de arquivos de instruções. Neste projeto, há orientações para várias tarefas, incluindo [atualizações de UI][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de forma diferente — por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição sobre cada interface indicará a opção quando for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório de arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos neste repositório — há um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, veja também as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot neste repositório.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados com Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` no início define um glob (relativo à raiz do repositório) que determina os arquivos aos quais as instruções se aplicam. Neste exemplo, os arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`, serão incluídos.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Assim, é possível dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis, além de apontar o Copilot para exemplos. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário no nível do arquivo.

## Criar uma branch para as alterações

Vamos seguir boas práticas e criar uma branch para as alterações.

1. Volte ao codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e acesse-a:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indicará o que selecionar.

Para observar o impacto das instruções personalizadas, envie um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para comparar os resultados.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot receba apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. No menu de agentes da exibição Chat, selecione **Agent** para que o Copilot possa alterar arquivos.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará alterações de código, muitas vezes no arquivo do helper e nos respectivos testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna um array tipado de distribuidoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Se isso acontecer, tudo bem: o objetivo continua sendo observar o aumento de *consistência* depois da atualização das instruções.

## Adicionar um novo padrão ao repositório

Como mencionado, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos registrar os padrões de programação para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões de programação, mas ainda não inclui uma regra para documentação no código, motivo pelo qual o helper gerado não tinha comentários.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, as instruções podem ser criadas no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local certo para padrões válidos para todo o projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Executar o prompt novamente e observar a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione novamente `src/lib/publishers.ts` para direcionar o Copilot ao arquivo correto.
3. Envie o **mesmo prompt**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Helpers de acesso a dados de distribuidoras da plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de distribuidoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Retorna uma lista de todas as distribuidoras com seus IDs e nomes.
    *
    * @param db - O cliente do banco de dados Drizzle.
    * @returns Uma promessa que é resolvida com um array de objetos de distribuidora.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código real que será usado no próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são recursos do repositório e devem ser gerenciados pelo controle de versão, como qualquer outro arquivo. Vamos fazer commit das alterações e enviar a branch ao repositório.

1. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirme se as instruções atualizadas e as alterações do helper estão presentes:

   ```bash
   git status
   ```

3. Adicione ao stage e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para criar a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer merge de um pull request

Com a branch enviada, vamos abrir um pull request e vinculá-lo à issue sobre o padrão de documentação no backlog. O Copilot pode encontrar a issue, criar um PR associado e fazer o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executará ações no GitHub. O modelo do projeto já configura as ferramentas do GitHub no workspace, então não é preciso configurar nada. Na primeira vez que o Copilot usar uma ferramenta, o VS Code talvez peça que você entre no GitHub. Siga as instruções para permitir o acesso. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que encontre a issue relacionada à atualização dos arquivos de instruções e crie um PR a partir da branch atual. O PR deve indicar que fecha a issue e mencionar a base do helper de distribuidoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a procurar a issue e criar o PR.
5. Se o Copilot pedir permissão para executar ações do GitHub, revise a solicitação e selecione **Allow** se for apropriado.
6. Quando o PR for criado, peça ao Copilot que o integre em `main` e atualize a branch local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Se o Copilot pedir permissão para executar ações do GitHub ou comandos do shell, revise a solicitação e selecione **Allow** se for apropriado.

Você acabou de criar e integrar um pull request com a ajuda do GitHub Copilot!

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão válido para todo o repositório a `.github/copilot-instructions.md`.
- enviar o mesmo prompt novamente e observar o código proposto adotar o novo padrão.
- fazer commit da atualização das instruções e da base do helper em `main`, para que o próximo exercício possa aproveitar o trabalho.

Em seguida, você aplicará essas instruções no [modo agente][next-lesson], enquanto o Copilot adiciona um recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
