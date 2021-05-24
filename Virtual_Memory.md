# Virtual Memory:

## Background 

O código precisa de estar em memória para executar, mas raramente se usa programa inteiro.

- Código para lidar com situações fora do comum é raramente executado.
- Estruturas de dados grandes costuma alocar mais memória do que a que realmente precisam.
- Mesmo que todo o programa seja usado, não é todo necessário ao mesmo tempo.

`Virtual Memory` é uma técnica que permite a execução de processos que não estão totalmente em memória.

 - Resumo a memória principal num extremamente grande e uniforme array de armazenamento.
 - Liberta os programadores de preocupações como as limitações da armazenamento em memória.

Executar um procesos que não se encontra totalmente em memória têm beneficios para o utilizador e para o Sistema Operativo
- Permite menos uso da memória
- Permite a criação mais eficiente de projetos
- Menor uso de operações de I/O necessárias para o load ou swaps de processos para a memória
- Mais programas podem correr de forma concorrente
- O espaço dos endereços lógicos podem ser mais largos que os espaços dos endereços físicos.

### From Virtual to Physical Memory

![imagem](https://user-images.githubusercontent.com/62023102/119268848-ab90c680-bbec-11eb-83ff-cad4d82fc9d3.png)

## Demand Pagin 

Acho que o Moura também não falou disto.

`90-10 rule`: Os programas gastam 90% do seu tempo em 10% do seu código

`Demand Pagin` é uma técnica que carrega uma page em memória apenas quando esta é necessária.

- Evita uso desnecessário de I/O
- Menos uso de mémoria e menos uso de I/O
- Resposta mais rápida
- Paginas que nunca são acedidas nunca são carregas na memória física (normalmente essas páginas residem numa memória secondária).

## Virtual Address Space of a process

Refere-se a visão lógica (virtual) de como um processo é guardado em memória.

Típicamente, um processo começa num determinado endereço lógico -> digamos, endereço 0 -> e existe em memória contígua até que o endereço lógico mais alto permitido.

## Page Fault

A `page fault` ocorre quando o hardware de paging acede a uma pagina marcada como invalida.
  - Na tradução do endereço pela page table, o paging hardware irá notar que um bit inválido foi coloca, causando uma trap(exception/fault) no sistema operativo.

O procedimento para resolver uma page faul é simples:

1. Verifique o PCB para determinar se a referênica era um acesso legal ou ilegal à memória (se ilegal terminar o processo).

2.Encontra uma free frame e trás a pagina em falta para esse frame (o carregamento é feito da backing store).

3.Reset a page table para indicar que a página esta agora em mémoria.

4.Repete a instrução que causou o page fault.

![imagem](https://user-images.githubusercontent.com/62023102/119269439-9e290b80-bbef-11eb-8498-c642c0268da8.png)

## O que acontece se não existem nenhuma free frame?

Pode acontecer de não exister uma free frame para carregar a página pretendida, nesta caso:
  - O sistema operativo podia dar swap out a um processo, libertando todos os frames que esse processo ocupava.
  - A solução mais comum é `page replacement` ( encontra alguma página que não esta a ser usada e liberta-a).

`Page replacement` completa a sepação entre memória logica e memória fisica
- Um grande memória virtual pode ser fornecida numa memória física mais pequena.
- A mesma página pode ser levada para a memória varias vezes

Queremos um algoritmo de page replacement que resulte num número mínimo de page faults

### Como funciona o algoritmo de Page Replacement:

1. Seleciona a victim page.
2. Escreve essa pagina em disco
3. Trás a pagina desejada para um novo espaço livre.
4. Update da page table.

`Nota`: A transfêrencia de duas paginas é necessária (one out || one in)

## FIFO Page Replacement

É um exemplo de algoritmo para Page Replacement 
FIFO (first-in first-out)
A página que vai ser substituida é a que lá esta a mais tempo.

A FIFO queue é usada para seguir a idade das páginas
- Quando uma página é trazida da memória é inserida na cauda de lista.
- E a página que se encontra na cabeça da lista é a próxima a ser substituida.

![imagem](https://user-images.githubusercontent.com/62023102/119269898-eea16880-bbf1-11eb-9ad7-c4fd0a746484.png)

## LRU Page Replacement

LRU (Least recently used) algoritmo

A página que vai ser substituida é aquele que não é usada durante um maior longo período de tempo.

- LRU associa a cada página o tempo do seu ultimo uso.
- Geralmente é um bom algoritmo (melhor que FIFO).

[HOW TO IMPLEMENT LRU PAGE REPLACEMENT ALGORITHM](https://www.geeksforgeeks.org/program-for-least-recently-used-lru-page-replacement-algorithm/)


## Second Chance Page Replacement

Este algoritmo substitui uma página antiga mas não necessáriamente a mais antiga.
 - É semelhante ao FIFO mas usa um bit de referência.

Quando verifica a página na cabeça da lista , também verifica o reference bit
  - Se o bit for 0 , substitui a página
  - Se o bit for 1 , limpa o bit e move a página para a cauda (é dada uma segunda chance a página)

A página que teve a sorte de ganhar uma segunda chance agora só será substitutida quando todas as páginas a sua frente na queue forem susbtituidas ou lhes for dada também uma segunda chance.
- Se uma página é usada frequentemente é  reference bit é sempre colocado a 1, essa página nunca vai ser substituida.

Problema: Mover páginas para a tail é uma operação complexa.

### Exemplo:
![imagem](https://user-images.githubusercontent.com/62023102/119270251-b0a54400-bbf3-11eb-8e89-18cff9634c8a.png)



## Clock Page Replacement

É uma implementação mais eficiente do algoritmo anterior, as páginas são organizadas numa queue circular e existe um pointer que indica qual a proxima página a ser verificada.

Quando é necessário escolher a página que vai ser trocada, o pinter avança até encontrar a página que tem o bit de referência a 0.
 - A médida que avança vai apagando os bits das outras páginas
 - Assim que a página for encontrada, ela é substituida e a nova página é inserida na queue circular na posição de onde a outra saiu.

### Encontrará sempre uma página ou vamos ter um loop infinit?

- No pior caso, quando todos os bits estão definidos, o ponteiro passa por toda a fila, é dada a cada página uma second chance e , em seguida, susbtitui a página inicial. 


### Exemplo:

![imagem](https://user-images.githubusercontent.com/62023102/119270491-c9fac000-bbf4-11eb-985f-ece38e606b47.png)

## NRU Page Replacement:

NRU (not recently used) este algoritmo é a favor de deixar em memória as páginas que foram usadas recentemente.

Considera um bit de acesso (A) e o bit de escrita (W) (definidos quando a página é modificada ou escrita) tornam-se possiveis as seguintes classes (A , W):
1. Class 0 = (0,0) não foi recentemente usada nem modificada - Melhor escolha para substituir
2. Class 1 = (0,1) não foi recentemente usada mas foi modificada - A página teria de ser escrita para o disco antes de ser substituida.
3. Class 2 = (1,0) recentemente usada mas não modificada - Prob vai ser usada novamente (soon)
4. Class 3 = (1,1) recentemente usada e modifica - Prob vai ser usada novamente (soon) e ainda teriamos de escrever a página novamente no disco antes de a susbtituir.

A configuração dos bits normalmente é feita pelo hardware

[How to implement a NRU Algotithm](https://www.geeksforgeeks.org/not-recently-used-nru-page-replacement-algorithm/)

### Diferentes Algoritmos

![imagem](https://user-images.githubusercontent.com/62023102/119270823-8a34d800-bbf6-11eb-81a9-3b003479e4ab.png)

## Allocation of Frames

### Como alocar uma quantidade fixa de memória físca entre processos?

- Não podemos alocar mais do que o número total de frames avaliaveis.
- Devemos alocar um número minimo de frames para manter justa a page-fault rate.

`Equal Allocation` -> Alocar uma parte igual para todos os processos
 - Se existem 100 frames e 2 processos -> 50/50;

`Proportional Allocation` -> Alocar proporcionalmente ao tamanho dos processos
- Se existem 100 frames e 2 processos, um dos processos tem 20 pages e outro 180 pages -> dá-mos 10 frames ao primeiro (20/200*100) e 90 frames ao segundo (180/200*100).

`Priotiy allocation`-> Usamos alocação proporcional mas em vez de termos em conta o tamanho do processo, temos em conta a sua prioridade.

-  No caso de page-fault, escolhemos um frame desse processo ou a frame de um um processo de lower priority.

## Global Vs Local Replacement

`Global Replacement` -> Permite a um processo selecionar uma frame de todo o conjunto de frames, mesmo aquelas que estão atualmente alocadas a outro processo. (Um processo pode "roubar" um frame de outro)
 - O tempo de execução do processo pode variar muito, uma vez que o seu comportamento de pagin depende dos outros processos a executar em simultâneo.
 - Resulta numa maior produção do sistema, logo é mais comum.

`Local Replacement`-> Cada processo seleciona apenas do seu próprio conjunto alocado de frames
- Performance mais consistente por processo.
- Pode sobrecarregar a mémoria, mas não permite o uso das pages menos usadas por outros processos

## Thrashing

Thrashing ocorre quando um processo esta ocupado a trocar pages in and out e demora mas tempo nessa troca do que a executar.

Se um processo não tem frames suficientes a sua taxa de page-fault é bastante alta.
- Page fault para obter page que precisa.
- Substitui frames existentes.
- Uma vez que todas as suas páginas estão em uso ativo, rapidamente deve precisar de trazer de volta a página que foi substituida.
- Como consenquência, entrámos novamente num estado de page fault (again,again,again) substituindo páginas que vai precisar de ir buscar imediatamente.

Isto leva a uma utlização baixa do CPU.

- O sistema operativo passa maior parte do tempo a fazer swapping para o disco.

Muitas operações de swap acabam por danificar o disco mas é para isso que servem os BACKUPS!!
