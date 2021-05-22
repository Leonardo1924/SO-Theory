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

  - `Prevenção de DeadLocks` -> Métodos que garantem que pelo menus uma das quatro condições ncessárias não pode segura, limitando a forma como os pedidos de recursos podem ser feitos.
  - `Evitar DeadLock` -> Métodos exigem que o sistema operativo seja previamente informado, sobre quais os recursos que um processo ira utilizar durante a ua vida útil, de modo a decidir se o pedido de recursos consegue ser satisfeito ou se deve ser adiado.

Se o sistem a não implementa nenhum dos protocolos anteriores então é provável que ocorra o aparecimento de um deadlock:

  - Deadlock detection -> Método que examina o estado do sistema para determinar se ocorreu algum deadlock e fornece algoritmos para recuperar dos mesmos.

No caso de ausência de métodos para prevenir, evitar ou recuperar de deadlocks chegamos a uma situação em que o sistema se encontra num estado bloqueado e não tem forma nenhuma de reconhecer o que aconteceu.

- Undetected deadlocks -> podem causar a deteriorização da performance do sistema, uma vez que os recursos estão a ser retidos por processos que não podem correr e devido a acumulação de cada vez mais processos que necessitam dos recursos que se encontram bloqueados acabando esses por entrar também em deadlock( eventualmente o sistema irá parar de funcionar e será necessário reinicia-lo manualmente).
