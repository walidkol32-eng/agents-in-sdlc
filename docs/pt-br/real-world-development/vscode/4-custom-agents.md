---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` do repositório podem oferecer instruções, boas práticas, padrões de programação e conhecimento do domínio para orientar o Copilot. As equipes podem transformar sua experiência em agentes reutilizáveis: um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com, no mínimo, os campos `name` e `description`, seguidos de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Há uma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA a executar tarefas. Uma forma simples de diferenciá-los: o **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Agentes personalizados têm sua própria janela de contexto e coordenam skills (e até outros agentes) para executar o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Para isso, poderá chamar skills, como uma skill para o fluxo de pull requests ou para executar e gerenciar testes.

> [!NOTE]
> Não há uma única forma correta de criar um agente personalizado. Como em qualquer trabalho com IA, teste e itere para encontrar o que funciona no seu ambiente e nos seus cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que todas as pessoas possam usar sua plataforma de financiamento coletivo, independentemente da visão ou das preferências visuais. Comentários de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design pediu a implementação de um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você quer implementar a funcionalidade rapidamente. Para isso, usará um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Revise o conteúdo para entender como ele orientará o Copilot.

Volte ao Codespace, abra um terminal e crie uma branch a partir de `main` para trabalhar nos recursos de acessibilidade. Assim, o PR de filtragem do Exercício 3 permanecerá separado:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - boas práticas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra os agentes personalizados definidos em `.github/agents` no seletor de agentes, na parte inferior da visualização Copilot Chat. Você pode selecionar um agente para que a sessão use as instruções e ferramentas dele.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam em cada exercício; as etapas indicarão as opções relevantes.

1. Selecione **Agent** no seletor de agentes da visualização Chat, caso ainda não esteja selecionado.

    ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Abra o seletor de agentes na parte inferior da visualização Chat. Ele mostra o agente ativo — por padrão, **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie o prompt a seguir ao agente de acessibilidade:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. Conforme as edições forem feitas, o indicador **Files changed** será atualizado para mostrar os arquivos modificados pelo agente.

> [!NOTE]
> Esse processo pode levar alguns minutos. O Copilot fará alterações reais no repositório — editará componentes Astro, CSS e testes relacionados, se houver.

Na próxima lição, você revisará e orientará o trabalho em andamento.

## Resumo e próxima etapa

Nesta lição, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com eles, é possível transformar a experiência e os padrões da equipe em orientações reutilizáveis.

Você aprendeu:

- como os agentes personalizados são definidos.
- a usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que são feitas e adicionando um controle para o modo claro à mesma sessão.

## Recursos

- [Sobre os agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
