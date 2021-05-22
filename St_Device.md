# Storage Devices

## Data Transfers 

Um unidade de disco é anexada a um computador através de um conjunto de fios chamadso de I/O bus
  - EIDE, ATA, SATA, USB, SCSI,....

Transfêrencias de dados são levadas a cabo por hardrare especial conhecido por controladores. 
- O `host controller` é o controlador na extremidade do computador do bus.
- O `disk controller`é o controlador incorpotado em cada disco.

Para realizar um operação de I/O no disco:

- O OS começa por colocar um comando no host controller
- O host controller em seguida envia esse comando para o disk controller
- The disk controller opera o hardware do HDD/SSD para realizar o comando
- A transferência de dados no HDD/SSD acontece entre a superfícei do disco e uma cache incorporada no disk controller.
- A transferência de dados para o host ocorre entre a cache e o host controller

## Latency Times:

Qualquer operação de I/O é um processo de 5 fases:

- `Queuing time` -> É o tempo para o driver do dispostivo processar o pedido.
- `Controller time`-> É o tempo para o disk controller levar a cabo o pedido.
- `Seek time`-> É o tempo para mover o braço do disco para o cilindro desejado.(HDD)
- `Rotational latency`-> É o tempo para o setor desejado rodar debaixo da cabeça do disco.
- `Transfer time`-> É o tempo para transferir os dados desejados entre o drive e o computador.

Disk latency -> é a soma de todos os tempos acima e corresponde ao tempo total entre um pedido e o resultado disponível.

![imagem](https://user-images.githubusercontent.com/62023102/119241973-2f49a500-bb52-11eb-95da-0822498fba19.png)

## Disk Scheduling (HDD):

Um pedido I/O especifica várias informações:
  - O tipo de operação (input ou output);
  - Os endereções de disco e de memoria para trasnferência;
  - O número de setores a ser transferidos

Sistema operativo mantém uma fila de pedidos pendentes por disco
- Quando um pedido é completado, OS escolhe qual o proximo pedido a atender.

Como é que o o OS programa a manutenção dos pedidos do disco?
  - Existem imensos algoritmos de otimização.
  - Algoritmos de otimização apenas fazem sentido quando queues existem, de outra forma um pedido pendente pode ser executada imediatamente.

## Disk Scheduling (SSD)

Algoritmos de agendamento de discos concentram-se proncipalmente em minimizar a quantdidade de movimentos da cabeça (ver constituição do hdd na imagem em cima), no entanto os discos SSD não contém esse mecânismo. 
 - Muitos dos SSD secheduler ordena usando o método de FCFS 
