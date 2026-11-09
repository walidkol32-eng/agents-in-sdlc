---
title: "Exercício 5 - Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse uma funcionalidade de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo modos de alto contraste e claro. Vamos revisar as alterações e, se necessário, orientar o Copilot para melhorar o resultado.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a IA generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Antes de habilitar a revisão para o restante da equipe, vamos revisar as propostas do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem executa tarefas de forma assíncrona e sem supervisão, existem controles de segurança para proteger o ambiente:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o trabalho.
- O agente de nuvem é executado no GitHub Actions, em um ambiente separado e temporário.
- Uma pessoa precisa aprovar a execução de qualquer workflow do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão][agent-firewall], inclusive para servidores MCP.

## Revisar a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot, que adiciona documentação ao código. Você fará essa revisão usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você poderá ver um aviso de que o firewall bloqueou o GitHub Copilot. Isso é **esperado**: o acesso a recursos externos, inclusive servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR estará marcado como **[WIP]**. Aguarde a conclusão da tarefa. Isso pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que pode incluir comentários TSDoc e outra documentação. As alterações exatas variam. Verifique os comentários TSDoc, os padrões TypeScript e os cabeçalhos de comentário; eles são orientados pelos arquivos de instruções personalizados que você examinou no exercício anterior.
6. Quando terminar a revisão, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows aparecerão na seção de verificações do PR. Aguarde a aprovação das verificações do projeto para o aplicativo Astro. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

O trabalho com o Copilot em um pull request não é uma via de mão única. Você pode mencioná-lo em comentários do PR ou em comentários associados a trechos de código. O Copilot verá o comentário e iniciará outra sessão para responder. Como os resultados são não determinísticos, não há um texto único a usar. Por exemplo, você pode pedir ao Copilot que:

- adicione um cabeçalho com uma breve descrição a cada arquivo de código.
- inclua comentários TSDoc nos arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot** como faria com outra pessoa. Use uma das ideias acima ou outra sugestão.
2. Selecione **View Session** para acompanhar o trabalho do Copilot na nova sessão.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar, um novo commit aparecerá no PR.
5. Selecione **Files changed** para revisar as alterações.

Continue iterando até que o resultado esteja adequado. Quando estiver satisfeito, marque o PR como pronto para revisão e faça o merge na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar a funcionalidade de jogos relacionados

Agora, vamos revisar o PR criado pelo Copilot para resolver a issue de jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** e revise o código gerado.
5. Quando as alterações estiverem revisadas, volte à guia **Conversation** e role a página para baixo.
6. Procure um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Aguarde as verificações do projeto do aplicativo Astro na seção de verificações do pull request.
9. **Opcional:** Para testar manualmente a funcionalidade de jogos relacionados, troque para a branch criada pelo Copilot no Codespace. No terminal, execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch (por exemplo, `copilot/fix-8`):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou a funcionalidade de jogos relacionados. Você pode continuar iterando com o agente de nuvem, por exemplo, para alterar a quantidade de jogos exibidos ou adicionar cabeçalhos de comentário e comentários TSDoc. Lembre-se: essa tarefa foi atribuída **antes** da atualização das instruções personalizadas. Para pedir ajustes, adicione um comentário na guia **Conversation**; o Copilot iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado. Esse PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro pedido no painel central do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR estará marcado como **[WIP]**. Aguarde a conclusão do trabalho.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, observando especialmente:
   - os componentes da interface para alternar entre os modos.
   - o uso do armazenamento local do navegador para manter as preferências.
   - as alterações de estilo para os modos de alto contraste e claro.
   - os recursos de acessibilidade, como atributos ARIA e navegação por teclado.
   - o código JavaScript/TypeScript que controla a alternância entre modos.
6. Quando terminar a revisão, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Aguarde as verificações do projeto do aplicativo Astro na seção de verificações do pull request.
10. **Opcional:** Para testar manualmente os recursos de acessibilidade, troque para a branch criada pelo Copilot no Codespace:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Depois, inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme que funcionam e continuam selecionados após recarregar a página.

O agente personalizado de acessibilidade ajudou a orientar a implementação segundo as boas práticas de acessibilidade. Se encontrar problemas ou melhorias, mencione **@copilot** em um comentário para solicitar alterações, como nos PRs anteriores.

## Exercício opcional — continuar delegando

O agente de nuvem é útil para tarefas reais do backlog que você pode delegar enquanto trabalha em outra atividade. Para praticar, crie mais algumas issues no repositório e atribua-as ao Copilot. Por exemplo:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **Inspecionou os arquivos de instruções personalizados do repositório** para observar como influenciam o trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, com alternância entre os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para acompanhar e orientar a sessão de acessibilidade.
- **Revisou e refinou os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar, explore outros ambientes:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas no terminal usando Plan, Autopilot, skills, um agente de QA, MCP do Playwright, Agent Merge e a integração opcional `/delegate` com o agente de nuvem.

No seu próprio repositório, experimente:

- atribuir ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
