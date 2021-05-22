# Escalonamento de Processos 

## Motivação 

O objetivo de multiprogramming é ter processos a correr o tempo todo, o que maximiza a utilização do CPU.
- Quando um processo tem de esperar, o sistema operativo retira esse processo do CPU e dá-lhe outro processo.

Uma função fundamental de um sistema operativo, que por acaso também é o básico de multiprogramming, é o `Escalonamento de Processos`.
- Ao escalonar eficientemente o CPU entre vários processos, o sistema operativo pode cobrir mais tarefas e tornar o sistema mais produtivo.

# CPU-I/O Burst Cycle

A execução de um processo pode parecer um ciclo de execução do CPU e tempos de espera e I/O.
![imagem](https://user-images.githubusercontent.com/62023102/119228002-afe3b380-bb08-11eb-8cfc-0d6bff072d0a.png)


# Decisões de Escalonamento:

Decisões de Escalonamento pode ocorrer quando um processo:
 1. Troca do estado de running para o estado de waiting (como resultado de um I/O request);
 2. Troca do estado de waiting para o estado de ready (como resultado de um I/O completion);
 3. Troca do estado de running para o estado de ready (como o resuktado de um interrupt);
 4. Simplesmente termina.

O escalonamento escolhe entre todos os processos na Lista-Ready e aloca o CPU para um deles.
 - Quando uma decisão de escalonamento ocorre tendo em conta o ponto 1 e 4 dizemos que o escalonamento é nonpreemptive ( ou coperativo);
 - De outra forma, o escalonamento é preemptive.

Escalonamento do tipo preemptive requer especial hardware como por exemplo um timer.

# Escalonamento Preemptive

O escalonamento preemptive pode resultar em condições de corrida (isto é , o output depende da sequência de execução de outros eventos incontroláveis)

  - Enquanto que um processo esta a atualizar dados, o mesmo é temporariamente interrumpido para que um segundo processo possa correr. O segundo processo pode tentar ler os mesmos dados, que podem estar num estado inconsistente.

- O processo de uma system call pode involver a mudança de alguns dados importantes ao nivel do kernel. Se o processo for temporariamente interrumpido (preempted) no meio destas mudanças e o kernel precisar de ler ou modificar o mesma estrutura, então vais estar perante um CAOS. 

Uma vez que interrupções podem ocorrer a qualquer momento, essas secções de codigo deve ser guardadas de acessos concorrentes por vários processos e para que essas interrupções sejam desativadas ao entrar nessas secções e apenas reativadas à saida.

# Critérios de Escalonamento

Foram surgeridos varios critérios para comparar algoritmos de escalonamento entre os quais se destacam:

- `Utilização do CPU` -> Manter o CPU o mais ocupado possivel;
- `Throughput` -> número de processos que completa a execução por unidade de tempo;
- `TurnAround/Completion Time` -> Quantidade de tempo necessário para executar um processo ( intervalo de tempo desde a submissão e o tempo de conclusão);
- `Tempo de espera` -> Quantidade de tempo que o processo tem de esperar na ready queue;
- `Tempo de resposta` -> Quantidade de tempo que demora desde de que o pedido é submetido até a primeira resposta (not output) ser produzida.

Critérios de otimização:
  - Maximizar a utilização do CPU E do throughput;
  - Minimizar o completion time, o tempo de espera e o tempo de respsota.

# Algoritmo First-Come First-Served (FCFS)

O processo que pede o CPU primeiro é o primeiro a ter o CPU alocado.
  - Facil de gerir com uma FIFO queue.
  - Quando um processo entra na ready queue é linkado a tail da queue.
  - Quando o CPU estiver livre, é alocado para o processo que esta na head da ready queue (o processo é removido da queue).

FCFS é nonpreemptive, uma vez que o CPU foi alocado a um processo, esse processo controla o CPU até que o mesmo termine ou necessite de algo de I/O.

 - A media de `TurnAround` e tempo de espera e muitas vezes bastante longa.
 - Problemático para os sistema de partilha de tempo, onde é importante que cada utilizador obtenha uma parte do CPU em intervalos regulares (seria um inferno permitir que um processo mantivesse o CPU por um período prolongado).

## Exemplo:

![imagem](https://user-images.githubusercontent.com/62023102/119229054-f851a000-bb0d-11eb-9481-10a263c9b2fd.png)

![imagem](https://user-images.githubusercontent.com/62023102/119229067-07385280-bb0e-11eb-94de-e0ccea78f288.png)

FCFS -> Média do tempo de espera.

 - (0 + 24 + 27) / 3 = 17 

![imagem](https://user-images.githubusercontent.com/62023102/119229112-46ff3a00-bb0e-11eb-92d7-9913e9870153.png)

FCFS -> Media do tempo de espera 

 - (0 + 3 + 6) / 3 = 3 

# Algoritmo Round Robin (RR) 

Parecido com o FCFS mas com preemption, sendo desenhado para sistemas com time-sharing:
- Cada processo ganha um tempo quantum ou uma "fatia" de tempo (pequena unidade do tempo do CPU);
- O temporizador(Timer) interrompe cada quantum para agendar o próximo processo, o processo atual é antecipado e adicionado ao final da ready queue (Circular system);

Se o tempo quantum for Q e houverem N processos na ready queue, então quanda processo recebe 1/N do tempo do CPU em pedações de no maximo Q (nenhum processo espera mais do que (N-1)* Q unidades de tempo).

 - Se o Q é grande -> FCFS;
 - Se o Q for pequeno -> O overhead de mudanças de contexto pode ser muito alto degradando os níveis de utilização do CPU.

## Round Robin (RR) com Q = 4 ;

![imagem](https://user-images.githubusercontent.com/62023102/119230533-83ce2f80-bb14-11eb-824f-e7e8b609d69d.png)

RR -> Tempo Médio de Espera

- (6 + 4 + 7) / 3 = 17/3 = 5.66
