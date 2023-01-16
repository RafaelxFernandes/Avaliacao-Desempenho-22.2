> Feedback professor:
- Mudar para 3300 rodadas e 1000 amostras
- Cálculo de E[N1], E[Nq1], E[N2] e E[Nq2] estão errados (para todos os rhos)
- Chi-Quadrado deveria dar precisão de 5% (0.05)
- Plotar médias chi-quadrado
- Para cada plot, explicar como cada eixo foi obtido
- Explicar detalhadamente/ deduzir como cada cálculo foi realizado
- Analisar para 10 seeds
- Acrescentar correlação de Pearson na explicação das seeds

- Otimização:
    - Explicar "cada passo"
    - V[W2] não deveria ser a métrica
    - Valor de clientes atendidos está errado
    - Nossos fatores mínimos não garantem 5% de IC

- Plot de E[N1] para seed 13 deveria ser:
    - rho = 0.2 => 0.1111
    - rho = 0.4 => 0.2500
    - rho = 0.6 => 0.4286
    - rho = 0.8 => 0.6667
    - rho = 0.9 => 0.8182

- Introdução:
    - Explicar para qual fila o número de amostras se refere
    - As métricas tem que ser salvas referente à rodada atual, e não à rodada do cliente
    - Em qual intervalo estão as seeds do Python?
    - As estatísticas E[N] e E[Nq] não são comentadas. Nas chegadas e términos de serviço os valores das métricas tem que ser calculados

- Feedback geral: fregueses chegam durante uma rodada ativa e são coloridos com a cor da rodada. Quando métricas são coletadas, o freguês que originou a métrica tem que ser da mesma cor da rodada atual ou a métrica é descartada. Fregeueses continuam no sistema normalmente e são sempre computados nas métricas que envolvem número de fregueses no sistema. Quando rodada nova começa, uma nova cor os fregueses presentes de outras cores são estado inicial para a nova rodada. Na implementação, tenho a impressão que estes conceitos não estão seguidos à risca, seguindo o português do texto. Há que checar a implementação e verificar o código. Como há erro nos cálculos de E[N] e E[Nq] para as duas filas, deve haver um erro conceitual.
    - Não está claro o que determina a fila da rodada: se coletas na fila 1, se coletas na fila 2
    - Quando uma rodada é determinada, as estatísticas da rodada são dos fregueses servidos durante a rodada ativa. Após o término da rodada, fregueses da cor da rodada que acabou de terminar permanecem no sistema, mas suas estatísticas não são mais usadas após o término da rodada. Checar isso
    - Cálculo de E[N] e E[Nq]: nada comentado e há erro nisso, ou simplesmente usaram E[W] e E[T]. Todavia isso não deveria ser assim. Os resultados dos cálculos da simualação guardam as métricas e elas estariam aproximadamente satisfazendo Little. Fácil ver que isso não foi observado nos resultados, demonstrando que E[N] e E[Nq] deem ter sido de fato calculados entre erro de implementação

- Email:
Os resultados envolvendo N e Nq, tanto da fila 1 como da fila 2, estão errados. As deduções dos valores analíticos para as métricas das médias para as duas filas devem estar no trabalho e V(W1) que pode ser obtido também. Para as variâncias o número mínimo de rodadas para uma precisão de 5% no IC de 95% é da ordem de 3300, como temos na aula 7 e apostila.
Para esta métrica, há portanto um cuidado especial, mas para as outras o IC de 5% de precisão pode ser obtido com menor esforço e com uma fase transiente menor. Isso pode levar a um fator mínimo menor.
Uma rodada deve terminar quando o número de coletas da fila 2 atingir o especificado. Neste instante, a rodada termina e uma nova rodada se inicia, com nova cor. As métricas que foram computadas até o instante do término da rodada são as que devem ser utilizadas, ainda que tenhamos no sistema fregueses ainda por serem servidos e com a cor da rodada que terminou. Quando um freguês desses for servido na fila 2, as métricas dele não serão computadas, pois estamos numa rodada de nova cor. Isso significa que o final de uma rodada é não determinado pelo número de chegadas que teremos para uma rodada como vocês estão fazendo . 
O cálculo de E[N] e E[Nq] nunca foi explicado como foi feito e isso tem que estar correto. Vocês irão computar E[W] e verão que E[Nq] aprox lambda x E[W]. Fácil ver que Little não está sendo observado nos resultados esperados.
Tudo sobre corretude tem que seguir o que foi discutido em aula e na apostila. É preciso bolar chegadas determinísticas e com alguns pouco eventos calcular na mãos os valores e verificar se o simulador calcula corretamente as métricas. Isso é essencial. Tem que usar sequências determinísticas simples para que o cálculo seja verificado. Tem que mostrar na mãos o que deve ser esperado e depois apresentar a saída do simulador comprovando o que foi obtido. Isso é corretude e é essencial que tudo esteja de acordo. Vocês não podem rodar simplesmente o simulador e mostrar que os valores analíticos estão comprovando o correto funcionamento do sistema, pois nem sempre teremos valores analíticos para isso. Mas caso haja discrepância com os valores analíticos, então isso é um alerta. Veja que nos resultados os valores analíticos têm que estar dentro dos ICs. Não adianta ter um IC de 5% de precisão mas com o valor analítico fora dele. No caso de V(W2) não há um valor analítico dedutível e sugiro que obtenha um valor com número grande de rodadas de tamanho também grande que garanta um valor o mais próximo possível do real e usar então este valor como referência quando for diminuindo o tamanho das rodadas e da fase transiente para ainda verificar se este valor está dentro do IC de 5% calculado.
No relatório de vcs aparece na fase transiente V(N) e V(Nq) que não são métricas solicitadas e não é indicado como estas métricas foram calculadas. Há que complementar e ser completo
Parece que os cálculos incrementais não foram realizados, o que poderia ser verificado na implementação para obtenção das métricas sem guardar logs, apenas acumulando os somatórios e somatórios dos quadrados. Vide notas de aula..
Há que ter um relatório preciso e completo, sem erros grosseiros.
