# MFQminix
Objetivo: modificar o escalonador padrão do minix para operar em Multilevel Feedback Queue (MFQ).

Para isso, as seguintes modificações foram feitas no código fonte:

- Adicionado um unsigned quantum no arquivo schedproc.h
em usr/src/servers/sched

- Criadas 3 filas adicionais para os níveis do MFQ no arquivo
config.h em /usr/src/include/minix/config.h. Agora há 19 filas.

As seguintes mudanças foram feitas nas funções declaradas no arquivo
schedule.c em /usr/src/servers/schedule.c.

- do_noquantum(): Para os processos entre as filas 16 e 18, aumente
a prioridade em 1 assim que seu quantum expirar. Os processos na fila
18 recebem 16 como sua nova prioridade. Para todas as outras filas,
utilize a política padrão.

- do_start_scheuling(): Inicialize o novo campo quantum junto com as
demais variáveis.

- do_nice(): Se não for possível designar uma nova prioridade para os
processos entre as filas 16 e 18, dê a eles sua prioridade anterior.
Adicione quantum nas variáveis para o rollback em caso de erro.

- balance_queues(): a cada rebalanceamento, aumente a prioridade
de todos os processos em 1.

Official MINIX sources - Automatically replicated from gerrit.minix3.org
