# Processos

## Conceito:

Um Processo é considerado uma unidade de trabalho na maioria dos sistemas e pode ser considerado como um programa em execução.

Um processo necessita de recursos para concretizar as suas tarefas, entre os quais:

- Tempo de CPU;
- Memória;
- Ficheiros;
- Dispositivos de I/O;
- ...

Recursos podem ser alocados para um processo no momento em que este é criado ou enquanto este está a executar.

### Um processo tem multiplas partes:

- `Text Section` -> Contém o codigo do programa;
- `Data Section` -> Contém as variavéis globais;
- `Heap` -> Contém memoria alocada dinamicamente durante o runtime do processo;
- `Stack`-> Contém dados temporarios(paramêtros de funções, endereços de returno e variavéis locais).

## Programs Vs Processes:

### Programa:

Uma entidade passiva (chamada maior parte das vezes de executável) que contém uma lista bem definida de instruções.

### Processo:

Uma entidade ativa correspondente a uma sequência de execução de um pograma.

Um programa torna-se um processo quando esta carregado na memoria.
 - Um programa pode ser um conjunto de varios processos.
 - Dois processos associados com o mesmo programa são considerados duas sequências de ececução separadas.

#### Exemplo:

Se o mesmo utilizador abrir varias copias do programa firefox:
 - Cada copia é considerada um processo diferente e embora a parte da text section ser equivalente, a data, heap e a stack podem variar.

## Estados de Um Processo:

A medida que um processo executa, vai mudando o seu estado de acordo com a atividade atual.

Um processo pode ser encontrado num dos seguintes estados:

- `New` -> O processo esta a ser criado.
- `Running` -> Instruções estão a ser executadas.
- `Waiting/Blocked` -> O processo esta a espera que algo ocorra.
- `Ready` -> O processo esta a espera de ser atribuido a um processador.
- `Terminated` -> O processo terminou a sua execução.

É importante perceber que muitos processos podem estar Read ou Waiting, no entanto apenas 1 processo pode estar a correr no processador de cada vez.

![imagem](https://user-images.githubusercontent.com/62023102/119210666-631ac100-baa5-11eb-9adc-37d15dd5f8b5.png)

# Creação de Processos

Durante a execução,um processo pode criar varios novos processos.
 - O processo criador é chamado de "Pai" e os novos processos sãos os seus "filhos".
 - Cada novo processo originado pelo Pai pode criar outros novos processos sendo criada uma "árvore de processos".

A maioria dos sistemas operativos identifica os processos de acordo com um unico `process identifier`
(pid), que tipicamente é um integer.

- O pid fornece um unico valor por cada processo no sistema, e pode ser usado como um indice para aceder a varios atributos de um processo.



