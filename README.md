<h1 align="center">Executando Arquvios com Flume e Kafka </h1>
<p>O projeto tem como objetivo apresentar um relatório utilizando Flume e Kafka. Esse é um projeto da materia de Processamento de fluxos discretos e contínuos de dados, que será feito por meio de uma máquina no VirtualBox disponibilizada pelo professor, onde já está configurado o Kafka e Flume na maquina virtual</p>


<h2>Inicinado o Servidor do Kafka</h2>

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

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic mateusvicentin
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/1e82c41c-b6f1-4978-9e4c-05d05d05c7d0" alt="img3">
</p>

<h2>Listando topicos no Kafka</h2>

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
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/b73b3667-b2fd-43d4-897b-29df6900a60d" alt="img5">
</p>

<h4>Consumer</h4>
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/eb8fc47c-69f1-484e-aadc-e8d42805aa75" alt="img6">
</p>

<h2>Testando a leitura</h2>
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





