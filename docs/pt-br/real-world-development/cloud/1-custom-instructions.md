---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou se houver informações de contexto que o Copilot deva conhecer — deixe esse contexto acessível. Os [arquivos de instruções][instruction-files] fornecem essa orientação para que o Copilot entenda não apenas *o que* você quer, mas também *como* quer que o trabalho seja feito.

Nesta lição, você vai:

- explorar como o contexto do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos com escopo por caminho.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, você não executará um prompt antes e depois para comparar os resultados. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar esse efeito em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests produzidos pelo agente.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter um visual moderno.
- A documentação deve ser adicionada ao código na forma de comentários TSDoc.
- Um bloco de comentários deve ser adicionado ao início de cada arquivo para descrever sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot, ajudando-o a entender seu estilo de programação e seus requisitos. Você pode especificar convenções, bibliotecas e os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações, como a stack, uma visão geral do que está sendo criado, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md`, arquivos que podem fornecer instruções para tarefas ou tipos de arquivo específicos, como determinadas linguagens ou a criação de componentes de UI e testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados apenas para geração de código no Copilot Chat — não para autocompletar nem para sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem usam os arquivos no nível do repositório e os arquivos `*.instructions.md` (com frontmatter `applyTo`) ao gerar código.
>
> O Copilot também oferece suporte a outros padrões de arquivos de instruções, como `AGENTS.md` e `CLAUDE.md` — consulte a [referência de compatibilidade][custom-instructions-support].

### Boas práticas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está além do escopo deste workshop, mas os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado em orientações do projeto, como uma descrição do que está sendo criado, a estrutura do projeto e padrões globais de programação.
- Use `*.instructions.md` para fornecer orientações sobre tipos de arquivo (como testes unitários, componentes Astro e a camada de dados) ou tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos de como o código deve — e não deve — ser escrito.

Não há uma única forma correta de criar arquivos de instruções ou de usar IA. Com a experimentação, você descobrirá o que funciona melhor para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot pode se beneficiar de uma coleção sólida de arquivos de instruções. Neste projeto, há orientações para várias tarefas, incluindo [atualizações de UI][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa opção de uma forma diferente — por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição do ambiente em que você está explicará o recurso quando ele for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], que reúne arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas do projeto

Reserve um momento para ler os arquivos de instruções incluídos no repositório: há um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Explore a pasta `.github/instructions`. Há instruções para arquivos Astro, para a camada de dados Drizzle, para testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início. Ele define um glob, relativo à raiz do repositório, para determinar a quais arquivos as instruções se aplicam. Neste exemplo, o glob abrange arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, conjuntos maiores de instruções podem ser divididos em arquivos menores e reutilizáveis, além de indicar ao Copilot exemplos para seguir. Os caminhos são relativos ao arquivo de instruções, e não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário no nível do arquivo.

## Adicionar um novo padrão ao repositório

Esta será a única edição feita nesta lição: adicionar um padrão para todo o projeto que exija comentários TSDoc e um cabeçalho de comentário no nível do arquivo. O agente de nuvem usará essa orientação nas issues que você atribuir a ele mais adiante.

Antes de editar, crie uma branch (o agente de nuvem lê as instruções da branch de destino da issue, mas você fará um commit organizado das alterações):

1. No terminal do Codespace, crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Abra novamente `.github/copilot-instructions.md` e localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões do projeto, mas ainda não exige documentação no código; por isso, o helper gerado não tinha comentários de documentação.

Adicione as linhas Markdown a seguir logo depois dos padrões existentes:

```markdown
- Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
- Before imports or any code, add a comment block to the file that explains its purpose.
```

Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, as instruções podem ser criadas no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações globais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões do projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Fazer commit, push e merge da atualização

O agente de nuvem lê as instruções da branch que é o destino da issue. Na próxima lição, o Copilot criará uma branch a partir de `main`; por isso, as alterações nas instruções precisam chegar a `main` para que o agente de nuvem as use.

1. Adicione e faça commit das alterações:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem lerá essas instruções quando trabalhar nas issues atribuídas na próxima lição.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar a criação da branch e fazer commit direto em `main`. A etapa da branch demonstra como o trabalho seria feito em um projeto real.

## Resumo e próxima etapa

Você explorou como o Copilot recebe contexto dos arquivos de instruções do projeto e adicionou um padrão válido para todo o repositório a `.github/copilot-instructions.md`. Você verá o efeito dessa regra nos pull requests gerados pelo agente de nuvem nas próximas lições.

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
