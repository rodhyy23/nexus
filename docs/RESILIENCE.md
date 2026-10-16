# Resiliência diante de ações repetidas e interrupções

Uma aplicação operacional precisa lidar com cliques repetidos, mudanças de contexto
e falhas intermediárias sem depender de uma sequência perfeita de ações do usuário.
Esta página descreve duas etapas de proteção do Nexus, registradas em
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

## Preservação e seleção de fichas

Uma geração nova recebe um nome exclusivo quando já existe uma ficha com o nome
habitual. O conteúdo é gravado em um temporário e conferido antes da publicação,
que recusa substituir arquivos existentes. A versão anterior permanece intacta.

A referência à ficha atual é atualizada de forma atômica, com coordenação entre
threads do mesmo processo. Se essa atualização falhar, a referência anterior é
mantida. Uma ficha já concluída permanece disponível para recuperação.

A seleção verifica se o arquivo é uma ficha legível antes de trocar o contexto.
Arquivos vazios, inválidos, temporários do Excel e planilhas sem estrutura de ficha
são recusados. A conferência estrutural complementa as validações operacionais.

Falhas de vínculo preservam a seleção anterior. Quando a seleção já foi concluída
e apenas sua atualização visual falha, o aviso informa o resultado e orienta a
retomar a operação antes de executar. Não há repetição automática.

A regeneração preliminar atualiza o vínculo sem retroceder a etapa da operação.
Se já houver uma ficha reconciliada ou arquivada, o fluxo exige retomar a versão
existente, evitando referências conflitantes.

## Cobertura e próximos passos

As duas etapas somam **55 casos de regressão de resiliência aprovados**: 23 da
coordenação da interface e 32 da preservação e seleção de fichas. A execução
completa isolada terminou com **798 aprovações, 7 falhas, 43 casos ignorados e
9 erros**, mantendo as pendências anteriores. Consulte o
[registro de evolução](EVOLUTION.md) para conhecer as evidências e os limites.

Exclusão entre processos, recuperação de preferências e estado danificados,
limites para entradas muito grandes ou maliciosas e interrupções com resultado
incerto permanecem como trabalho futuro. A concorrência testada nesta etapa usa
quatro threads; ela não representa quatro aplicações completas. Não há garantia
geral de durabilidade diante de queda de energia.

[Voltar à apresentação →](../README.md)
