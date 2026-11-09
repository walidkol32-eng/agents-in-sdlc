---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica, deixe essa orientação disponível para o Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *que* código você quer, mas também *como* ele deve ser estruturado. Agora que você implementou a filtragem, explore as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos com escopo por caminho chegam ao agente.
- atualizar o arquivo de instruções para que os padrões de programação sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e as decisões que não sejam óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, os parâmetros e os valores retornados com TSDoc/JSDoc, incluindo o argumento `db` injetável quando houver.
- Os componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para executar tarefas de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Elas podem especificar convenções, bibliotecas e comentários preferidos. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações relevantes para a maioria das solicitações, como a stack, uma visão geral do projeto, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md`, arquivos que fornecem orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia de acordo com o ambiente. Consulte a [referência sobre compatibilidade com instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções personalizadas do projeto

O projeto inicial já inclui um conjunto de arquivos de instruções. Explore os arquivos existentes antes de alterá-los para observar o efeito das instruções.

1. Volte ao Codespace.
2. No editor do Codespaces, e não no terminal, abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de programação. Elas se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, para a camada de dados Drizzle, para testes e muito mais.
5. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no frontmatter, que define um glob para determinar a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e para arquivos existentes do projeto. Assim, conjuntos maiores de instruções podem ser divididos em arquivos menores e reutilizáveis, e o Copilot pode receber exemplos.

## Atualizar as instruções para corresponder às práticas da equipe

Os arquivos existentes são um bom começo, mas ainda falta uma orientação. Vamos modificar `copilot-instructions.md` para garantir que o novo código TypeScript inclua comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione o item a seguir como último tópico da seção:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Usar a orientação atualizada

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções, encerre e retome a conversa de filtragem para que o Copilot comece com a orientação atualizada sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para encerrá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório e na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique a orientação atualizada:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e revise os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme que descrevem o código corretamente.

## Resumo e próxima etapa

Você explorou como o Copilot CLI usa os arquivos de instruções do projeto e aplicou um novo padrão ao recurso. Nesta lição, você:

- explorou como as instruções do repositório e os arquivos com escopo por caminho chegam ao agente.
- atualizou o arquivo de instruções para que os padrões de programação sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Em seguida, você [personalizará e executará a skill reutilizável de verificações de qualidade][next-lesson] para executar testes e lint de maneira consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Compatibilidade com instruções personalizadas][custom-instructions-support]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — uma coleção de agentes e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
