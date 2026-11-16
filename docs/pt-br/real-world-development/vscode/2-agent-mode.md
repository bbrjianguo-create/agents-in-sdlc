---
title: "Exercício 2 - Adicionando novas funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até a atualização mais simples de um aplicativo costuma exigir alterações em vários arquivos e a execução de operações, como testes. Como desenvolvedor, seu fluxo normalmente envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, fazer outra atualização... e a lista não para.

É aqui que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para agir de forma mais autônoma no seu IDE. Ele se comporta de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os erros encontrados. Vamos explorar como você pode usar o modo de agente para introduzir novas funcionalidades no seu site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem foram criados para dois tipos diferentes de experiência. O modo de agente executa suas tarefas no IDE, permitindo ciclos de feedback rápidos e interação. O agente de nuvem foi projetado para ser um par de programação, trabalhando de forma assíncrona como um membro da equipe e interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar seu projeto, identificar os arquivos relevantes e fazer alterações coordenadas.
- o modo de agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de fazer o merge na sua base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e distribuidora. Você já adicionou um helper de distribuidoras no exercício anterior e agora concluirá o restante do trabalho na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Executar o site da Tailspin Toys

Antes de fazer qualquer alteração, vamos explorar o site da Tailspin Toys para entender sua funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza suas páginas como HTML estático no momento do build. As páginas consultam um banco de dados SQLite local diretamente no frontmatter por meio do Drizzle ORM; não há uma API de back-end separada nem um framework de interface no cliente. Os helpers de acesso a dados reutilizáveis ficam em `src/lib/`, e qualquer interatividade é adicionada com um pequeno `<script>` do Astro, de escopo restrito, usando APIs padrão do DOM.

### Iniciar o site

Para facilitar a execução do site, foi fornecida uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento do Astro. Você pode executá-la no seu GitHub Codespace com as etapas a seguir:

1. Volte ao seu codespace. Você deve estar de volta na `main` após o exercício anterior.
2. Abra uma nova janela de terminal dentro do Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma nova branch para este trabalho de filtragem e mude para ela:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando a seguir para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento do Astro estiver pronto, você deverá ver um banner indicando a URL, semelhante ao exemplo abaixo:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo for aberta pedindo que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Ao usar um codespace, selecionar um link de URL localhost no terminal do Codespace redireciona você automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o seu codespace, que agora está hospedando seu servidor web!

### Explorar o site

Com o site em execução, você pode explorar sua funcionalidade. Os principais recursos do site incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você é levado a uma página de detalhes com mais informações sobre ele, incluindo título, descrição, distribuidora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção de modo e de modelo varia de acordo com o exercício; cada etapa indica isso quando for relevante.

A implementação inicial do site é funcional, mas queremos aprimorá-la adicionando novos recursos. Vamos começar revisando o backlog. Quando você configurou seu projeto a partir do modelo, um backlog de issues foi criado automaticamente; peça ao GitHub Copilot que mostre esses itens.

1. Selecione **Agent** na lista suspensa de agentes na visualização de Chat. O agente **Agent** planeja e implementa alterações de forma autônoma em vários arquivos, executa comandos de terminal e invoca ferramentas.

   ![Captura de tela mostrando o seletor de agentes na visualização de Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não indicam preferência por um modelo ou outro. Ao criar este workshop, usamos o Claude Sonnet 4.5 e, por isso, ele aparece nas instruções. A expectativa é que as sugestões de código que você receber sejam relativamente consistentes, para garantir uma boa experiência. No entanto, como os LLMs são probabilísticos, você pode notar que as sugestões recebidas diferem do que é indicado no workshop. Isso é perfeitamente normal e esperado.

> [!NOTE]
> Por causa da natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deve conseguir concluir a tarefa.

3. Pergunte ao Copilot sobre o backlog de issues enviando o prompt a seguir:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe que o Copilot até priorizou os itens para você, com base nos que ele considera mais úteis para o usuário.

## Revisar os arquivos de instruções

Antes de iniciar o agente para gerar o código, é um bom momento para revisar o arquivo de instruções que você usará para fornecer contexto ao Copilot durante o trabalho dele. Você aproveitará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contém contexto sobre como abordar a adição de funcionalidades ao site.

1. No seu codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe a orientação geral sobre como abordar a adição de funcionalidades. Ela inclui:
   - Uma visão geral da arquitetura.
   - Princípios de design de componentes, testabilidade e acessibilidade.
   - Links para arquivos de instruções específicos para vários tipos de arquivo, incluindo:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Os arquivos de instruções permitem referenciar tanto outros arquivos de instruções quanto arquivos do seu projeto. Os caminhos são relativos à localização do arquivo de instruções. Isso permite a reutilização, a divisão de instruções complexas em partes menores e mais gerenciáveis e o fornecimento de exemplos e modelos.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, será preciso fazer pelo menos três atualizações separadas no aplicativo:

- Adicionar ou refinar a lógica de filtragem restante na camada de dados (`src/lib/`)
- Adicionar ou atualizar testes para o comportamento de filtragem
- Atualizar a página de listagem de jogos para introduzir a interface de filtragem

Além disso, os testes precisam ser executados (e passar) antes de você fazer o merge de tudo na sua base de código. O modo de agente do Copilot pode executar essas tarefas por você! Vamos adicionar a funcionalidade.

1. Você pode continuar na conversa atual com o Copilot ou iniciar uma nova selecionando **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela mostrando um exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista suspensa de agentes na visualização de Chat.

   ![Captura de tela mostrando o seletor de agentes na visualização de Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que o **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do seu backlog usando o prompt a seguir:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot começar explorando o projeto e localizando os arquivos associados à funcionalidade desejada. Você deve vê-lo encontrar tanto os helpers da camada de dados e a interface quanto os testes. Em seguida, ele começa a modificar os arquivos e a executar os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Você notará que o Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante esse processo, você pode notar que os botões **Keep** e **Undo** aparecem no editor de código. Quando o Copilot terminar, haverá um **Keep** ou **Undo** para todas as alterações, portanto você não precisa selecioná-los enquanto o trabalho está em andamento.

7. Quando o Copilot solicitar, selecione **Continue** para executar os testes.

   ![Captura de tela mostrando uma caixa de diálogo no painel do Copilot Chat pedindo ao usuário que confirme que concorda em executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Você pode notar algumas pausas e até ver alguns testes falharem durante o processo. Tudo bem! O Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar nenhum erro.

   ![Captura de tela mostrando uma sessão de chat completa com o modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado em busca de possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador com o site em execução. Explore a nova funcionalidade!
11. Depois de confirmar que tudo funciona e de revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar novos recursos ao site da Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados.
- o modo de agente do Copilot pode explorar seu projeto, identificar os arquivos relevantes e fazer alterações coordenadas.
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de fazer o merge na sua base de código.

Agora vamos [testar seu recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request para ele.

### Exercício bônus de exploração – Implementar paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Usando as habilidades que você aprendeu neste exercício, peça ao Copilot que atualize o site para implementar a paginação. Algumas considerações para o código incluem:

- seguir as melhores práticas existentes, incluindo o uso dos arquivos de instruções já existentes.
- considerar como você quer que a paginação seja implementada: se quer permitir que o usuário selecione o tamanho da página ou se ele deve ser fixo no código.
- ao criar o prompt, fornecer ao Copilot a orientação necessária para criar a implementação da forma desejada.
- talvez seja necessário iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot!

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usá-los][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
