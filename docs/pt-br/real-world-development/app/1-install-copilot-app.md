---
title: "Lição 1 - Instalar o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento com agentes. Criado com base no GitHub Copilot CLI e integrado nativamente ao GitHub, ele permite usar repositórios, branches e pipelines de integração contínua (CI) sem configuração adicional. O aplicativo foi projetado para fluxos de trabalho em que você orienta vários agentes em paralelo, cada um em seu próprio espaço de trabalho isolado, em vez de fazer todo o trabalho por conta própria, e automatiza tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, instale o aplicativo e conecte o repositório.

Nesta lição, você irá:

- instalar o aplicativo GitHub Copilot e entrar na sua conta;
- adicionar o projeto ao aplicativo usando o repositório do GitHub;
- conhecer o espaço de trabalho, incluindo o backlog criado pelo modelo;
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e fazer merge de pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instalar e configurar o aplicativo GitHub Copilot

O primeiro passo para usar o aplicativo GitHub Copilot é instalá-lo. Há versões para Windows, macOS e Linux. Instale o aplicativo, autentique-se e adicione o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo após a instalação.
4. Selecione **Sign in to GitHub** e siga as instruções para se autenticar. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe seu host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo inicial oferecer a seleção de repositórios, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode ignorar essa etapa e adicionar o projeto depois.
6. Quando solicitado, escolha um tema e conclua a configuração inicial.
7. Procure seu repositório em **Sessions** na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Use o nome do proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme que o repositório aparece na barra lateral. No layout usado aqui, pastas de repositório, como `tailspin-toys`, ficam agrupadas em **Sessions**, com sessões individuais sob cada repositório.

## Conhecer o espaço de trabalho

Com o projeto conectado, conheça as áreas da barra lateral:

- **New**: inicie uma nova sessão de chat com o Copilot.
- **Pull requests**: acesse seus pull requests por meio da integração nativa com o GitHub. Você pode navegar e filtrar pull requests, verificar o status da CI e revisá-los sem sair do aplicativo.
- **Issues**: acesse suas issues da mesma forma. Você pode navegar e filtrar issues e iniciar uma sessão a partir de uma issue.
- **Automations**: gerencie tarefas de agente salvas que são executadas em uma agenda ou sob demanda. Elas são úteis para listas de tarefas, manutenção regular do projeto e outras atividades repetitivas que você quer delegar. A seção de revisão e próximos passos traz links para esse recurso; ele não é outro exercício do workshop.
- **Customize**: adicione recursos ao aplicativo Copilot, como servidores do Model Context Protocol (MCP), plugins, skills e outros componentes. Você usará esta área para configurar o MCP do Playwright.
- **Chats**: use conversas leves para perguntas e brainstorming que não precisam de uma branch nem de um espaço de trabalho próprio. Esta seção aparece depois que você inicia seu primeiro chat. Se não estiver visível, selecione **+** ao lado de **Sessions** para iniciar um chat.
- **Sessions**: encontre pastas de repositório e as sessões de agente correspondentes. Selecione um nome de repositório, como `tailspin-toys`, para abrir a visualização do repositório, ou selecione uma sessão abaixo dele para voltar à conversa. Cada sessão é executada em um espaço de trabalho isolado, então você pode executar várias ao mesmo tempo sem que as alterações entrem em conflito. Você iniciará sua primeira sessão ao adicionar avaliações por estrelas.

Ao longo do workshop, você explorará o espaço de trabalho.

> [!TIP]
> Na dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se algo é possível, pergunte ao Copilot para receber orientações.

### Localizar o backlog criado pelo modelo

Explore o backlog criado automaticamente quando você criou o repositório a partir do modelo.

1. Em **Sessions** na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual dentro dela. Em seguida, selecione **Issues** na navegação superior do repositório.
2. Localize estas issues pelo título, sem presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de fazer uma primeira alteração rápida.

> [!NOTE]
> As listas de **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione-os ao aplicativo.

## Experimentar um chat rápido

Uma ótima maneira de conhecer o aplicativo é usá-lo para aprender sobre o *próprio aplicativo*. Um **chat rápido** é ideal para isso: você pode fazer perguntas ou trocar ideias sem criar uma branch nem um worktree, sem precisar de uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um chat novo. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois.
2. Pergunte como as sessões do aplicativo funcionam:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na visualização da conversa. Cada sessão é executada em seu próprio worktree Git isolado, o que permite executar vários agentes em paralelo sem que suas alterações entrem em conflito. Você pode continuar a conversa ou iniciar outro chat a qualquer momento.

## Resumo e próximos passos

Parabéns, você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Você:

- instalou o aplicativo e entrou na sua conta do GitHub;
- adicionou um projeto usando o repositório do GitHub;
- conheceu o espaço de trabalho e localizou o backlog criado em **Issues**;
- usou um chat rápido para fazer uma pergunta breve e descartável.

Em seguida, [inicie sua primeira sessão de agente][next-lesson] e use-a para exibir uma avaliação por estrelas nos cartões de jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
