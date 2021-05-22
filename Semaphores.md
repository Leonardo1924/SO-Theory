# Semaphores

## Conceito:

Um semáforo é uma ferramente de sincronização usada para controlar o acesso a um dado recurso constituído por um número finito de instâncias.

- Definido pela primeira vez por Dijkstra no final dos anos 60s.

### Semáforos são como integers, except:
- Não é possível ler ou escrever valores, exceto para defini-lo inicialmente;
- Não utiliza valores negativos (quando um semáforo atinge o 0 todas as instâncias estão a ser usadas, signifcando que todos os processos que desejam usar o recurso ficam bloqueados até que o valor apresentado no semáforo seja maior que 0).

#### Existem 2 tipos de semáforos:

- `Counting semaphore` -> Pode variar sobre um domínio sem restrições.
- `Binary semaphore` -> Que só pode varia entre 0 e 1.

## Operações com Semáforos:

Semáforos são acedidos atrás de duas operações atómicas padrão:
- `Wait()` -> Espera que o semáforo se torne positivo e depois decrementa-o por 1.
- `Signal()` -> Operação que incrementa o smáforo por 1.

```c
wait(semaphore S){
  while ( S == 0); //busy waiting
  S--;
  }
  
  signal(semaphore S){
    S++;
   }
```

# Semaphores Implementation

A implementação deve garantir que não são executados ao mesmo tempo 2 wait() e/ou signal().
  - Operações de Wait() simultâneas não podem descer para valores abaixo de zero.
  - É possivél perder um incremento do sinal() se a operação de espera acontecer ao simultaneamente.

Novamente, existem duas maneiras diferentes para minimizar o tempo de espera.
- Através de uniprocessadores através de disabling interrupts 
- Através de multiprocessadores através de disabling interrupts + instruções atomicas.

## Uniprocessor Semaphore Implementation:

```c
typedef struct {
  int value; //semaphore's value
  PCB *queue; //associated queue of waiting processes
 } semaphore;
 
 init_semaphore(semaphore S){
    S.value = 0;
    S.queue = EMPTY;
   }
   
   wait(semaphore S){
     //disable interrupts
     if(S.value == 0){
        //avoid busy waiting
        add_to_queue(current PCB, S.queue);
        suspend();
        // kernel reenables interrupts just before restarting here
       } 
       else {
         S.value--;
       //enable interrupts;
      }
    }
    
    signal(semaphore S){
    //disable interrupts;
    if(S.queue != Empty){
      //keep semaphore's value and wakeup one waiting process
      PCB = remove_from_queue(S.queue);
      add_to_queue(PCB, ready queue);
     }
     else {
      S.value++;
     }
     //enable interrupts;
    }
```
    
## MultiProcessor Semaphore Implementation

 ```c
 // semaphore data struct;
 typedef struct{
    boolean guard; // to guarentee atomicity
    int value;     // semaphores's value
    PCB *queue;    // associated queue of waiting processes
   } semaphore;

  init_semaphore(semaphore S){
    S.guard = false;
    S.value = 0;
    S.queue = Empty;
   }
   
   wait(semaphore S){
    //disable interrupts;
    while (test_and_set(&S.guard)); //short busy waiting time
    if (S.value == 0){
      add_to_queue(current PCB, S.queue);
      S.guard = false;
      suspend();
      // kernel reenables interrupts just before restating here
     } 
     else {
          S.value--;
          S.guard = false;
         // enable interrupts;
      }
   }
   
   signal(semaphore S){
      //disable interrupts;
      while(test_and_set(&S.guard)); //short busy waiting time
      if(S.queue != Empty){
        //keep semaphore's value and wakeup one waiting process
        PCB = remove_from_queue(S.queue);
        add_to_queue(PCB, read queue);
       }
       else{
        S.value ++;
       }
       S.guard = false;
      // enable interrupts;
     }
 ```
 
 No proximo capitulo vamos estudar Starvation and Deadlock...
