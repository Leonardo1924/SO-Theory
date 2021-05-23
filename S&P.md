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

