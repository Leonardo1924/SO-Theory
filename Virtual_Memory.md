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

A `page fault` ocorre quando o hardware de paging acede a uma pagina marada como invalida.
  - Na tradução do endereço pela page table, o paging hardeare irá notar que um bit inválido foi coloca, causando uma trap(exception/fault) no sistema operativo.

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







