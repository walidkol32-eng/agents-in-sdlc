---
title: "Lição 0 - Pré-requisitos"
description: "Crie sua própria cópia do Tailspin Toys e prepare um GitHub Codespace para o workshop do Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de começar as lições do Copilot CLI, prepare o ambiente. Você criará uma cópia própria do repositório Tailspin Toys e iniciará um [Codespace][codespaces]. Na próxima lição, você usará o terminal integrado para instalar e executar o Copilot CLI.

Nesta lição, você vai:

- criar sua própria cópia do projeto Tailspin Toys a partir do template.
- criar um Codespace e confirmar que o projeto está pronto.

## Configurar o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório de template][tailspin-template]. O novo repositório contém todos os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [template do Tailspin Toys][tailspin-template].
2. Crie sua cópia do repositório selecionando **Use this template** e, em seguida, **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Preencha as configurações do repositório criado a partir do template](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (`organization-or-user-name/repository-name`), pois você precisará dele mais adiante no workshop.

> [!NOTE]
> **Seu backlog já está pronto**
>
> Quando você cria o repositório a partir do template, um backlog de issues do GitHub é criado automaticamente. Você trabalhará com essas issues durante o workshop — não é necessário criar nenhuma.

Use uma cópia nova do template do workshop. Ela inclui instruções do repositório, o código do aplicativo, testes, a skill `quality-checks` e o backlog que você usará. Se estiver usando uma cópia antiga, confirme com a pessoa que facilita o workshop se ela contém os arquivos necessários.

## Criar um codespace

Agora, você usará um Codespace para concluir o workshop.

O [GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um editor completo com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que você acabou de criar.
2. Selecione **Code**.

    ![Selecione o botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e, em seguida, **Create codespace on main**.

    ![Criar um novo Codespace](../../../_images/ex0-create-codespace.png)

4. Aguarde a conclusão da configuração do Codespace. O template instala as dependências do projeto, o Chromium do Playwright e o banco de dados local.
5. Se aparecer a pergunta **Do you trust the authors of the files in this folder?**, selecione **Trust Folder & Continue**.
6. Abra um terminal na raiz do repositório e inicie o aplicativo:

   ```bash
   npm run dev
   ```

7. Quando o Codespaces indicar que a porta `4321` está disponível, selecione **Open in Browser** e confirme que o site Tailspin Toys é carregado.
8. Volte ao terminal e pare o servidor de desenvolvimento com <kbd>Ctrl</kbd>+<kbd>C</kbd>.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumo e próxima etapa

Ambiente pronto! Nesta lição, você criou sua própria cópia do projeto Tailspin Toys a partir do template, criou um Codespace e confirmou que o projeto estava pronto.

Em seguida, você [instalará o GitHub Copilot CLI][next-lesson] no Codespace e fará a autenticação com sua conta do GitHub.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[tailspin-template]: https://github.com/github-samples/tailspin-toys
