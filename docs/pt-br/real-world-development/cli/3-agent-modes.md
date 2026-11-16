---
title: "Lição 3 - Modos do agente: Plan e Autopilot"
description: "Use o modo Plan para combinar uma abordagem, o Autopilot para criar a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Mas alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para funcionar com o fluxo já existente de uma organização, garantindo que construamos as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes: começar de uma issue para gerar um novo recurso, garantir que o código seja válido, verificar se o recurso se comporta como esperado e, por fim, fazer o merge dele no projeto com sucesso.

> [!NOTE]
> Você usará a mesma conversa e a mesma branch ao longo do fluxo deste recurso. Normalmente, haveria branches ou PRs diferentes para os diferentes tipos de arquivo em que você trabalharia, mas vamos usar um atalho para manter o foco nos conceitos principais.

Para começar, nesta lição, você vai:

- iniciar uma nova conversa com o Copilot a partir de uma issue do GitHub.
- definir requisitos no modo Plan.
- implementar o novo recurso usando o modo Autopilot.
- revisar o código.
- validar o recurso manualmente em um navegador com porta encaminhada.

Ao continuar este recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam restringir os jogos por categoria e distribuidora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam de acordo antes da codificação. Você usará o modo Plan para resolver essas decisões e, depois, autorizará uma implementação limitada com o Autopilot.

## Contexto

Introduzir agentes de programação com IA no seu fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! A maioria dos desenvolvedores segue um fluxo parecido com este:

1. Começar de uma issue registrada que descreve o que precisa ser feito.
2. Criar um plano do que precisa ser construído.
3. Construir e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois que o código for revisado e o processo de integração contínua for concluído com sucesso, fazer o merge do código.

> [!NOTE]
> Dependendo da sua equipe e da sua organização, os detalhes variam. Mas a maioria será uma variação do tema listado acima.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos definidos e passe pelo mesmo processo de verificação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o quanto de autonomia o agente tem. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua resposta antes de prosseguir.
- **Plan**: o agente cria primeiro um plano e fica impedido de editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escrevendo código, executando testes e iterando até concluir a tarefa.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Começar pela issue

Antes de iniciar o trabalho de filtragem, volte ao seu Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao seu Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR da avaliação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize a `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O `#` permite trazer para o contexto uma issue ou um pull request (PR) do GitHub.

## Planejar o recurso de filtragem

O planejamento dá a você a chance de definir a abordagem para implementar um recurso ou realizar tarefas antes de entregá-los ao Copilot. Para qualquer coisa complexa, sempre vale a pena dedicar um pouco de tempo ao planejamento. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador de modo abaixo do prompt exibe **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o prompt a seguir:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começa a elaborar o plano! Ele inicia explorando o projeto e, em seguida, determina a melhor abordagem.

3. Durante o processo, o Copilot pode fazer perguntas sobre como a filtragem deve funcionar. Responda de acordo com suas preferências. Não há respostas erradas!
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role para cima e para baixo para revisar o plano.
6. Peça ao Copilot que revise qualquer parte do plano que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo! Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot itere sobre o problema até considerar que ele está resolvido.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com rótulo semelhante na sua versão instalada.
2. Confirme que o indicador de modo abaixo do prompt exibe **Autopilot**.
3. Acompanhe o Copilot percorrendo o plano estabelecido, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot relatar dependências ausentes ou um conflito de porta, resolva o problema de configuração antes de tratar as verificações como concluídas.

## Revisar e verificar a implementação

Depois que o código é gerado, ele precisa ser revisado antes do merge, como qualquer outro código. Vamos revisar o código e executar o site para garantir que tudo esteja correto.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador de modo não exibe mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões que você tomou durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise a saída das verificações do projeto e peça ao Copilot que resolva eventuais falhas.

## Explorar a nova funcionalidade

Certo, o código parece bom, mas ele funciona? Vamos iniciar o aplicativo como fizemos antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Experimente a filtragem por categoria, a filtragem por distribuidora e as combinações que você definiu no plano.
4. Confirme que o comportamento de redefinição e de resultado vazio corresponde à issue e às suas decisões.
5. Volte ao seu Codespace e peça ao Copilot que interrompa o servidor de desenvolvimento que ele iniciou.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma nova conversa com o Copilot a partir de uma issue do GitHub.
- definiu requisitos no modo Plan.
- implementou o novo recurso usando o modo Autopilot.
- revisou o código.
- validou o recurso manualmente em um navegador com porta encaminhada.

Em seguida, vamos nos aprofundar em como o código é gerado, garantindo que ele siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
