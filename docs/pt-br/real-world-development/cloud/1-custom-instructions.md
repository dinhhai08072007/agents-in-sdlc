---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: pré-requisitos][previous-lesson] · [Próxima lição: agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou se houver informações de contexto que o Copilot deva conhecer — você precisa garantir que elas estejam disponíveis. Os [arquivos de instruções][instruction-files] permitem fornecer essa orientação para que o Copilot entenda não apenas *o que* você quer que ele faça, mas também *como*.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo por caminho.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Ao contrário dos ambientes do VS Code e da CLI, aqui você não executará um prompt *antes e depois*. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante neste percurso, ao revisar os pull requests criados pelo agente de nuvem.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem um conjunto de diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- A documentação deve ser incluída no código na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever o que ele faz.

Com o uso de arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso ajuda a orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de programação, bibliotecas e até os tipos de comentários que prefere incluir no código. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo, fornecendo contexto no nível da tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto — contexto relevante para a maioria das solicitações feitas ao Copilot por chat ou CLI. Isso pode incluir a stack usada, uma visão geral do que está sendo criado, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md`: arquivos que podem ser criados para tarefas ou tipos de arquivo específicos. Use-os para fornecer orientações para linguagens específicas, como TypeScript ou Astro, ou para tarefas como criar um componente de UI ou um conjunto de testes unitários.

> [!NOTE]
> Ao trabalhar no IDE, os arquivos de instruções são usados apenas para gerar código no Copilot Chat — não para autocompletar nem para sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções no nível do repositório quanto os arquivos `*.instructions.md` (com frontmatter `applyTo`) ao gerar código.
>
> O Copilot também oferece suporte a arquivos de instruções em outros formatos, incluindo `AGENTS.md` e `CLAUDE.md`. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support].

### Boas práticas para gerenciar arquivos de instruções

Uma discussão completa sobre como criar arquivos de instruções está além do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em alto nível:

- Mantenha o conteúdo de `copilot-instructions.md` focado em orientações no nível do projeto, como uma descrição do que está sendo criado, a estrutura do projeto e padrões globais de programação.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve — e não deve — ser escrito.

Não há uma única forma de criar arquivos de instruções, assim como não há uma única forma de usar IA. Com a experimentação, você descobrirá o que funciona melhor para o seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção robusta de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você encontre instruções para vários tipos de tarefa, incluindo [atualizações de UI][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de forma diferente — por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição sobre cada interface indicará a opção quando for relevante.
>
> Está procurando modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório repleto de arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório — há um `copilot-instructions.md` principal e uma coleção de arquivos `*.instructions.md` para várias tarefas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe a orientação aninhada para **GitHub Actions Workflows**. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
3. Abra a pasta `.github/instructions` e explore os arquivos. Observe que há instruções para arquivos Astro, a camada de dados com Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início: ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`, serão incluídos.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis, além de apontar o Copilot para exemplos. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário no nível do arquivo.

## Adicionar um novo padrão ao repositório

A próxima etapa é a única edição que você fará neste exercício: adicionar uma regra válida para todo o projeto, exigindo que a documentação seja incluída no código como comentários TSDoc e cabeçalhos de comentário no nível do arquivo. O agente de nuvem usará essa regra quando trabalhar nas issues que você atribuir a ele mais adiante.

Antes de editar, crie uma branch (o agente de nuvem lerá as instruções da branch de destino da issue, mas você fará o commit das alterações de forma organizada):

1. No terminal do codespace, crie e acesse uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como mencionado anteriormente, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos registrar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela descreve os padrões de programação do projeto, mas ainda não tem uma regra para documentação no código, motivo pelo qual o helper gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes, para instruir o Copilot a incluir cabeçalhos de comentário no arquivo e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações globais, ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local certo para padrões válidos em todo o projeto, como a regra para comentários de documentação que você acabou de adicionar.

## Fazer commit, push e merge da atualização de instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`. Portanto, suas alterações nas instruções precisam estar em `main` para que o agente de nuvem as use.

1. Adicione os arquivos ao stage e faça o commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request da branch `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem lerá essas instruções quando trabalhar nas issues que você atribuir a ele.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar as etapas da branch e fazer commit diretamente em `main`. A branch foi incluída para que o workshop reflita o fluxo de trabalho usado em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto dos arquivos de instruções deste projeto e adicionou um novo padrão válido para todo o repositório a `.github/copilot-instructions.md`. Você verá a influência desse padrão nos pull requests que o agente de nuvem criar nas próximas lições.

Em seguida, você [atribuirá sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
