# Memory Management

## Background:

A memória(ou memória principal) consiste num grande array de bytes cada um com o seu próprio endereço.
 - A unidade de memória vê apenas um fluxo de endereços de memória e não sabe como são gerados ou para que servem

Memória e Registos são o único tipo de armazenamento a que o CPU pode aceder diretamente.

- Os programas deve ser trazidos do disco para a memória para ser corridos
- O CPU, em seguida, recolhe as instruções da memória de acordo com o counter do programa.
- Estas instruções podem causar carregamento/armazenamento adicional de/para endereços de memória específicos

## Hardware Basico:

A maioria dos CPUs consegue descodifcar as instruções e realizar operações simples em registos à taxa de uma ou mais operações por CPU clock cycle

A memoria é acedida pelo memory bus o que pode levar algums clock cycles do CPU

Cache encontra-se entre a memoria e os registos do CPU 
  - Permite aumentar a velocidade de acesso a memóra 

Nota: Tudo o que foi mencionado anteriormente ja terá sido abordado na caderia de Arquitetura de Computadores

## Memory Space

`Uniprogramming`: 
- As aplicações correm sempre no mesmo local na memória fisica;
- As aplicações podem aceder a qualquer endereço fisico;
- Não é necessária a proteção da memória uma vez que apenas é executada um aplicação de cada vez.


`Multiprogramming`:
- Aplicações podem correr em diferentes localizações na memória fisica;
- Aplicações não podem aceder a todos os endereções físicos ( memória normalmente dividida em duas divisórias: 
 (i) sistema operativo, geralmente em memória baixa juntamente com o vetor de interrupt; 
 (ii) processos de utilizador, geralmente em alta memória);
 
 - Proteção da memória é necessária para prevenir o overlap de endereços entre processos
 
 ## Memory Protection
 
 A proteção da memória é necessária para garantir um funcionamento correto.
  - Proteger os processos do OS do acesso por processos de utilizador;
  - Proteger processos do utilizador uns dos outros.
 
 A proteção da memória deve ser fornecida pelo hardware.
 - Normalmente, o Sistema Operativo não intervém entre o CPU e os seus acessos à memória devido à consequente penalização de desempenho

Precisamos de garantir que cada processo tem um espaço separado na memória.
  - Separar o espaço de memória por processo protege os processos uns dos outros e é fundamental para ter múltiplos processos carregados na memória para execução simultânea
  - Para separar o espaço na memória, precisamos da capacidade de determinar o leque de endereços legais a que o processo pode aceder.

## Base and Limit Registers
 
 Um par de registos definem o espaço de endereço de um processo:
 - O registo base contem o menor endereço legal da memória fisica.
 - O registo limit especifica o tamanho.
 
 Os users não têm permissão para alterar os registos base/limite.
 
 Sem esta proteção, bugs em qualquer programa podiam levar ao crash de outros programas ou até mesmo do OS.
 
![imagem](https://user-images.githubusercontent.com/62023102/119243191-789ef200-bb5c-11eb-913f-f349e9e43554.png)


# Logical Vs Physical Address Space:

O conceito de um espaço de endereço lógico que está ligado a um espaço de endereço físco separado é central para uma gestão adequada de memória

- `Logical Address`: Endereço gerado pelo CPU;
- `Physical Address`: Endereço visto pela unidade de memória;
 
 O conjunto de todos os endereços lógicos gerados por um programa é o espaço de endereço lógico e o conjunto de todos os endereços físicos correspondentes a cada um dos endereços lógicos é o espaço de endereços físicos.

No tempo de compilação e de load, o "esquema" de ligação de endereços gera os mesmos endereços lógicos e físicos.

No tempo de execução, o "esquema" de ligação de endereços gera um diferente endereço lógico e físico.

# Memory Management Unit (MMU)

O dispositivo de hardware que no tempo de execução mapeia endereços lógicos para físicos é chamado de MMU (Memory-Management Unit).

- Existem diferentes métodos para completar tal mapeamento.

Para começar considera o seguinte esquema para mapeamento:

- O registo base agora é chamado de registo de deslocalização (relocation register)
- O valor nesse registo é adicionado a todos os endereços gerados por um user process no momento em que o endereço é enviado para a memória.


# Swapping 

## O que acontece se nem todos os processos tiverem espaço na memória?
  - Usa uma forma extrema de context switch onde algums processos em memória são temporariamente trocadas da memória para uma backing store.
  - Torna possivel para o espaço total de endereços fisicos de todos os processos exceder a verdadeira memória fisica do sistema.

`Backing store` -> Geralmente um disco rápido e grande o suficiente para acomodar cópias de todas as imagens de memória para todos os utilizadores.

- A read queue mantém os processos ready-to-run dos quais têm imagens de mémoria na backing store.

`Dispatcher`-> Verifica se um processo esta agendado para executar na mémoria.
  - Se não estiver, e se não houver memória livre nessa região, o dispatcher troca o processo atual em memória pelo processo desejado.
  -O tempo do Context-switch num sistema de swapping é bastante elevado.

 Maior parte do tempo de swap é na verdade tempo de transferência(temos de trocar em ambos out e in)
 - O tempo total de transferência é diretamente proporcional a quantidade de memória trocada.

![imagem](https://user-images.githubusercontent.com/62023102/119259021-22fe3000-bbc4-11eb-8b1c-e88ecd719ed9.png)

### Um processo trocado para a backing store precisa de ser colocado no mesmo endereço fisico de onde foi retirado?

- Não!! , se foi usada alocação dinâmica para mudar o relocation register.

### Pode um processo que esta a espera de uma operação de I/O ser swapped out?

 - Pode, se usarmos double buffering e a opereção de I/O for executada dentro de bufffers do kernel (transferências entre buffers do kernel e buffers de memória de processo, só ocorrem quando o processo é trocado back in.)

## Contiguous Memory Allocation

Alocação contígua de memória é um método inicial em que cada processo está contido numa única secção contígua de memória.

Um esquema simples é dividir a memória em varías partições de tamanho fixo.
 - Cada partição contém exatamente 1 processo.
 - O grau de multiprogramação está ligado pelo número de partições

A geralização é o esquema de partição variável

-  Cada partição contém na mesma um único processo mas o tamanho da partição é exatamente exato ao que o processo precisa.
-  O Kernel mantém uma tabela que indica que partes da memória estão ocupados e que partes estão disponíveis (memory holes).

## Variable-Partition Scheme

![imagem](https://user-images.githubusercontent.com/62023102/119263057-6b721980-bbd5-11eb-91b9-61bec1995950.png)


# Internal Fragmentation

Fragementação interna existe quando a memória alocado a um processo é ligeiramente maior do que memória solicitada.
 - A diferença de tamanhos esta relacionada com memória que não é usada que é interna a partição.

É o resultado de tentar evitar o overhead quando se mantém o rasto de pequenos buracos na memória.
  `Exemplo:`
   - Considera que tens um buraco na memória para 1000 bytes e o processo que está a pedir essa parte da memória necessita de 998 bytes, ficamos um um buraco mais pequeno que agora só tem espaço para 2 bytes -> é melhor ignorar esse pequeno buraco do que lidar com ele.
   - Um aproximação geral para evitar pequenos espaços na memória é separar a memória fisica em blocos de tamanho fixo é alocar unidades de memória baseado no tamanho do bloco

# External Fragmentation

Fragementação externa acontece quando existe espaço suficiente na memória para satisfazer o pedido de alocação no então o espaço disponível não é contíguo.
  - O armazenamento esta fragementado num número grande de pequenos buracos (small holes)
  - No pior caso , podemos ter sempre um espaço de dois em dois processos em toda a memória.
  
Uma possivel solução seria "baralhar"(shuffle) a memória para compactar toda a que está livre num único espaço.

- Só é possivel com alocação dinâmica, feita em tempo de execução, e nós estamos a usar double buffering para operações de I/O pendentes
- Pode ser muito dispendioso 

Outra solução possivel seria usar esquemas de alocação de memoria não contiguos.
 -`Segmentation` e `Paging` são dois tipos desse esquema.
 
 ## Programmer View of Memory
 
 A memória é vista como uma coleção de unidades lógicas separadas e de tamanho variável sem nenhuma ordem necessária entre elas.
  1. Main Program
  2. procedures, functions
  3. objects, methods
  4. local variables, global variables
  5. stack
  6. symbol table
  7. Arrays

![imagem](https://user-images.githubusercontent.com/62023102/119263882-cce7b780-bbd8-11eb-8764-35358562fbf5.png)

`Note` : Nos próximos capitulos falamos sobre os Esquemas de `Segmentation` e de `Paging`. Para depois entrar na parte da matéria referente a memória virtual.


