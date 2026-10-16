# Resiliência diante de ações repetidas e interrupções

Uma aplicação operacional precisa lidar com cliques repetidos, mudanças de contexto
e falhas intermediárias sem depender de uma sequência perfeita de ações do usuário.
Esta página descreve a primeira rodada de proteções do Nexus, registrada em
16/10/2026, em termos genéricos e sem dados do ambiente de origem.

## Uma tarefa interna por vez

Um executor com uma única worker ainda pode acumular vários pedidos iguais.
Por isso, a interface agora verifica seu estado antes de submeter uma tarefa.
A proteção permanece ativa até o resultado ser recebido pela interface, mesmo
quando a worker já terminou.

O editor fica somente leitura e os controles que trocam a operação ou a ficha
ficam indisponíveis durante esse intervalo. Uma atualização do guia também
respeita esse estado.

## Confirmações e tarefas assíncronas

Um diálogo modal continua processando eventos. Um timer pode iniciar trabalho
enquanto o usuário lê a confirmação. A interface verifica novamente o estado
depois da resposta, antes de efetuar uma mudança de contexto.

A conclusão de uma etapa pode iniciar legitimamente a etapa seguinte. Esse
encadeamento é preservado; a proteção passa a acompanhar a nova tarefa.

## Falhas e encerramento

Falhas na submissão e na atualização da tela são tratadas. Se a etapa terminou,
mas a tela não conseguiu refletir o resultado, o diagnóstico orienta a conferir
a operação antes de repetir a ação. Não há repetição automática da tarefa.

O fechamento normal da janela aguarda tarefas internas em andamento. Isso não
impede encerramento forçado do processo, queda de energia ou travamento de uma
dependência. Também não existe cancelamento forçado de threads.

Monitores que precisam reconciliar o término de um processo externo aguardam
quando outra tarefa interna está ativa, preservando a atualização pendente.

## O caso das teclas repetidas

Na janela Qt testada, 50 eventos F5 não dispararam uma tarefa. Isso não demonstra
o comportamento de uma aplicação web externa nem explica incidentes anteriores.
Recarregamentos durante gravações precisam de testes próprios em ambiente seguro
e reconsulta do resultado antes de qualquer novo envio.

## Cobertura e próximos passos

Os 23 novos casos de regressão usam dados sintéticos e estado temporário. A
validação focada ampliada passou com 246 testes. A suíte completa isolada ainda
possui falhas e limitações descritas no [registro de evolução](EVOLUTION.md).

A proteção desta rodada cobre a coordenação interna da interface. Exclusão entre
processos, preservação de arquivos existentes, recuperação de estado danificado
e interrupções com resultado incerto permanecem como trabalho futuro.

[Voltar à apresentação →](../README.md)
