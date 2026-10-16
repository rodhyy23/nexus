# Evolução do Nexus

Este registro acompanha melhorias e problemas tratados no desenvolvimento do Nexus.
Ele é uma síntese sanitizada para o portfólio: o código operacional, os dados de
origem e o histórico privado não são publicados aqui. A data de uma entrada indica
o registro da evolução, sem representar lançamento ou aprovação para produção.

## 16/10/2026 — preservação e seleção de fichas

Gerar uma ficha novamente podia sobrescrever a versão anterior. Agora, a primeira
geração conserva o nome habitual e as seguintes recebem sufixos numéricos, sem
substituir arquivos existentes, inclusive quando há disputa pelo mesmo nome.

| Situação | Comportamento verificado |
| --- | --- |
| Interrupção durante a gravação | O arquivo final só aparece depois da conclusão e da conferência do conteúdo |
| Atualização indisponível da referência à ficha atual | A referência anterior é preservada; uma nova ficha já concluída continua disponível para recuperação |
| Seleção de pasta, arquivo inválido, temporário do Excel ou planilha sem estrutura de ficha | A seleção é recusada antes da troca |
| Falha ao vincular a ficha à operação | A ficha anterior e seu contexto são preservados |
| Falha visual após uma seleção já concluída | A seleção é mantida, com aviso e orientação para retomar antes de executar |
| Nova geração após reconciliação ou arquivamento | O fluxo orienta retomar a ficha existente |

Foram acrescentados **32 casos de regressão**, totalizando **55 casos de
resiliência aprovados** nas duas etapas. Os testes usam arquivos sintéticos,
estado temporário e interface Qt offscreen, sem executar integrações operacionais.

A suíte completa isolada terminou com **798 aprovações, 7 falhas, 43 casos
ignorados e 9 erros**. Não surgiram falhas novas na execução final; permanecem
as divergências e limitações de ambiente documentadas na primeira auditoria.
A suíte completa ainda não possui aprovação integral.

As gravações da referência à ficha atual são coordenadas entre threads do mesmo
processo. Trava entre instâncias, recuperação de estado danificado e garantias
de durabilidade diante de queda de energia permanecem fora desta etapa.

[Detalhes das proteções e limites →](RESILIENCE.md)

## 16/10/2026 — consolidação e primeira rodada de resiliência

Foi consolidado o trabalho local acumulado no Nexus Principal e na Automação,
incluindo testes, documentação e scripts. A interface oficial permanece em
PySide6; componentes históricos retirados deixam de fazer parte da árvore ativa.

A rodada de resiliência investigou situações comuns de uso acidental e condições
menos frequentes envolvendo processamento assíncrono.

| Problema observado | Comportamento após a correção |
| --- | --- |
| Vários pedidos da mesma ação eram enfileirados enquanto uma tarefa rodava | A coordenação da interface aceita uma tarefa interna por vez e recusa os pedidos sobrepostos |
| Uma atualização do guia podia reativar o botão durante o processamento | O botão respeita o estado ocupado até a entrega do resultado à interface |
| Era possível trocar a operação ou a ficha durante uma tarefa | Controles de contexto e edição ficam bloqueados durante o processamento |
| Um timer podia iniciar uma tarefa enquanto uma confirmação estava aberta | O estado é verificado novamente depois da confirmação e antes da alteração |
| Falhas ao iniciar a tarefa ou atualizar a tela escapavam do tratamento previsto | A interface apresenta diagnóstico; a tarefa não é repetida automaticamente |
| A janela podia fechar com trabalho interno em andamento | O fechamento normal aguarda a entrega do resultado da tarefa |
| Uma reconciliação automática podia se perder quando havia outra tarefa ativa | O monitor conserva a atualização pendente e aguarda a liberação |

A revisão também removeu configurações locais do rastreamento corrente e substituiu
nomes pessoais encontrados em novas fixtures por personagens fictícios. Os arquivos
locais foram preservados. O histórico operacional anterior não foi enviado ao
portfólio público.

### Evidências e limites

- Foram acrescentados **23 casos de regressão de resiliência**, com interface Qt
  offscreen, estado temporário e cenários sintéticos.
- Os testes incluem 50 disparos consecutivos, cliques rápidos, fechamento repetido,
  tarefas lentas, sinais atrasados, erros de callback e reconciliação adiada.
- A validação focada ampliada terminou com **246 testes aprovados**, incluindo
  módulos da Automação e fixtures sanitizadas.
- A execução completa isolada da auditoria terminou com **766 aprovações, 7
  falhas, 43 casos ignorados e 9 erros**. Parte dos casos depende de navegador
  local ou tentou usar caminhos de estado padrão, bloqueados pelo isolamento.
  Também existem divergências de expectativa que exigem investigação própria.

Esses resultados delimitam o que foi validado; não certificam prontidão para
produção. A execução completa ocorreu antes da sanitização final das fixtures;
a rodada focada ampliada verificou essas alterações antes do commit local.

### Pendências prioritárias

A preservação de fichas existentes foi tratada na segunda etapa, registrada acima.
As prioridades seguintes são:

1. Recuperar preferências inválidas e diagnosticar estado local danificado.
2. Impedir concorrência entre instâncias e entre fluxos externos da mesma operação.
3. Reconciliar resultados incertos após interrupções, antes de permitir um reenvio.
4. Investigar as divergências de regressão sem alterar regras de negócio por suposição.

[Detalhes das proteções e do que ainda falta →](RESILIENCE.md)

## Como este registro é mantido

Cada publicação deve explicar o problema, a alteração observável, as evidências de
validação e as limitações conhecidas. O registro público é revisado separadamente
dos commits locais. Credenciais, dados pessoais, endereços internos, relatórios
reais e regras proprietárias permanecem fora desta edição.

[Política de segurança e publicação →](../SECURITY.md)
