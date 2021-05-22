# O que é um Sistema Operativo?

## Motivação:

- Hardware por si só não é particularmente facil de usar, sendo portanto necessário algum tipo de software.
- Diferentes programas, geralmente requerem operações comuns para operar os recursos de hardware.
- Uma boa ideia seria juntar num unico programa as funções comuns para o controlo e alocação dos recursos de hardware.
- Este programa comum é conheçido como Sistema Operativo.

### Como podemos definir um Sistema Operativo ou Parte dele?

- Não existe um definição universalmente aceite.
- Uma definição mais restrita seria "Aquele programa que corre o tempo todo num computador", tudo o resto ou é um system program ou uma aplicação.

#### Um Sistema Operativo é o que gere o hardware do Computador:

- Atua como intermediário entre o utilizardo e o hardware.
- Fornece o basico para aplicações.
- Alguns sistemas operativos são projetados para tornar o uso do sistema conviniente, outros para tornar o uso do hardware mais eficiente e outros combinam os dois casos.

## O que devem fazer os Sistemas Operativos?
  
  - Um sistema operativo foi concebido para maximizar a utlização de recursos e para garantir que todo o tempo do Cpu, memoria e I/O são usados de forma eficiente e justa entre todos os utilizadores.
  
  - O Sistema operativo é projetado principalmente para facilitar a utilização, com mais foco na performance do que na utlização de recursos.
  - Configurado para correr com nenhuma/minima intervenção do usuario.

### Um Sistema pode ser divido em 4 componentes principais:
  
  - `Hardware:` Os recursos basicos do sistema (Cpu, memoria, I/O devices, ...);
  - `Sistema Operativo:` Controla e coordena o uso do Hardware entre os diferentes programas e utilizadores;
  - `Programas:` Definem a maneira como os recursos do sistema são usados para resolver as necessidades do Utilizador( processadores de palavras, browsers, bases de dados, games, compiladores);
  - `Users`.

## Principios de um Sistema Operativo:

### Sistema Operativo como um alocador de recursos/controlador:

  - Atua como um gestor de todos os recursos: Tempo do CPU, espaço em memoria, I/O devices.
  - Decide entre pedidos em conflito para um uso de recursos mais eficiente e justa.
  - Previne erros e utilização "impropria" do computador.
  - Especialmente importante quando muitos utilizadores tem acesso aos mesmos recursos.
  
### Sistema Operativo como um facilitador:
  
  - Fornece serviços que todos precisam.
  - Torna a programação um processo mais facil, rápido , menos propenso a erros.
  - Especialmente preocupado com a execução de varios programas.
  
### Muitos OS aplicam os principios anteriores:
  - `Exemplo:` O gestor de ficheiros é necessário para todos os users (facilitador) e tem de ser eficiente e protegido (controlador).

## Principais Componentes.
  ### Sistemas Operativos modernos incluem, normalmente, os principais componentes:
  
  - Gestor de Processos;
  - Gestor de Memoria;
  - Gestor de Armazenamento;
  - Gestor de I/O (Input/Output);
    
## Gestor de Processos:
  ### Funções Principais:
    
   - Criar, suspender, resumir e terminar processos (User/System);
   - Fornecer mecanismos para a comunição de processos;
   - Fornecer mecanismos para a sincronização de processos;
   - Fornece mecanismo para problemas ligados ao bloqueio de processos;
    
## Gestor de Memoria:
   ### Funções Principais:
   
   - Alocar e desalocar espaço na memoria conforme necessário.
   - Ratreio das partes da memoria que estão atualmente a ser utilizadas e por quem.
   - Decidir que processos/dados mover para dentro e fora da memoria e quando.

## Gestor de Armazenamento:
   ### Funções Principais:
   
   - Fornece uma visão uniforme (abstrata) e logica da informação no armazenamento ( visão em ficheiros e diretorias logicas).
   - Apoia `primitives` para criar, apagar e manipular ficheiros ou diretorias.
   - Apoia o controlo de acesso para determinar quem te acesso ou não a determinado ficheiro ou diretoria.
   - Mapeamento em suportes de armazenamento secundário.
   
## Gestor de I/O :
   ### Funções Principais:
   
   #### Esconder pecularidades de dispositivos de hardware do utlizador:
   
   - Geralmente interface dispostivo-driver.
   - Drivers para dispostivos de hardware especificos.

  #### Responsável pela gestão de memória de I/O incluindo:
  
  - Buffering - Armazenamento temporario de dados enquanto estão a ser transferidos.
  - Catching  - Armazenamento de partes de dados em armazenamento rapido para melhor a performance.
  - Spooling  - Sobreposição de um output de um processo com o input de outros. 
  
## Serviços Comuns de um Sistema Operativo:

  ### Sistemas operativos fornecem um ambiente para a execução de programas e, para isso, fornece servicos especificos para programas e para utilizadores
  ![imagem](https://user-images.githubusercontent.com/62023102/119206636-95bbbe00-ba93-11eb-96c6-3fe7e2ed6592.png)
  
  #### Os serviços fornecidos varia entre sistemas operativos mas existem alguns comuns a todos:
  
  - `USER interfaces`: Permitem o funcionamento e controlo eficazes do sistema.
  - `Programa execution`: Para carregar um programa em memoria e corre-lo.
  - `I/O operations`: Fornece um meio para as operações ligadas a I/O.
  - `File Systems`: Permite uma manipulação eficaz de ficheiros e diretorias.
  - `Communications`: Permite a troca de informação entre processos no mesmo dispostivo ou entre dispostivos.
  - `Error detection`: Para estar constantemente ciente de possiveis erros que podem ocorrer no CPU/ Memoria / I/O ou programas do utlizador de forma a tomar a ação adequada para assegurar uma computação correta e consistente.

#### Outros Serviços existentes não para proveito do utilizador mas do sistema em si:

-`Alocação de Recursos`: Quando multiplos processos se encontram a correr de forma concorrente, os recursos disponiveis (CPU/MEMORY,File Storage ...) devem ser alocados de forma eficiente para cada um dos processos.

-`Accounting` : Para acompanhar a quantidade e os tipos de recursos utilizados por cada utilizador.

-`Proteção e Segurança`: Para evitar a interfêrencia entre processos concorrentes ou com o próprio OS e proteger o sistema contra outsiders.

# MultiPogramming
  - Um dos aspetos mais importantes de um sistema operativo é a capacidadede ter multiplos programas a correr.
  
  - MultiProgramming aumenta a utilização do CPU pela organização de processos para que o CPU possa sempre executar um processo.

## A ideia é a seguinte:

- O sistema operativo começa a executar processo por processo ordenamente.
- Eventualmente algum processo terá de esperar por alguma tarefa, tal como uma operação da I/O.
- Num caso de não existir MultiProgramming, o CPU ficar inativo enquanto esperava.
- Num Sistema MultiProgramming, o sistema operativo troca para outro processo. Quando um processo precisa de esperar por algo, o CPU volta a trocar para outro processo e assim sucessivamente. Eventualmente o primeiro processo recebe as informação pelas quais estive a aguardar e recupera o CPU.
- Desde que pelo menos um processo precise de ser executado o CPU nunca fica inativo.

# Multitasking
  - Como referi anteriormente MultiProgramming aumenta a utilização do CPU.
  - Multitaskin é uma extensão logica de MultiProgramming que aumenta o tempo de resposta no qual o CPU troca entre processos, permitindo ao utilizador interagir com o processo enquanto este esta a correr.

# Arranque do Sistema
 ## BootStrap(FirmWare):
 É carregado quando ligas ou das reboot ao sistema.
 - Inicializa todos os aspetos do sistema.
 - Carrega o Kernel do Sistema Operativo e começa a executá-lo.
 - Estando o Kernel carregado, o sistema pode começar a fornecer serviços ao utilizador.

Completada esta fase, o sistema esta completamente iniciado e preparado para o momento em que algum evento ocorra.

# Proteção do CPU

Não podemos permitir que um programa fique "preso" ou falhe e nunca devolva o controlo do sistema ao utilizador. Para evitar isso podemos utilizar um `Timer`;

## Timer:

Pode ser configurado para interrumper um processo depois de um certo periodo de tempo.
Se o timer interromper algum procresso o controlo é transferido de imediato para o sistema operativo sendo a interrupção tratada como um fatal error.

# System Calls

As `System Calls` funcionam como uma interface para os serviços do Sistema Operativo.
São, na maior parte dos casos acedidas através de uma API em vez de uma chamada direta ao sistema.

## Por que razão é melhor usar uma API que uma chamada direta ao sistema?
  
  - O mesmo programa deve compilar e correr num outro sistema que suporta essa API.
  - As System Calls podem ser mais detalhas e dificies de trabalhar quando comparadas a API disponivel para o programador. 

## System Call -> Implementação
  
  Tipicamente, um número é associado a cada system call que depois é mantido numa tabela indexada de acordo com esses mesmos números.
  
### Exemplos de Tipos de System Calls: 

#### Podem ser agrupadas em 6 categorias:
  - Controlo de Processos;
  - Manipulação de Ficheiros;
  - Manipulação de Dispositivos;
  - Manutenção de informação;
  - Comunicação(Pipes);
  - Proteção;

![imagem](https://user-images.githubusercontent.com/62023102/119208669-2ba71700-ba9b-11eb-9649-5c4199b6b5aa.png)

