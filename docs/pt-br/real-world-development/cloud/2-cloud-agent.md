---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Instruções personalizadas][previous-lesson]

Muitas organizações enfrentam problemas de dívida técnica: vulnerabilidades de segurança não resolvidas, código legado que precisa de atualização ou solicitações de funcionalidades que ficaram paradas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request de rascunho para que uma pessoa desenvolvedora revise a proposta. Assim, você pode delegar tarefas repetitivas e se concentrar em outras prioridades.

Neste exercício, você explorará:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de delimitar claramente o escopo das issues.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem problemas de dívida técnica a resolver. A equipe gostaria de adicionar comentários TSDoc às funções exportadas do aplicativo.

A equipe de design também quer melhorar a descoberta de jogos. O pedido é mostrar, na página de detalhes de cada jogo, outros jogos da mesma categoria. A equipe quer uma implementação simples para testar a experiência; esse trabalho está bloqueado por outras issues de prioridade mais alta. São tarefas que podem ser delegadas ao agente de nuvem do Copilot para que ele trabalhe de forma assíncrona enquanto você se dedica a outras atividades.

## Conhecer o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][copilot-agents] executa tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Por exemplo, você pode [atribuir uma issue do GitHub ao Copilot][assign-issue]. Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot: o trabalho de raciocínio continua necessário. Até tarefas aparentemente simples podem ter complexidades ocultas; portanto, pense com cuidado ao [definir o escopo das tarefas para o agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

As lições anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções incluídos no repositório para reconhecer o efeito deles nos resultados.

Abra os arquivos a seguir na interface web do GitHub ou no Codespace:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, principalmente as orientações para TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao agente de nuvem, observe os comentários TSDoc, os padrões TypeScript e os cabeçalhos de comentário no pull request. As instruções do repositório influenciam essas alterações.

## Configurar o ambiente de desenvolvimento do agente de nuvem

Criar código normalmente exige um ambiente específico e a execução de scripts para instalar ferramentas e dependências. O mesmo vale para tarefas do Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] para configurar o ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração especial][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes de o agente começar. O template já inclui essa configuração para instalar Node.js, dependências do projeto, navegadores do Playwright para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido do aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para estas etapas. O Copilot recebe um token próprio para as operações dele.
    permissions:
      # Se as etapas de configuração clonarem o repositório para instalar dependências, por exemplo, será necessária a permissão `contents: read`. Se o repositório não for clonado nessas etapas, o Copilot fará isso automaticamente depois que elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do aplicativo: Node.js (o aplicativo agora é Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar a configuração sem esperar o início de uma tarefa do Copilot.

## Adicionar documentação

Muitos projetos têm documentação desatualizada ou insuficiente. Esse tipo de dívida técnica pode reduzir a produtividade e dificultar a manutenção do código e a integração de novas pessoas à equipe. O Copilot pode ajudar a criar documentação e trabalhar nessa tarefa em segundo plano.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

    ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) criado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um pouco e atualize a página.
12. Aguarde o Copilot criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer. O Copilot está configurando o ambiente com o workflow descrito anteriormente, analisando o projeto e definindo uma abordagem.

13. Revise as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request e procure a atualização que indica que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Acompanhe a sessão em tempo real e observe como o Copilot explora o código, analisa o estado do projeto, define uma abordagem e cria código.

Esse trabalho pode levar alguns minutos. Uma das vantagens do agente de nuvem é executar tarefas de forma assíncrona, enquanto você se dedica a outras atividades. Vamos aproveitar esse recurso e atribuir outra tarefa ao agente; depois, você poderá se concentrar em desenvolver uma funcionalidade.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma vantagem do agente de nuvem do GitHub Copilot é dividir o trabalho: você pode se concentrar em uma tarefa enquanto ele executa outra. Vamos atribuir ao Copilot a solicitação da equipe de design para mostrar jogos relacionados.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Em pouco tempo, você verá o emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O agente de nuvem está trabalhando nessa segunda tarefa. Ele age de forma semelhante a uma pessoa engenheira de software: não é necessário acompanhá-lo o tempo todo, mas revise o trabalho quando ele terminar. Enquanto isso, vamos criar e usar agentes personalizados.

## Resumo e próxima etapa

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues para execução assíncrona e delegar a resolução de dívida técnica, a criação de funcionalidades ou migrações entre frameworks.

Você explorou:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de delimitar claramente o escopo das issues.
- como atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, você pode [criar e usar agentes personalizados][next-lesson]. O agente de nuvem também pode usar [servidores MCP][cloud-agent-mcp] e instruções personalizadas, apresentadas em lições anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
