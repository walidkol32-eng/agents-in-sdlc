---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nas últimas lições, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas dos agentes com pull requests abertos, explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que seria útil adicionar também um modo claro. Como o trabalho já inclui alterações no estilo do site e uma opção para alternar entre modos, faz sentido incluir essa funcionalidade. Você orientará o agente para garantir que ele implemente tanto o modo claro quanto o modo de alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos verificar o status das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem usada.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você conhece as tarefas em andamento, peça ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

    ![Sessão de acessibilidade no painel central de agentes](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos, até que a configuração seja concluída e o trabalho comece. Você verá mensagens semelhantes às mostradas a seguir.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

    ![Tarefa do agente de nuvem na página de agentes com a caixa de diálogo para orientar uma sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot reconhece a solicitação e a incorpora ao trabalho.

## Deixar o Copilot trabalhar

O Copilot incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Como isso pode levar alguns minutos, aproveite para fazer uma pausa e refletir sobre o que aprendeu até aqui.

## Resumo e próxima etapa

Nesta lição, você explorou a página de agentes do Copilot, o painel central para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. Nele, você pode acompanhar tarefas em andamento e concluídas, revisar o trabalho e redirecionar uma sessão ativa.

Você aprendeu:

- a usar o painel central e a página de agentes para acompanhar tarefas do agente de nuvem.
- a redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, você pode [iterar nos pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
