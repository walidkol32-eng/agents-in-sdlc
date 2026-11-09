---
title: "Lição 8 - Criar e mesclar o PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Agora, revise o conjunto e abra um pull request. Você fez o merge do primeiro PR manualmente; desta vez, usará o **Agent Merge** para gerenciar o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções e da skill e o agente de QA em alguns PRs. Para agilizar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e branch, e incluiremos o trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar todas as alterações do recurso e as evidências de verificação.
- criar o PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar o merge do PR.

## Cenário

Durante o fluxo de trabalho da filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle de quem desenvolve.

## Conhecer o Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir um pull request. Quando você o habilita, o Copilot resolve os impedimentos do PR — corrige verificações de integração contínua (CI) que falharam, responde aos comentários da revisão e faz rebase quando necessário — e, depois, habilita o merge automático do GitHub quando o repositório permite.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode cuidar desse trabalho. Revise as alterações e confirme que estão prontas antes de habilitá-lo.

## Usar o Agent Merge para gerenciar o PR

Com o código pronto, revise-o, crie o PR e deixe que o Agent Merge gerencie o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista e pressione <kbd>Enter</kbd>.
4. Crie um novo PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará a integração contínua do PR e fará o merge depois que tudo for aprovado.
7. Confirme que o Copilot exibiu uma mensagem semelhante a “PR #14 was squash-merged successfully”.

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branches, filas de merge, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e conclua manualmente a revisão e o merge quando o repositório permitir.

## Resumo e próxima etapa

Você automatizou várias partes do processo de desenvolvimento: geração de código, testes e validação e, agora, o processo do pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou todas as alterações do recurso e as evidências de verificação.
- criou o PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou o merge do PR.

Em seguida, você [explorará outros comandos de barra do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
