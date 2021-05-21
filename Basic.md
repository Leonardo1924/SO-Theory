# O que é um Sistema Operativo?

## Motivação:

- Hardware por si só não é particularmente facil de usar, sendo portanto necessário algum tempo de software.
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


