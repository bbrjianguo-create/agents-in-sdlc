---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] no GitHub Copilot permitem criar assistentes de IA especializados, adaptados a tarefas ou domínios específicos do seu fluxo de desenvolvimento. Ao definir agentes por meio de arquivos markdown na pasta `.github/agents` do seu repositório, você fornece ao Copilot instruções focadas, boas práticas, padrões de código e conhecimento de domínio que o orientam a executar determinados tipos de trabalho com mais eficácia. As equipes podem codificar sua experiência em agentes reutilizáveis: um agente de acessibilidade que reforça a conformidade com as [WCAG][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos por arquivos markdown na pasta `.github/agents` do seu projeto ou, de forma global, em `~/.copilot/agents`. Cada arquivo tem um frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt em markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados em comparação com skills de agente

Há alguma sobreposição lógica entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente com arquivos markdown e dizem a uma IA como executar operações. A forma mais clara de separá-los: um **agente personalizado** é o trabalhador, e as **skills** são ferramentas.

Os agentes personalizados têm a própria janela de contexto e foram criados para orquestrar skills (e até outros agentes) como parte do trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade; nesse trabalho, ele poderia chamar skills como uma skill de fluxo de pull request ou uma que executa e gerencia testes.

> [!NOTE]
> Não existe uma única forma "certa" de criar um agente personalizado. Como em tudo que envolve IA, teste e itere para encontrar o que funciona para seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em garantir que sua plataforma de financiamento coletivo seja acessível a todos os usuários, independentemente de suas habilidades ou preferências visuais. Feedbacks recentes de usuários destacaram que alguns acham o tema escuro atual difícil de ler por causa do contraste insuficiente entre o texto e as cores de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é essencial, você quer implementar isso o mais rápido possível. Você usará um agente personalizado para gerar a funcionalidade.

Neste exercício, você irá:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao seu codespace, abra um terminal e mude para uma nova branch a partir da `main` para o trabalho de acessibilidade (você manterá o PR de filtragem do Exercício 3 separado):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Em seguida, percorra e revise as próximas seções, que destacam:
   - Responsabilidades principais ao gerar código para um site acessível.
   - Boas práticas de acessibilidade.
   - Exemplos de código em HTML, CSS e JavaScript.
   - Uma lista de armadilhas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` na lista suspensa de agentes, na parte inferior da visualização do Copilot Chat. Você pode selecionar um agente personalizado para restringir uma sessão de chat às instruções e ferramentas desse agente.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção de modo e de modelo varia de acordo com o exercício; cada etapa indica isso quando for relevante.

1. Selecione **Agent** na lista suspensa de agentes na visualização de Chat, caso ainda não esteja selecionado.

   ![Captura de tela mostrando o seletor de agentes na visualização de Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione a lista suspensa de agentes na parte inferior da visualização de chat (ela mostra o agente ativo, que por padrão é **default**).
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie o prompt a seguir ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará a trabalhar: ele explorará a base de código, proporá alterações e aplicará edições nos arquivos do projeto. Cada edição aparece em linha no chat, com o caminho do arquivo e um diff que você pode revisar.
6. Conforme as edições são aplicadas, o indicador **Files changed** é atualizado para que você veja o conjunto de trabalho que o agente modificou.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot está fazendo alterações reais no seu repositório: ele editará arquivos existentes, como os componentes Astro, o CSS e quaisquer testes relacionados.

Você revisará e direcionará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Esta lição explorou os [agentes personalizados][custom-agents] no GitHub Copilot, assistentes de IA especializados, adaptados a tarefas e domínios específicos. Com eles, você pode codificar a experiência e os padrões da sua equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficácia.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- usar um agente personalizado no modo de agente do Copilot Chat.

A seguir, você [monitorará e direcionará o trabalho do agente][next-lesson], revisando as alterações conforme acontecem e adicionando um alternador de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criando agentes personalizados no seu IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testando seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorando e gerenciando agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
