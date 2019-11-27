## Kafka

Sistema de streaming de dados distribuído

- desenhado para mover dados em alta performance
- distribuído nativamente e por padrão, garantindo recuperação de falhas
- Tem sido utilizado como 'single source of truth'

### Características

- Flexível para publish/subscribe
- baixo acoplamento
- escalável horizontamente
- alta vazão de dados (throughtput)
- Reliable & Durable
- Usa tópicos ao in´ves de filas

### Fila x Tópico

- Fila eu tenho um modelo de dados onde a mensagem é sempre colocada no final da fila (fifo, mantém a ordem)
- Tópico, quando coloco um dado, dispara para todos os subscribers que eu tiver

No kafka, um dado que está no tópico nunca sairá do tópico, com um offset (uma posição). Quem consome o offset é o consumer (watermark).

O consumer sabe o último watermark que ele consumiu e continua dali por diante.

### Aplicações mais comuns:

- Message Broker
- Storage systems (pode ser usado como banco de dados)
- Streams processors

### Arquitetura

#### Producers

- quem coloca as mensagens para dentro do cluster

#### Connectors

- Traz dados para dentro do KAFKA oriundos de uma base que já existe hoje (pego um driver do kafka e conecto num sql e processo dentro do kafka).

#### Stream Processors

- Processa a informação do kafka como um consumer sem tirar dados do kafka (pega um dado e não remove do kafka como um consumer).
- Consigo fazer tranformações dentro do próprio kafka (fazer engenharia de dados dentro do próprio kafka)
- Não tenho trafego na rede

#### Consumers

- quem usa os dados que foram inseridos no cluster

### Topic

Tópico possui partições
Cada partição é independente uma da outra

Quando escrevo um dado no tópico, sempre adiciono um ítem na cauda (no final)

Tópicos independentes.

Offset = ID (imutável)
Partição = Table (banco de dados de estado)

Tópic é um streaming que atua como um banco de dados
Possui um armazenamento persistente

Um tópico tem diversas partições, cada uma definida por um número.

A quantidade de partições é definida na criação do tópico

#### Propriedades

- As partições são independentes
- Ordenadas e sequencia dos registros são imuáveis
- O offset é a posição de um registro na partição, ID sequencial e único do dado.

Producer sempre escreve na cauda
consumer pode estar no offset 1 e o outro num offset 8

Se eu ligo o meu consumer no offset 1, ele vai ler todoas as mensagens daquela posição pra frente.

-Os producers adicionam registror ao stream sempre na cauda da partição
-Os consumers controlam o offset que desejam ler;
-Os consumers podem ler e reler as mensagens sem "perde-las"
-É possível criar consumer groups.

#### Consumer groups

Se todos os consumers estiverem dentro do mesmo consumer group, as mensagens são entreues separadamente como um load balancer.

Mas se os consumers estiverem em consumer groups diferentes, as mensagens são entregues para todos como um broadcast

para se comportar como uma fila, cria um consumer group só
para ser broadcast, crio vários grupos de consumidores

#### broker

cada broker é um servidor diferente
Existe um fator de replicação (quando crio um fator de replicação de um tópico, será 5 e tenho 6 servidores, 5 deles terão partições preenchidas com o meu tópico);
Cada tópico possui N partições e cada partição será replicado N vezes.
Cada partição tem pelo menos 1 servidor atuando como lider (controlando a leitura e escrita na partição).
Se eu tiver mais de um servidor, cada um terá a escrita, leitura e replicação em mais de um servidor.

Tenhos lideres e followers, se o lider morre, o kafka elege um novo lider.
quando voltar, novos followers são elegidos.

- cada partição é replicada em diversos brokers, de acordo com o replication-factor
- Isto garante que um dado nunca seja perdido;
- Cluster possui a estratégia de controllers (fica acima das partições, paralelizando as solicitações para pedir a mesma informação para todas as partições e recebe todas de uma só vez), loaders (manda na partição) e followers (recebe a replicação).

#### Podcast

hipsters.tech -> kafka

### no kafka nada fica em memória, ele precisa guardar o dado no estado

O kafka transforma a leitura em disco tão rápida quanto a leitura em memória

confluent

### Com o kafka é possível fazer georeplicação dos dados.

Implementando um pub/sub no kafka

Criar conta no confluent.cloud
https://confluent.cloud

U\$50,00 por mês para uso gratuito
npm install node-rdkafka
wrapper -> for kafka with c/c++ with blizzard
github

dev.to/wfelix
@waldyrfelix
Kafka
https://github.com/waldyrfelix/rocketseat-kafka
Kcicle com database

Dataflow - Coursera (kinesis)
Terraform
Metabase
