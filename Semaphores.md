# Semaphores

## Conceito:

Um semáforo é uma ferramente de sincronização usada para controlar o acesso a um dado recurso constituído por um número finito de instâncias.

- Definido pela primeira vez por Dijkstra no final dos anos 60s.

### Semáforos são como integers, except:
- Não é possível ler ou escrever valores, exceto para defini-lo inicialmente;
- Não utiliza valores negativos (quando um semáforo atinge o 0 todas as instâncias estão a ser usadas, signifcando que todos os processos que desejam usar o recurso ficam bloqueados até que o valor apresentado no semáforo seja maior que 0).

