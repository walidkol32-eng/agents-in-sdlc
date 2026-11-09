---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até uma pequena atualização em um aplicativo pode exigir alterações em vários arquivos e a execução de testes. Normalmente, uma pessoa desenvolvedora localiza os arquivos necessários, faz alterações, executa os testes, depura problemas, descobre o que faltou e atualiza o código novamente.

O modo de agente do Copilot pode ajudar nesse trabalho. Ele explora a estrutura do projeto, faz alterações coordenadas, executa tarefas como testes e corrige erros encontrados. Vamos usá-lo para adicionar uma funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona no GitHub, como uma pessoa parceira da equipe, usando issues e pull requests.

Neste exercício, você aprenderá a:

- usar o modo de agente do Copilot para explorar o projeto, encontrar arquivos relevantes e coordenar alterações.
- implementar uma funcionalidade que afeta a interface e a camada de dados.
- revisar alterações e testes gerados antes do merge.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes usando o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de alterar o código, explore o site para entender as funcionalidades existentes. O Tailspin Toys é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. O site lista jogos e exibe seus detalhes. O aplicativo Astro gera páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local por meio do Drizzle ORM; não há API de back-end nem framework de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade usa pequenos elementos `<script>` do Astro com APIs padrão do DOM.

### Iniciar o site

Para facilitar a execução, há uma tarefa `npm run dev` que inicia o servidor de desenvolvimento do Astro.

1. Volte ao Codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
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

   Quando o servidor Astro estiver pronto, você verá uma mensagem semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir o navegador em `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> No Codespace, o link local será encaminhado para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor em execução no Codespace.

### Explorar o site

Os principais recursos do site são:

- **Página inicial**: mostra uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: apresenta mais informações sobre um jogo, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam em cada exercício; as etapas indicarão as opções relevantes.

A primeira versão do site funciona, mas queremos adicionar recursos. Comece revisando o backlog de issues que foi criado automaticamente a partir do template.

1. Selecione **Agent** no seletor de agentes da visualização Chat. O agente pode planejar e implementar alterações em vários arquivos, executar comandos de terminal e chamar ferramentas.

    ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos Claude Sonnet 4.5 ao criar estas instruções para tornar as sugestões relativamente consistentes. Como os modelos de linguagem são probabilísticos, as sugestões podem ser diferentes dos exemplos; isso é normal.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem, o Copilot pode usar um comando MCP diferente e ainda concluir a tarefa.

3. Envie o prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que o Copilot liste as issues.
5. Revise a lista gerada.

Observe como o Copilot priorizou os itens de acordo com a utilidade para quem usa o site.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações sobre arquitetura, princípios de componentes, testabilidade e acessibilidade, além dos links para instruções específicas de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem apontar para outros arquivos de instruções e para arquivos do projeto. Os caminhos são relativos ao arquivo de instruções, o que permite reutilizar orientações, dividir conjuntos maiores em partes menores e fornecer exemplos.

## Implementar a filtragem

Para concluir a filtragem, será necessário:

- adicionar ou atualizar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Os testes também precisam passar antes do merge. O modo de agente do Copilot pode executar essas tarefas.

1. Continue na conversa atual ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** para adicionar o arquivo de instruções.

    ![Exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado.

    ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado.
5. Peça ao Copilot que implemente a funcionalidade de acordo com a issue:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os helpers da camada de dados, a interface e os testes. Em seguida, ele alterará os arquivos e executará os testes.

    ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e executar testes. Isso pode levar alguns minutos. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o trabalho terminar, você verá esses controles para todas as alterações — não é necessário selecioná-los enquanto o agente ainda trabalha.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

    ![Caixa de diálogo do Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez haja pausas ou falhas em alguns testes durante o processo. O Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar erros.

    ![Sessão concluída do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Inspecione o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próxima etapa

Parabéns! Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu a:

- implementar funcionalidades na interface e na camada de dados com o modo de agente.
- explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes gerados antes do merge.

Em seguida, você [testará o recurso com o servidor MCP do Playwright][next-lesson] e abrirá um pull request.

### Exercício extra — implementar paginação

À medida que a lista de jogos cresce, será necessário paginá-la. Use o que aprendeu neste exercício e peça ao Copilot que implemente a paginação. Considere:

- seguir as práticas existentes e usar os arquivos de instruções apropriados.
- decidir se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- fornecer orientações suficientes no prompt para que o Copilot implemente o comportamento desejado.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
