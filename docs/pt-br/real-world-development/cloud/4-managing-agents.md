---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse Mission Control, você pode ver todas as tarefas do agente com pull requests (PRs abertos), explorar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar outra funcionalidade.

## Cenário

Depois que a equipe atribuiu ao agente a criação de um modo de alto contraste, percebeu que seria um bom momento para adicionar também um modo claro. Como o agente já estava atualizando o estilo do site e adicionando controles de alternância, pareceu lógico incluir esse recurso. Você orientará o agente para garantir que ele implemente também o modo claro.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos conferir o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas nos painéis principal e esquerdo. Você deverá encontrar as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações já realizadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados para facilitar o monitoramento.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que inclua um controle de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente ao modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no Mission Control](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece. Isso será indicado por mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa do Copilot destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot confirmar o prompt e incluí-lo no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot executará a tarefa atualizada. Ele incorporará o novo pedido ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso levará alguns minutos; aproveite a pausa para refletir sobre o que aprendeu até aqui.

## Resumo e próximos passos

Neste exercício, exploramos a página de agentes do Copilot, o espaço central para monitorar e orientar as tarefas do agente de nuvem do GitHub Copilot. Nela, você acompanha tarefas ativas e concluídas, revisa o trabalho e redireciona tarefas em andamento para ajustar o escopo ou fornecer orientações.

Você explorou:

- como usar o Mission Control e a página de agentes para monitorar tarefas do agente de nuvem.
- como redirecionar uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot conclui os recursos de acessibilidade, vamos [iterar sobre os pull requests criados por ele][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
