---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar o contexto, selecionar modelos, compartilhar sessões e delegar tarefas opcionalmente."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como outras ferramentas de linha de comando, o GitHub Copilot CLI inclui vários comandos de barra. Eles dão acesso a recursos avançados, informações internas e opções de configuração adicionais. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usar `/model` para conhecer os modelos disponíveis.
- aprender como `/share` exporta ou compartilha uma sessão.
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação para o agente de nuvem.

## Cenário

O fluxo principal da CLI está concluído. Agora, você conhecerá outros recursos: gerenciamento de contexto, troca de modelo, compartilhamento de sessões e delegação opcional para o [agente de nuvem do Copilot][about-cloud-agent].

## Explorar o contexto do Copilot CLI

Em tarefas maiores ou mais complexas, você pode atingir o limite de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário, e você também pode inspecionar ou compactar o contexto com comandos de barra.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, se ele ainda não estiver aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare os resultados. Se a conversa ainda for curta, talvez não haja uma grande diferença.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente quando a janela fica cheia. Use `/compact` quando quiser escolher o momento. Use `/clear` ou `/new` ao mudar para uma tarefa sem relação e iniciar uma conversa limpa.

## Escolher um modelo

Os modelos têm pontos fortes diferentes e as pessoas desenvolvedoras também têm preferências diferentes. O Copilot CLI permite listar e selecionar o modelo que você quer usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhar uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todo mundo a aprimorar o uso de ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um gist no GitHub.

1. Digite `/help` e consulte as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar a sessão, escolha o destino adequado, como `/share file` para exportá-la como arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

A publicação de um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não queira compartilhar.

## Opcional: expandir ou delegar

O fluxo principal do workshop está concluído. O Copilot CLI também inclui comandos para tarefas maiores:

- `/fleet` pode dividir tarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar uma worktree isolada para outra tarefa.
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais, pois podem criar worktrees adicionais ou trabalho remoto. Comece com uma tarefa nova e bem delimitada e revise os resultados usando seu fluxo habitual. Para saber mais sobre o trabalho assíncrono com agentes, continue com o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próxima etapa

Os comandos de barra do Copilot CLI permitem configurá-lo, compartilhar sessões e entender o que ocorre nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto.
- usou `/model` para conhecer os modelos disponíveis.
- aprendeu como `/share` exporta ou compartilha sessões.
- explorou comandos opcionais para trabalhar em paralelo, usar worktrees e delegar tarefas ao agente de nuvem.

Há outros comandos de barra para explorar. Agora, vamos [revisar o que você aprendeu][next-lesson] e conhecer os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
