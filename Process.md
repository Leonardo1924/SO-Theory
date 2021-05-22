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

# Criação de Processos

Durante a execução,um processo pode criar varios novos processos.
 - O processo criador é chamado de "Pai" e os novos processos sãos os seus "filhos".
 - Cada novo processo originado pelo Pai pode criar outros novos processos sendo criada uma "árvore de processos".

A maioria dos sistemas operativos identifica os processos de acordo com um unico `process identifier`
(pid), que tipicamente é um integer.

- O pid fornece um unico valor por cada processo no sistema, e pode ser usado como um indice para aceder a varios atributos de um processo.

# Alternativas da criação de Pai/Filho

 ## Partilha de recursos:
 Existem 3 casos possiveis no que toca a Partilha de Recursos
  - Processos Pais e Filhos partilham todos os recursos.
  - Processos Filhos partilham um subconjunto de recursos dos pais.
  - Processos Pais e Filhos não partilham nehum recurso.

## Execução:
Existem 2 casos:
 - Pais e Filhos executam de forma concorrente.
 - Pais espera até um filho ou todos acabem de executar.

## Espaço de Endereço:
 - Pais e filhos são duplicados (a filho começa com o programa/dados do processo pai (Herança)).
 - O processo filho contém um novo programa carregado dentro dele.

# Quanto "custa" para criar um processo?
   Copiar o estado de I/O dos pais (Dispositvos alocados , lista de ficheiros abertos , etc)
    - Medium expensive.
   
   Configurar novas tabelas de memória para o espaço de endereço.
   - More expensive 

   Copiar dados do processo pai
   - Originalmente very expensive
   - Less expensive com copy-on-write

# Criação de Processos - Unix/Linux

Em sistemas Unix/Linux, é possivel criar novos processos através da system call `fork()`;
- O novo processo (filho) consiste numa copia do endereço do processo original(pai);
- O return do `fork()` é zero para o filho e o process identifier (pid) do filho é returnado para o pai.
- Ambos os processos (Pai-Filho) continuam a execução concorrente das instruções depois do fork()
- Este mecanismo premite ao processo pai comunicar de forma mais fácil com o processo filho.

```c
// codigo do processo pai antes do fork()
((pid = fork()) < 0) {
... // fork failed
else if (pid == 0){
... // codigo do processo filho depois do fork()
else{
... // codigo do pai depois do fork();
. // codigo comum depois do fork();
```

Normalmente depois de um fork(), um dos dois processos (pai ou filho) utiliza outra system call `exec()`
 - execl("/bin/ls","ls", NULL);

## Exec():

Esta system call substitui o espaço de memória do processo -> text,data,heap,stack -> com um novo programa vindo do disco começando a executar este novo programa na sua main function, destroindo a iamgem anterior do processo. 
 - Desta maneira, pais e filhos são capazes de seguir caminhos diferentes.

# Fim de um processo

Um processo termina a sua execução quando explicitamente o mesmo utliza a system call `exit()` ou quando executa a ultima instrução (neste segundo caso, a chamada do `exit()` esta implicita pelo uso do return na main()).

 - Todos os recursos dos processos -> incluindo memoria virtual/fisica, ficheiros abertos e I/O buffers -> são desalocados pelo sistema operativo.

Um valor de estado `exit()` (normalmente um integer) é disponibilizado ao processo pai atráves de outra system call -> `wait()`; 

## Um processo pode terminar a sua execução atráves do seu processo pai.
Quando é que isso acontece:
 - O processo filho excedeu o número de recursos alocados.
 - A tarefa atribuida ao processo filho não é mais necessária.
 - Se o processo pai esta a dar um exit() o sistema operativo não permite que o processo filho continue a sua execução sem o processo pai (cascading termination).

Se um processo terminou e nenhum processo pai esta se encontra a espera, então esse processo que finalmente terminou entra num estado conhecido como estado Zombie (Zombie Process).
 - Apenas quando o pai chama o wait(), o pid do processo Zombie e a sua entrada na tabela são lançados.

Se um processo pai terminar antes do seu processo filho, então todos os processos filhos ficam conhecidos como processos órfãos (orpahn processes).

 - Unix/Linux abordam os processos órfãos atribuindo o processo init como o novo progenitor a esses processos.
 - O processo init periodicamente usa wait(), permitindo a libertação do pid e da entrada na tabela do processo órfão.

Em Unix/Linux, podemos terminar um processo usando a invocação da system call exit(), fornecendo um status como parâmetro.
 - exit(status)

Para esperar pelo fim do processo filho e obter o exit status, podemos usar a system call wait(), que também returna o pid do processo que termina para que o pai seja capaz de dizer qual dos seus filhos é que ja acabou de executar.

  - pid = wait(&status)

![imagem](https://user-images.githubusercontent.com/62023102/119225352-2d082c00-bafb-11eb-8003-9fe458ae8a85.png)

## Forking do comando 'ls' - Unix/Linux

```c 
int main(){

pid_t pid;

/* fork a child process */
 pid = fork();
 
 if(pid < 0) {  /* Ocorreu um erro */
   fprintf(stderr, "Fork Failed");
   return 1;
 }
 else if (pid == 0){ /* Processo Filho */
      execlp("/bin/ls","ls",NULL);
 }
 else { /* processo pai */
   /* o pai ira esperar pelo processo filho completar a execução */
   wait(NULL);
   printf("Child Complete");
 }
 
  return 0;
}
```

# Comunicação entre Processos

Os processos dentro de um sistema podem ser independentes ou cooperar:
 - `Processos Independetes`: Não podem afetar nem ser afetados por outras execuções.
 - `Processos cooperantes`: Podem ser afetados por outras execuções.

## Principais razões para os processos de cooperação:
 - `Partilha de Informação` -> Acesso concorrente ao mesmo pedaço de informação
 - `Modularidade`-> Quebrar o cálculo em subtasks que fazem mais sentido
 - `SpeedUp` -> Executar subtasks concorrentes de forma paralela.

Para trocarem dados e informação, processos de cooperação precisam de suportar mecanismos de IPC (interprocess communication). Existem dois modelos fundamentais:

 - Passagem de mensagens (Message passing) -> Comunicação via enviar/receber mensagens; 
 - Memória Partilhada (Shared Memory) -> A comunicação ocorre simplesmente por ler/escrever para a memoria partilhada.

#### Note: 
  Memória Partilha pode levar a problemas de sincronização complexos. No entanto é funciona de maneiras mais rapida comparativamente a Message Passing.

![imagem](https://user-images.githubusercontent.com/62023102/119226075-6b074f00-baff-11eb-8315-18e178b498e2.png)

