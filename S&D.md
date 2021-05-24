# Starvation and DeadLock

## Conceitos:

`Starvation`(indefinite blocking) corresponde a situação em que um processo aguarda indefinidamente por um evento que pode nunca ocorrer.

`DeadLock`é uma situação em que um conjunto de processos está à espera indefinidamente de um evento que nunca irá ocoorer porque esse evento só pode ser causado por um dos processos em espera nesse conjunto.

DeadLock -> Starvation não a acontece o vice-versa.
  - Starvation pode acabar (apesar de não ter de o fazer) vimos num dos modulos anterior uma possivel solução para este problema;
  - DeadLock não termina sem intervenção externa.


## DeadLock

DeadLocks ocorrem quando estamos a aceder a multiplos recursos.
  - Não podes resolver um deadlock por cada recurso de forma independente.

DeadLocks não são determinísticos e nem sempre acontecem no mesmo pedçado de código
  - Tem de ser exatamente no momento certo (ou momento errado?)
  
  ![imagem](https://user-images.githubusercontent.com/62023102/119237446-be47c480-bb34-11eb-922f-16ae37c2202a.png)

Acho que o Moura não falou deste proxima parte nas aulas logo deixo aqui para os mais curiosos

# Problemas Classicos de Sincronização 

 - [`Bounded Buffer Problem`](https://www.tutorialspoint.com/producer-consumer-problem-using-semaphores)
 - [`Readers and writers Problem`](https://www.tutorialspoint.com/readers-writers-problem)
 - [`Dining philosophers Problem`](https://www.tutorialspoint.com/dining-philosophers-problem-dpp)

## Caracterização do DeadLock

DeadLock só podem surgir se as próximas 4 condiçioes se mantiverem simultaneamente:

  1.`Mutual exclusion`-> Apenas um processo de cada vez pode utilizar um recurso (um processo de pedido deve ser adiado até que o recurso seja libertado);
  
  2.`Hold and wait` -> Um processo que detém (pelo menos) um recurso está à espera para adquirir recursos adicionais detidos por outros processos.
  
  3.`No preemption` -> um recurso não pode ser antecipado, ou seja, um recurso só pode ser libertado voluntariamente pelo processo que o mantém, depois desse processo ter concluído a sua tarefa.
  
  4.`Circular wait` -> Existe um conjunto de processos de espera{P1,P2,...,PN} de tal forma que P1 está à espera de um recurso detido pelo P2, que por sua vez esta a espera de um recurso detido por P3 e assim sucessivamente até chegar a PN que esta retido porque precisa de um recurso do P1.

## Handling DeadLocks

Para garantir que deadlocks nunca ocorem, o sistema pode usar um protocolo para prevenir ou para evitar deadlocks.

  - `Prevenção de DeadLocks` -> Métodos que garantem que pelo menos uma das quatro condições ncessárias não pode segura, limitando a forma como os pedidos de recursos podem ser feitos.
  - `Evitar DeadLock` -> Métodos exigem que o sistema operativo seja previamente informado, sobre quais os recursos que um processo ira utilizar durante a ua vida útil, de modo a decidir se o pedido de recursos consegue ser satisfeito ou se deve ser adiado.

Se o sistem a não implementa nenhum dos protocolos anteriores então é provável que ocorra o aparecimento de um deadlock:

  - `Deadlock detection` -> Método que examina o estado do sistema para determinar se ocorreu algum deadlock e fornece algoritmos para recuperar dos mesmos.

No caso de ausência de métodos para prevenir, evitar ou recuperar de deadlocks chegamos a uma situação em que o sistema se encontra num estado bloqueado e não tem forma nenhuma de reconhecer o que aconteceu.

- `Undetected deadlocks` -> podem causar a deteriorização da performance do sistema, uma vez que os recursos estão a ser retidos por processos que não podem correr e devido a acumulação de cada vez mais e mais processos, que necessitam dos recursos que se encontram bloqueados acabando esses por entrar também em deadlock( eventualmente o sistema irá parar de funcionar e será necessário reinicia-lo manualmente).

## DeadLock Prevention

`Prevenir mutual exclusion` -> tentar evitar recursos non-sharable (cada recurso é tornado sharable e assim um processo nunca precisa de esperar por qualquer recurso).

- `Problema`: Não é muito realista, uma vez que alguns recursos são intrinsecamente non-sharable.

`Prevenir hold and wait` -> Garantir que sempre que um processo solicita recursos, não detém quaisquer outros.

  - Exige que o processo solicite todos os recursos antes da execução começar (prever o futuro é dificil, tende-se a sobreestimar recursos)
  - Alternativamente, requer que processo liberte todos os recursos que possui antes de solicitar recursos adicionais.

 - `Problema`: Pouca utilização de recursos e starvation são possiveis.

`Prevenir no-preemption`-> libertar recursos voluntáriamente.

- Se o processo P falha ao alocar alguns recursos, então libertamos todos os recursos que P esta a utilizar e apenas quando P ganhar novamente os antigos e os novos recursos é que podemos reiniciar o processo.
- Alternativamente,  se o processo P falha a alocar alguns recursos, verifcamos se esses recursos estão alocados a um outro processo Q que esteja a espera de recursos adicionais e antecipamos os recursos desejados de Q e alocamo-los para o processo P.

`Problema`: Geralmente não se pode aplicar a recursos como semáforos.

`Preventing circular wait` -> Impor uma "encomenda" total de todos os tipos de recursos e exigir que cada processo solicite recursos numa ordem crescente de enumeração.

`Problema`: Progamadores podem escrever programas sem seguir esse tipo de ordem.

## DeadLock Avoidance

Os métodos para evitar deadlock requer informação adicional a priori, sobre quais os recursos que um processo utilizará durante a sua vida útil.

  - Métodos mais simples é útis requerem que cada processo declare o número máximo de recursos de cada tipo que possa precisar.

Quando um processo solicita um recurso disponível, temos de decidir se a sua alocação imediata deixa o sistema num estado seguro

- Safe state => no deadlock
- Unsafe stata => possiblidade de deadlock

![imagem](https://user-images.githubusercontent.com/62023102/119239317-38317b00-bb40-11eb-988b-42e9c9a10a4b.png)


## Recovey from DeadLock

Para recuperar de um deadlock, podemos aborta processos utilizando dois métodos:

- `Abortar todos os processos em deadlocked` -> O resultado de cálculos parciais feitos por tais processos são perdidos e provavelmente terão de ser refeitos.

- `Abortar um processo de cada vez até que o ciclo de deadlock seja eliminado`-> Incorre em sobrecargas consideráveis, uma vez que o algoritmo de deteção de deadlocks deve ser invocado após cada processo ser abortado.

### Por que ordem devemos decidir abortar?

 1. Prioridade do processo;
 2. Quanto tempo o processo demorou a correr e a concluir;
 3. Recursos que o processo usou;
 4. Recursos que o processo precisa para completar;
 5. Quantos processos terão de ser terminados.
 
 Em alternativa, para recuperar de um deadlock, podemos sucessivamente antecipar alguns recursos dos processos e dá-los a outros até que o ciclo do deadlock seja quebrado.
 
#### Três questões têm de ser abordadas

- `Selecting a victim` -> tentar minimizar os custos tais como o número de recursos que um processo em deadlock possui e a quantidade de tempo que o processo tem consumido até agora.
- `Rollback` -> Devolver o processo a um estado de segurança (safe state) e reiniciá-lo a partir desse estado. 
- `Starvation` -> O mesmo processo pode ser sempre escolhido como vítima levando ao estado de starvation e, portanto , a solução mais comum é incluir o número de rollbacks no fator de custo.
 

