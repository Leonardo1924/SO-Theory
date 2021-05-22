# Memory Management

## Background:

A memória(ou memória principal) consiste num grande array de bytes cada um com o seu proprio endereço.
 - A unidade de memória vê apenas um fluxo de endereções de memória e não sabe como são gerados ou para que servem

Memória e Registos são o único tipo de armazenamento a que o CPU pode aceder diretamente.

- Os programas deve ser trazidos do disto para a memoria para ser corridos
- O CPU, em seguida, recolhe as instruções da memória de acordo com o counter do programa.
- Estas instruções podem causar carregamento/armazenamento adicional de/para endereços de memória específicos

## Hardware Basico:

A maioria dos CPUs consegue descodifcar as instruções e realizar operações simples em registos à taxa de uma ou mais operações por CPU clock cycle

A memoria é acessida pelo memory bus o que pode levar algums clock cycles do CPU

Cache encontra-se entre a memory e os registos do CPU 
  - Permite aumentar a velocidade de acesso a memóra 

Nota: Tudo o que foi menciona anteriormente ja terá sido abordado na caderia de Arquitetura de Computadores

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
 
