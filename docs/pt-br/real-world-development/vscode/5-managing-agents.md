---
title: "Exercício 5 - Monitorando e gerenciando agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora sua base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina em tempo real, o Copilot Chat no VS Code oferece uma visão contínua de cada chamada de ferramenta e de cada edição de arquivo à medida que ocorrem. Você pode revisar cada diff em linha, aceitar ou rejeitar alterações individuais e direcionar a conversa com prompts de acompanhamento para refinar ou estender o trabalho sem sair do editor.

Neste exercício, você irá:

- monitorar o trabalho do agente de acessibilidade em tempo real.
- revisar e aceitar as edições de arquivo propostas.
- direcionar o agente no meio da sessão para adicionar um alternador de modo claro junto com o alto contraste.

## Cenário

O agente personalizado de acessibilidade que você usou no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design voltou com um pedido adicional: eles também gostariam de um alternador de modo claro para usuários que acham o tema padrão escuro demais. Em vez de iniciar uma nova conversa do zero, você direcionará a sessão existente para estender o trabalho.

## Monitorar o progresso do Copilot

A visualização do Copilot Chat mostra cada etapa que o agente executa: os arquivos que ele explora, os comandos que executa e as edições que propõe. Cada edição de arquivo aparece em linha no chat com um diff e controles **Keep** / **Undo**.

1. Volte à visualização do Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, role o chat para acompanhar cada chamada de ferramenta. Você verá leituras de arquivos, edições de arquivos e quaisquer comandos de terminal que ele pedir para executar.
3. Quando o agente propõe uma edição de arquivo, o diff aparece em linha:
    - Selecione **Keep** para aceitar a alteração.
    - Selecione **Undo** para reverter a alteração e deixar o agente fazer outra tentativa.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver o conjunto completo de arquivos que o agente modificou durante esta sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando de terminal), o agente fará uma pausa e exibirá um prompt **Allow**. Aprove apenas os comandos com os quais você se sente confortável.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Você deverá ver uma mensagem final resumindo as alterações feitas.

## Direcionar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você estenderá a mesma conversa para adicionar um alternador de modo claro. Como você está continuando a mesma sessão de chat, o agente mantém todo o contexto do trabalho anterior, incluindo os componentes que alterou, a abordagem de estilo que usou e o padrão de local storage que configurou.

1. Na entrada do Copilot Chat, envie o prompt de acompanhamento a seguir:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente retomará de onde parou, explorando as alterações que já fez e propondo acréscimos para o modo claro.
3. Revise cada nova edição à medida que ela chega, como você fez para o alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção que você não quer (por exemplo, tentar refatorar a interface dos alternadores de um jeito de que você não gosta), você pode interrompê-lo com o botão **Stop** ao lado da entrada do chat e depois enviar uma mensagem de esclarecimento.

> [!TIP]
> Direcionar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplify the CSS — use CSS variables instead of duplicating selectors"), corrigir o rumo ("don't add a third toggle, integrate light mode into the existing settings panel") ou ampliar o escopo ("also add keyboard shortcuts for both toggles").

## Revisar o conjunto de trabalho

Antes de fazer o commit, dê uma olhada rápida em tudo que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em quaisquer testes relacionados.
3. Abra alguns dos arquivos alterados e percorra os diffs. Confirme que os padrões de acessibilidade do agente personalizado estão refletidos: atributos ARIA, navegação por teclado, HTML semântico e persistência por local storage.
4. Quando estiver satisfeito, faça o stage e o commit das alterações pelo painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e direcionar uma sessão em andamento: revisou cada edição de arquivo à medida que ela chegava, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para estender o trabalho sem perder o contexto.

Nesta lição, você explorou:

- monitorar o trabalho de um agente em tempo real.
- aceitar e rejeitar edições individuais de arquivos.
- direcionar uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará seu trabalho de acessibilidade como um pull request e revisará as alterações que o agente personalizado local produziu ao longo deste laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usando o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterando no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
