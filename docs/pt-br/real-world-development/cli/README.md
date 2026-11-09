---
slug: pt-br/real-world-development/cli
title: "CLI do GitHub Copilot"
description: "Crie, verifique e entregue duas mudanças no Tailspin Toys enquanto aprende sobre modos, personalizações, ferramentas MCP e automação de pull requests no Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O **[GitHub Copilot CLI][about-copilot-cli]** coloca o GitHub Copilot no terminal como um assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela linha de comando, para você manter o foco sem alternar para um editor gráfico.

Neste workshop, você seguirá um fluxo contínuo de trabalho no Tailspin Toys:

1. Prepare o projeto no GitHub Codespaces, instale o Copilot CLI e conheça o ambiente.
2. Faça uma alteração focada na classificação por estrelas, revise-a no navegador e mescle manualmente seu primeiro pull request (PR).
3. Comece pela issue de filtragem, defina a abordagem no modo Plan, implemente no modo Autopilot e revise no modo Interactive.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill existente `quality-checks` e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem no navegador.
7. Crie um agente personalizado de garantia de qualidade (QA) para revisar requisitos, cobertura e evidências de verificação.
8. Revise a alteração completa de filtragem e use o Agent Merge no PR correspondente.
9. Explore comandos de barra úteis para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

Para manter o workshop focado, você criará dois PRs: primeiro, o de classificação por estrelas; depois, o de filtragem, que incluirá as atualizações de instruções, da skill, do perfil de QA e dos testes. O fluxo de filtragem e qualidade usa a mesma conversa e branch para que você possa aproveitar o trabalho anterior ao explorar cada ferramenta.

## Lições

| Lição | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI |
| [2. Adicionar classificações por estrelas: uma vitória rápida][ex2] | Primeira alteração | Exiba as classificações existentes e a indicação para valores nulos; depois, mescle seu primeiro PR |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planeje o recurso a partir da issue, implemente com Autopilot e revise no modo Interactive |
| [4. Orientar o Copilot com instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções e aplique-as à filtragem |
| [5. Personalizar e usar uma skill de verificações de qualidade][ex5] | Verificações repetíveis | Explore a skill existente, altere o formato do relatório e execute-a |
| [6. Validar funcionalidades com o MCP do Playwright][ex6] | Observação no navegador | Configure o MCP na CLI e inspecione o comportamento da filtragem |
| [7. Criar e usar um agente de QA][ex7] | Requisitos e cobertura | Crie e selecione um perfil especialista e reúna as evidências finais de verificação |
| [8. Criar e mesclar o PR do recurso][ex8] | Revisão e merge | Revise a alteração completa, crie o PR e use o Agent Merge |
| [9. Comandos de barra no GitHub Copilot CLI][ex9] | Recursos da CLI | Explore contexto, modelos, compartilhamento e delegação opcional para o agente de nuvem |
| [10. Encerramento e próximos passos][ex10] | Resumo | Revise o fluxo, as personalizações reutilizáveis e outros recursos |
| [Opcional: Incorpore o Foundry][foundry] | Agentes hospedados | Prepare um modelo, crie e implante um Backer Concierge baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Familiaridade básica com operações de terminal e linha de comando
- [ ] Permissão para criar um repositório e um Codespace

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][student-plan]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, verifique se o administrador habilitou o Copilot CLI para uso.

## Começar

**[Comece pelos pré-requisitos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
