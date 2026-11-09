---
title: "Exercício 3 - Testar o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ele funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor MCP do Playwright** e peça ao Copilot que teste o recurso em um navegador real. Depois, publique a branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que o use para testar manualmente o recurso de filtragem no navegador.
- publicar a branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite que o Copilot se comunique com ferramentas e serviços externos.

![Diagrama da estrutura interna do modo de agente e da interação com contexto, LLM e ferramentas, incluindo ferramentas de servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas.

Essas ferramentas e recursos são acessados por um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos. O servidor MCP gerencia a comunicação entre o agente e as ferramentas externas, como APIs existentes ou ferramentas locais em pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos disponíveis para o agente.

Alguns servidores MCP conhecidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: fornece acesso a APIs para gerenciar repositórios do GitHub, criar repositórios e gerenciar issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com Playwright, como acessar páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e a contribuição para o ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações sensíveis.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Observe que já há um servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada `github` vem do template do projeto; por isso, o Copilot conseguiu ler as issues do backlog no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é preciso instalar nada localmente. O VS Code conecta-se por HTTP, e você autentica no GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicionar o servidor MCP do Playwright

Agora, você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` junto de `github`, de modo que o arquivo fique assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, o servidor `playwright` é **local**: o VS Code o inicia na sua máquina com `npx @playwright/mcp@latest`. O sinalizador `--headless` executa o navegador sem uma janela visível, o que é necessário no Codespace, pois não há um desktop para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia os servidores MCP **sob demanda**: na primeira vez que o Copilot precisar das ferramentas de um servidor, o VS Code o iniciará e pedirá que você confirme se confia nele.

1. Em `.vscode/mcp.json`, selecione a ação **Start** que aparece acima da entrada `playwright` para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code pedir que você confirme se confia no servidor, revise a configuração e autorize-o.

Iniciá-lo manualmente é opcional. Se você ignorar essa etapa, o VS Code iniciará o servidor e pedirá autorização quando o Copilot precisar das ferramentas na etapa seguinte.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real. O agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e apresentar um resumo. Assim, você verifica rapidamente se o recurso se comporta como esperado sem sair da conversa.

O servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, e não capturas de tela. O agente trabalha com elementos estruturados e rotulados (botões, links, itens de lista), como a tecnologia assistiva; assim, uma verificação funcional também serve como uma breve verificação de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá as etapas e apresentará os resultados. Compare o relatório com os critérios de aceitação da issue. Se algo estiver incorreto, faça perguntas adicionais ou peça uma correção antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento tiver sido parado, inicie-o antes de enviar o prompt. Na primeira vez que usar o servidor MCP do Playwright, o Copilot talvez precise baixar um navegador. Se ele informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Depois de confirmar que o recurso funciona, publique a branch `filtering-vscode` para abrir um pull request (PR) e permitir que a equipe revise o trabalho.

1. Abra o painel **Source Control** no Codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

    ![Painel Source Control mostrando as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias formas de criar um pull request, inclusive pelo github.com e pela interface de linha de comando do GitHub (CLI). Como você já está trabalhando com o GitHub Copilot, peça que ele crie o PR e o associe à issue relevante.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão.
2. Confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Selecione **Continue** quando necessário para permitir que o Copilot reúna informações e execute as operações. Na primeira vez que usar uma ferramenta do GitHub, talvez seja necessário autenticar; siga as instruções.
5. Observe o Copilot pesquisar as issues, localizar a correta e criar o PR.
6. Abra o link criado pelo Copilot para revisar o PR, mas **não faça merge ainda**.

## Resumo e próxima etapa

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou o servidor para testar manualmente o recurso de filtragem no navegador.
- publicou a branch e abriu um pull request para o trabalho.

Você usou um servidor MCP para *testar* um recurso, mas há outras formas de ampliar os recursos do Copilot. Em seguida, [criará um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
