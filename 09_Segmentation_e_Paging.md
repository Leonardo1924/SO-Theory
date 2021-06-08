# Segmentation and Paging.

## Segmentation:

Segmentação é um esquema de gestão de memória que satisfaz a visão dá memória do programdor (como visto anteriormente).

 - Um espaço de endereços lógicos é uma coleção de segmentos.
 - A cada segmento é dada uma região contigua na memória (com o registo base e o limit) que pode residir em qualquer lugar na memória física.

![imagem](https://user-images.githubusercontent.com/62023102/119264340-aa569e00-bbda-11eb-9c06-1c4bbe12740d.png)


Cada segmento tem um indetificador (número) e um tamanho (length)

Endereções logicos (dentro do segmento) existem na forma de tuplos:

```c 
<segment-number, offset-within-segment>
```

A tabela dos segmentos mapeia endereços logicos de duas dimensões para endereções fisicos de uma dimensão. Cada entrada inclui:
- `Segment base` : Contém o endereço físco inicial onde o segmento reside na memória.
- `Segment limit`: Especifica o tamanho do segmento.

![imagem](https://user-images.githubusercontent.com/62023102/119264521-78920700-bbdb-11eb-879f-8644794cd1eb.png)

## Segmentation Architecture

A tabela de Segmentos encontra-se no PCB (Process control block). Esta tabela também inclui proteção de dados tais como:
- Validação de Bit (legal/ilegal segment).
- Privilégios de Ler/escrever/executar/partilhar.

![imagem](https://user-images.githubusercontent.com/62023102/119266280-0a9d0e00-bbe2-11eb-8190-8d5231816d80.png)

# Managing segments

Quando um novo processo é carragado em memória:
- Cria-se uma nova tabela de segmento e armazena-se no PCB de processos
- Alocar espaço na memória fisica para todos os segmentos do processo

Se não houver espaço na memória física:
- Compacta a memória (move segmentos, update bases) para tornar o espaço contiguo.
- Swap um ou mais segmentos para fora do disco

Para aumentar um segmento S:
- Se existe espaço livre em baixo do segmento, só é necessario atualizar o limit do segmento e usar esse espaço livre
- Mover o segmento S para um espaço livre maior
- Swap o segmento acima do S para o disco

![imagem](https://user-images.githubusercontent.com/62023102/119266782-083bb380-bbe4-11eb-9586-3841fb1227fe.png)


# Paging

Como a segmentação, `Paging` é outro esquema de gestão de memória que permite que o espaço do endereço físico de um processo não seja contiguo.

Comparada segmentação, `paging`:
- Torna a alocação e o swapping mais fácil ( não existem segmentos de tamanho variavel).
- Evita fragmentação externa
- Não é ncessária compactação

Devido a estas vantagens, pagin é usado de varias formas na maioria dos Sistemas Operativos, desde mainframes até telemóveis.

A ideia por detras deste esquema é organizar a memória em blocos de tamanho fixo.
 - Dividir a memória logica em blocos de tamanho fixo chamados `pages`.
 - Dividir a memória física ( e a backing store) em blocos do mesmo tamanho chamados `frames`.

Para carregar um processo de N pages, precisamos de encontrar N frames livres.
- Pages não têm de ser carregadas num conjunto contiguo de frames.
- Necessidade de acompanhar as free frames.

## Paging Model of Memory

Uma page Table acompanha todas as pages  num processo em particular.
 - Cada entrada contem o frame correspondente na memória física
 - Traduz endereços logicos em físicos.

![imagem](https://user-images.githubusercontent.com/62023102/119267304-de838c00-bbe5-11eb-9b86-43f198d5360b.png)

## Address Translation Scheme

Todos os endereços logicso são divididos em 2 partes:

- `Page Number (p)` : index na page table.
- `Page offset (d)` : deslocamento dentro da página

O endereço base na page table combinado com o page offset definem o endereço físico de memória que é enviado para a unidade de memória.

Se o tamanho do espaço do endereço lógico é 2^m e o tamanho da pagina é 2^n bytes (m > n), então a high-order m-n bits do endereço logico designa o `page number (p)`, e a lower-order de n bits indica a `page offset (d)`.

![imagem](https://user-images.githubusercontent.com/62023102/119267697-a1b89480-bbe7-11eb-8837-46eb53d09c74.png)


`Nota:` O hardware necessário para suportar paging é relativamente menor que o necessário para a segmentação.

## Paging Implementation

O sistema operativo intervem 4 vezes na paginação:

1. `Criação do Processo`
 - Determinar o tamanho do programa
 - Criação da tabela de páginas

2. `Execução do Processo`
 - Re-inicializar a MMU para o novo processo.
 - Limpar a TLB (Translation lookaside buffer)

3.`Na Page Fault`
 - Determinar o endereço virtual causador da page fault
 - Colocar a página em memória, se o endereço for legal

4. `Fim da execução do processo`
 - Libertar a tabela de páginas e as páginas associadas.

## Paginação + Segmentação

É possivel combinar estes dois métodos em dois niveis de mapeamento:

- Processos são vistos como um conjunto de segmentos lógicos de tamanho variavel
- Cada segmento é depois dividio em pages de tamanho fixo.

Os endereços lógicos consistem mais uma vez em tuplos:

```c
<segment-number, page_within_segment,offset_within_page>
```

### Implementação 

- Uma tabela de segmentos por processo + uma tabela de paginação por segmento.
- Evita fragmentação externa

A partilha pode ser dada em ambos os niveis
 - Partilhar um segmento completo por ter a mesma base em duas tabelas de segmento.
 - Partilhar uma frame por ter a mesma `frame reference` nas duas tabelas de paginação.


#### Nota:
A definição de `Page fault` vai ser vista no proximo capítulo onde é abordada a memória virtual.
