# Pipes

## Contexto:
Pipes foram uns dos primeiros mecânismos de IPC, nos primeiros sistemas Unix e fornecem uma das formas mais simples para a comunicação entre processos.

Pipes permitem a comunicação num estilo standard (Producer-Consumer):

 - O producer escreve para uma ponta do pipe (the write-end);
 - O consumer lê pela outra ponta do pipe (the read-end);

Os pipes são Unidirecionais, apenas permitem comunicação one-way.

### Note:
 Se for necessária comunicação two-way,devem ser usados dois pipes,com cada pipe a enviar dados em direções diferentes.
 
 Pipes podem ser contruidos no `terminal` usando o este caráter `|`:
  - ls | sort
  - cat fich.txt | grep xpto

# Pipes - UNIX/Linux

Em sistemas Unix/Linux, os pipes são criados por mais uma system call pipe();
  ```c
  int pipe(int fd[2]);
  ```
A system call pipe() cria um novo pipe e inicia fd[2] com os seguintes descritores:
 - fd[0] é o read-end do pipe.
 - fd[1] é o write-end do pipe.

Unix/Linux tratam os pipes como um tipo de ficheiro especial, o que permite o acesso aos mesmos usando as comuns system calls read() and write();

![Imagem1](https://user-images.githubusercontent.com/62023102/119226666-33e66d00-bb02-11eb-8d81-6de704d5044c.png)

# Forking de um Pipe

Um pipe não pode ser acedido fora do processo que o criou.

 - Tipicamente, um processo pai cria um pipe e usa-o para comunicar com o processo filho que é criado via fork().
- Como o pipe é considerado um tipo de ficheiro especial, o processo filho herda o pipe do processo pai.

![imagem](https://user-images.githubusercontent.com/62023102/119226751-a9523d80-bb02-11eb-9924-58f592eea9f2.png)

# Pipe de Pai para Filho

Depois de um fork, podemos decidir a direção do fluxo dos dados dentro do pipe.

 - Num pipe de Pai-Filho, o progenitor fecha a extremidade de leitura (read-end) do pipe fd[0] e o filho fecha a extremidade de escrita (write-end) fd[1].

- Leitura apartir de um pipe aberto (i.e pelo menos um dos processos tem fd[1] aberto) bloqueia o pipe enquanto este se encontra vazio.

![imagem](https://user-images.githubusercontent.com/62023102/119226933-7b212d80-bb03-11eb-8590-cd7e1e216fe7.png)


# Comunicação por Pipes - UNIX/Linux

```c
#define BUFFER_SIZE 25
#define READ_END 0
#define WRITE_END 1

int main(void){
  char write_msg[BUFFER_SIZE] = "Folks";
  char read_msg[BUFFER_SIZE];
  int fd[2];
  pid_t pid;
  
  /* create the pipe */
  if(pipe(fd) == -1){
    fprintf(stderr,"Pipe failed");
    return 1;
  }
  
  /* fork a child process */
  pid = fork();
  
  if (pid < 0){ /* error occurred */
    fprintf(stderr, "Fork Failed");;
    return 1;
   }
   
   if (pid > 0) { 
   /* parent process */
   /* close the unused end of the pipe */
   close(fd[READ_END]);
   
   /* write to the pipe */
   write(fd[WRITE_END]), write_msg , strlen(write_msg)+1);
   
   /*close the write end of the pipe */
   close(fd[WRITE_END]);
  }
  else{
   /* child process */
   /* close the unused end of the pipe */
   close(fd[WRITE_END]);
   
   /* read from the pipe */
   read(fd[READ_END], read_msg, BUFFER_SIZE);
   printf("read %s \n",read_msg);
   
   /* close the read end of the pipe */
   close(fd[READ_END]);
  }

  return 0;
 }

