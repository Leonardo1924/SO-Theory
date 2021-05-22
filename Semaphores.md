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



