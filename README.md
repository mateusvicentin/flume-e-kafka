<h1 align="center">Executando arquvios com Flume e Kafka </h1>
<p>O projeto tem como objetivo apresentar um relatório utilizando Flume e Kafka. Este é um projeto da matéria de Processamento de Fluxos Discretos e Contínuos de Dados, que será realizado por meio de uma máquina no VirtualBox disponibilizada pelo professor, onde já estão configurados o Kafka e o Flume na máquina virtual.</p>

<h2>Kafka</h2>
<p>O Apache Kafka é um sistema de mensageria que pode realizar as seguintes funções: publicar e assinar fluxos de registros, armazenar fluxos de registros de forma eficiente na ordem em que os registros foram gerados, e processar fluxos de registros em tempo real.</p>

<h2>Flume</h2>
<p>O Apache Flume é utilizado para coletar arquivos de log de várias fontes, como servidores web, servidores de aplicativos e dispositivos de rede, ou até mesmo arquivos de texto de um computador.</p>


<h2>Iniciando o Servidor do Kafka</h2>
<p>Vamos iniciar o servidor do Kafka para que seja possível que o <code>Producer</code> e o <code>Consumer</code> funcionem corretamente.</p>
<p><strong>Producer:</strong> envia mensagens para um tópico, que é gerenciado e armazenado por um cluster.</p>
<p><strong>Consumer:</strong> subscreve no tópico utilizado para realizar a leitura e o processamento das mensagens do <code>Producer</code>.</p>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-server-start.sh /home/puc/kafka_2.11-1.0.0/config/server.properties
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/b7cb3ee6-a21b-44ac-b091-048db4f1c48b" alt="img1">
</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/f8d0c25d-bd2b-462d-ae50-3bf19bffdbdb" alt="img2">
</p>

<h2>Criando topicos no Kafka</h2>
<p>Irei criar um topico chamado <code>mateusvicentin</code> para exemplo do funcionamento do Kafka</p>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic mateusvicentin
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/1e82c41c-b6f1-4978-9e4c-05d05d05c7d0" alt="img3">
</p>

<h2>Listando topicos no Kafka</h2>
<p>Vou utilizar o comando <code>--list</code> para visualizar todos os topicos que ja foram criados, incluisve o que criei <code>mateusvicentin</code></p>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-topics.sh --list --zookeeper localhost:2181
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/08f30aad-9020-43be-b5b3-e910acb5ac48" alt="img4">
</p>

<h2>Inserindo Strings no topico criado no Kafka</h2>
<p>Vamos utilizar dois comandos, o <code>Producer</code> e o <code>Consumer</code> um será responsavel pro inserir as Strings e o outro será responsavel por ler essas String</p>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic mateusvicentin
```
```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic mateusvicentin
```
<h4>Producer</h4>
<p>O responsavel por enviar as mensagens ao topico.</p>

<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/b73b3667-b2fd-43d4-897b-29df6900a60d" alt="img5">
</p>

<h4>Consumer</h4>
<p>O responsavel por por receber e fazer a leitura das mensagens.</p>


<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/eb8fc47c-69f1-484e-aadc-e8d42805aa75" alt="img6">
</p>

<h2>Testando a leitura</h2>
<p>Para testar a leitura, fiz a gravação da tela onde foi aberto dois terminais, um contendo o <code>Producer</code> e outro contendo o <conde>Consumer</conde> para que possa ser visualizado em tempo real o funcionamento.</p>

<p align="center">
  
![Exemplo de GIF](https://github.com/mateusvicentin/flume-e-kafka/blob/main/gif2.gif)
</p>

<p>No terminal a esquerda é o <code>Producer</code> e a direita é o <code>Consumer</code></p>

<h2>Criando agentes do Flume</h2>

```shell
flume-ng agent --conf-file spool-to-logger.properties --name agent1 -Dflume.root.logger=WARN,console
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/bf2cd0e5-e63d-4c0c-8e04-7d4dd9f4e271" alt="img8">
</p>
<p>O Flume vai fazer a leitura pela pasta <code>Spool-Test</code> irei criar uma pasta chamada <code>Arquivos</code> e nele irei criar arquivos de texto e escrever alguma coisa nesse arquivo e apos isso arrastar esse arquivo de pastas</p>

<p align="center">
  
![Exemplo de GIF](https://github.com/mateusvicentin/flume-e-kafka/blob/main/gif1.gif?raw=true)
</p>

<h2>Criando topico no Kafka para comunicação com o Flume</h2>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic flume-mateus
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/d38692f4-1bc3-4ae9-b36f-092b1bdaa832" alt="img9">
</p>
<h4>Configurando o <code>Spool-to-Kafka</code> para que ele faça a função de <code>Procuder</code> e <code>Consumer</code></h4>
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/54eed2f3-e90d-4168-8895-888e42d92823" alt="img10">
</p>

<h4>Acessando o Flume</h4>

```shell
flume-ng agent --conf-file spool-to-kafka.properties --name agent2 -Dflume.root.logger=WARN,console
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/d0579e76-878a-4188-a906-e52c188b371e" alt="img11">
</p>
<h4>Acessando o Consumer do topico criado</h4>
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/1a5f8ae2-3aeb-4f96-b9b2-87e3baf0a69f" alt="img12">
</p>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic flume-mateus
```

<h2>Testando comunicação</h2>

<p align="center">
  
![Exemplo de GIF](https://github.com/mateusvicentin/flume-e-kafka/blob/main/gif3.gif)
</p>





