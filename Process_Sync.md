# Sincronização de Processos

Os processos cooperativos são uns dos quais podem afetam ou podem ser afetados por outros processos a correr ao mesmo tempo no sistema

 - Estes processos podem partilhar diretamente um endereço logico ou serem permitodos a partilhar dados atráves de ficheiros ou mensagens.

Acesso concorrente a dados partilhados pode resultar em inconsistencia de dados.

- Um processo pode ser interrumpido em qualquer ponto completando a sua execução parcialmente.
- A manutenção da consistência dos dados requer mecaniusmos para assegurar a execução ordenada dos processos de cooperação.

## Critical Sections and Race Conditions

Uma região critica é um pedaço do codigo que acesse a um recurso partilhado.

- Alterar códigos de variáveis comuns, atualizar uma tabela, escrever um ficheiro.

Qaundo um processo esta a executar numa região critica, nenhum outro processo pode estar a exeutar nessa mesma região critica, isto é , 2 processos não podem estar a executar na mesma região critica ao mesmo tempo.
- Isto requer que os processos sejam sincronizados de alguma maneira.

Uma race condition ocorre quando vários processos tem permissão para aceder e manipular um recurso partilhado e o resultado depende particularmente na ordem em que o acesso ocorre, maior parte das vezes leva a um resultado surpreendete e indesejável.

## Critical Section Problem

O objetivo do problema da secção crítica é desenhar um protocolo que os processos possam usar para cooperar:

- Os processos devem pedir permissãopara entrar na secção crítica, a entry section.
- A região critica deve ser seguida pela exit section ou por uma remainder section(secção não critica).

![imagem](https://user-images.githubusercontent.com/62023102/119234360-6bb2dc00-bb25-11eb-8542-2e05cb5b8222.png)

Uma solução para este problema tem de satisfazer 3 requerimentos:

 - `Exclusão mútua` - Se um processo estiver a ser executado numa secção crítica, então nenhum outro processo pode ser executado na mesma secção crítica.

- `Progresso` - Se nenhum processo for executado numa secção crítica, então apenas os processos que não estão a executar numa secção restante podem participar na decisão de qual entrará na secção crítica em seguida, e esta decisão não pode ser adiada indefinidamente.

- `Espera limitada`- Deve existir um limite no número de vezes que outros processos são autorizados a entrar nas suas secções críticas depois de um processo ter feito um pedido para introduzir a sua secção crítica e antes que esse pedido seja concedido

No Capítulo que fala sofre Semáforos podes ver um tecnica usada para resovler este problema.
