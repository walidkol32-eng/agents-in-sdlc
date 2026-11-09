---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou se houver informações que o Copilot deva conhecer — deixe esse contexto acessível. Os [arquivos de instruções][instruction-files] fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos com escopo por caminho.
- gerar a primeira parte dos dados de filtragem (um helper de editoras) com as instruções *atuais* em vigor.
- adicionar um novo padrão válido para todo o repositório a `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código gerado adotar o novo padrão.
- fazer commit das instruções e da primeira parte da filtragem em `main` para que o próximo exercício possa aproveitar o trabalho.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo é observar a *tendência* de mudança depois de atualizar as instruções, e não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter um visual moderno.
- A documentação deve ser adicionada ao código na forma de comentários TSDoc.
- Um bloco de comentários deve ser adicionado ao início de cada arquivo para descrever sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para executar o trabalho de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot, ajudando-o a entender seu estilo de programação e seus requisitos. É possível especificar convenções, bibliotecas e os tipos de comentários preferidos. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações feitas pelo chat ou pela CLI, como a stack, uma visão geral do que está sendo criado, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md`, arquivos que fornecem instruções para tarefas ou tipos de arquivo específicos, como determinadas linguagens ou a criação de componentes de UI e testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados apenas para geração de código no Copilot Chat — não para autocompletar nem para sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem usam os arquivos no nível do repositório e os arquivos `*.instructions.md` (com frontmatter `applyTo`) ao gerar código.
>
> O Copilot também oferece suporte a outros padrões, como `AGENTS.md` e `CLAUDE.md` — consulte a [referência de compatibilidade][custom-instructions-support].

### Boas práticas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está além do escopo deste workshop, mas os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado nas orientações do projeto, como uma descrição do que está sendo criado, a estrutura e os padrões globais de programação.
- Use `*.instructions.md` para orientações específicas sobre tipos de arquivo (como testes unitários, componentes Astro e a camada de dados) ou tarefas.
- Use linguagem natural e orientações claras. Inclua exemplos de como o código deve — e não deve — ser escrito.

Não há uma única forma correta de criar arquivos de instruções ou de usar IA. Com a experimentação, você descobrirá o que funciona melhor.

> [!TIP]
> Todo projeto que usa o GitHub Copilot pode se beneficiar de uma coleção sólida de arquivos de instruções. Neste projeto, há orientações para várias tarefas, incluindo [atualizações de UI][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa opção de forma diferente — por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição do ambiente em que você está explicará o recurso quando ele for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], que reúne arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas do projeto

Leia os arquivos de instruções incluídos no repositório: há um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe a orientação aninhada sobre **GitHub Actions Workflows**. Essas informações se aplicam a todas as interações com o Copilot.
3. Explore a pasta `.github/instructions`. Há instruções para arquivos Astro, para a camada de dados Drizzle, para testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início. Ele define um glob, relativo à raiz do repositório, para determinar a quais arquivos as instruções se aplicam. Neste exemplo, o glob abrange arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, é possível dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir. Os caminhos são relativos ao arquivo de instruções, e não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário no nível do arquivo.

## Criar uma branch para as alterações

Crie uma branch para trabalhar nas alterações.

1. Volte ao Codespace usado na lição anterior.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch e selecione-a:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

Para observar o efeito das instruções personalizadas, envie um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam em cada exercício; as etapas indicarão as opções relevantes.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot receba somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deverá ficar.
3. Selecione **Agent** no seletor de agentes da visualização Chat para permitir que o Copilot altere arquivos.

    ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará alterações, que podem incluir o helper e os testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna um array tipado de editoras. Isso segue as convenções da camada de dados definidas em `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observe que o código proposto **não tem** comentários TSDoc nem cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele inclua comentários de documentação mesmo sem essa orientação. Tudo bem: o objetivo continua sendo observar a melhora da *consistência* depois da atualização das instruções.

## Adicionar um novo padrão ao repositório

O arquivo `.github/copilot-instructions.md` fornece orientações do projeto ao Copilot. Vamos documentar os padrões de programação para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões de programação, mas ainda não exige documentação no código.
3. Adicione as linhas Markdown a seguir logo depois dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, as instruções podem ser criadas no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões do projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Executar novamente o prompt e observar a alteração

Com a nova regra de documentação, peça ao Copilot que atualize o helper de editoras criado anteriormente. A mesma orientação de padrões guiará a atualização.

1. No Copilot Chat, selecione **New Chat** para limpar a conversa.
2. Selecione novamente o arquivo `src/lib/publishers.ts` para que o Copilot se concentre nele.
3. Envie o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função também inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo a um código que será usado no próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são controlados pelo Git como qualquer outro arquivo do repositório.

1. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirme que as alterações estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções e de todos os arquivos alterados pelo Copilot para o helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Com a branch publicada, crie um pull request (PR) associado à issue sobre a atualização dos arquivos de instruções. O Copilot pode localizar a issue, criar um PR que a encerra e fazer o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub. O template do projeto já conecta as ferramentas do GitHub ao workspace. Na primeira vez que o Copilot usar uma delas, o VS Code talvez peça que você entre no GitHub. Siga as instruções para autorizar. Você aprenderá sobre essa conexão (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot que localize a issue sobre os arquivos de instruções e crie um PR a partir da branch atual, descrevendo tanto as instruções quanto a base do helper de editoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Quando o Copilot solicitar autorização para executar ações no GitHub, revise o comando e selecione **Allow** conforme apropriado.
5. Quando o PR for criado, peça ao Copilot que faça o merge e volte à branch `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Quando o Copilot solicitar autorização para executar ações no GitHub ou comandos de shell, revise o comando e selecione **Allow** conforme apropriado.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próxima etapa

Você explorou como o Copilot recebe contexto dos arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um padrão válido para todo o repositório a `.github/copilot-instructions.md`.
- executar o mesmo prompt novamente e observar o código proposto adotar o novo padrão.
- fazer commit das instruções e do helper em `main` para que o próximo exercício possa aproveitar o trabalho.

Em seguida, você usará as instruções no [modo de agente][next-lesson] enquanto o Copilot adiciona um recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
