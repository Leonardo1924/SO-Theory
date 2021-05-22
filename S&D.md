# Starvation and DeadLock

## Conceitos:

`Starvation`(indefinite blocking) corresponde a situaçãpo em que um processo aguarda indefinidamente por um evento que pode nunca ocorrer.

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



# Problemas Classicos de Sincronização 
Acho que o Moura não falou disto logo deixo aqui para os mais curiosos

 - [`Bounded Buffer Problem`](https://www.tutorialspoint.com/producer-consumer-problem-using-semaphores)
 - [`Readers and writers Problem`](https://www.tutorialspoint.com/readers-writers-problem)
 - [`Dining philosophers Problem`](https://www.tutorialspoint.com/dining-philosophers-problem-dpp)
