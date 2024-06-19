<h1 align="center">Executando Arquivos com Flume e Kafka</h1>
<p>Este projeto tem como objetivo apresentar um relatório utilizando Flume e Kafka. Trata-se de um trabalho da matéria de Processamento de Fluxos Discretos e Contínuos de Dados, que será realizado por meio de uma máquina virtual no VirtualBox. Nessa máquina virtual, o Kafka e o Flume já estão configurados.</p>
<h2>Apache Kafka</h2>
<p>O Apache Kafka é uma plataforma distribuída de streaming que pode realizar as seguintes funções:</p>
<ul>
    <li>Publicar e assinar fluxos de registros (também chamados de eventos).</li>
    <li>Armazenar fluxos de registros de forma eficiente e ordenada.</li>
    <li>Processar fluxos de registros em tempo real.</li>
</ul>
<p>O Kafka é altamente escalável e pode ser usado para uma variedade de casos de uso, incluindo análise em tempo real, monitoramento de atividades e coleta de logs.</p>
<h2>Apache Flume</h2>
<p>O Apache Flume é um serviço distribuído, confiável e disponível para coletar, agregar e mover grandes quantidades de dados de log de várias fontes para um repositório centralizado.</p>
<ul>
    <li>Coleta arquivos de log de várias fontes, como servidores web, servidores de aplicativos e dispositivos de rede.</li>
    <li>Pode ser utilizado para coletar dados de texto de computadores e outras fontes.</li>
    <li>Flume é altamente configurável, permitindo a criação de pipelines de dados personalizados para atender a diferentes necessidades de coleta e ingestão de dados.</li>
</ul>
<p>Com Flume, é possível canalizar dados de diversas fontes para sistemas como Hadoop, HDFS, HBase ou Kafka, facilitando a análise e o processamento de grandes volumes de dados.</p>
<p>Neste projeto, utilizaremos o Flume para coletar dados de várias fontes e enviar esses dados para o Kafka, onde serão processados e analisados em tempo real.</p>


<h2>Iniciando o Servidor do Kafka</h2>
<p>Para que o <code>Producer</code> e o <code>Consumer</code> funcionem corretamente, é necessário iniciar o servidor do Kafka. Abaixo estão os passos para iniciar o servidor:</p>
<p><strong>Producer:</strong> O Producer é responsável por enviar mensagens para um tópico. Esse tópico é gerenciado e armazenado por um cluster do Kafka. Ele permite que diferentes fontes de dados enviem informações de forma eficiente e escalável.</p>
<p><strong>Consumer:</strong> O Consumer subscreve no tópico utilizado pelo Producer. Ele lê e processa as mensagens enviadas pelo Producer, permitindo que os dados sejam consumidos e analisados por diferentes aplicações ou serviços.</p>
<p>Abaixo está um exemplo básico de como iniciar o servidor do Kafka e utilizar um Producer e um Consumer:</p>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-server-start.sh /home/puc/kafka_2.11-1.0.0/config/server.properties
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/b7cb3ee6-a21b-44ac-b091-048db4f1c48b" alt="img1">
</p>
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/f8d0c25d-bd2b-462d-ae50-3bf19bffdbdb" alt="img2">
</p>

<h2>Criando Tópicos no Kafka</h2>
<p>Vamos criar um tópico chamado <code>mateusvicentin</code> como exemplo do funcionamento do Kafka.</p>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic mateusvicentin
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/1e82c41c-b6f1-4978-9e4c-05d05d05c7d0" alt="img3">
</p>

<h2>Listando Tópicos no Kafka</h2>
<p>Vou utilizar o comando <code>--list</code> para visualizar todos os tópicos que já foram criados, inclusive o que criei: <code>mateusvicentin</code>.</p>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-topics.sh --list --zookeeper localhost:2181
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/08f30aad-9020-43be-b5b3-e910acb5ac48" alt="img4">
</p>

<h2>Inserindo Strings no Tópico Criado no Kafka</h2>
<p>Vamos utilizar dois comandos: o <code>Producer</code> e o <code>Consumer</code>. Um será responsável por inserir as strings, e o outro será responsável por ler essas strings.</p>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic mateusvicentin
```
```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic mateusvicentin
```
<h4>Producer</h4>
<p>O responsável por enviar as mensagens ao tópico.</p>

<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/b73b3667-b2fd-43d4-897b-29df6900a60d" alt="img5">
</p>

<h4>Consumer</h4>
<p>O responsável por receber e fazer a leitura das mensagens.</p>


<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/eb8fc47c-69f1-484e-aadc-e8d42805aa75" alt="img6">
</p>

<h2>Testando a Leitura</h2>
<p>Para testar a leitura, fiz a gravação da tela onde foram abertos dois terminais: um contendo o <code>Producer</code> e outro contendo o <code>Consumer</code>, para que seja possível visualizar em tempo real o funcionamento.</p>

<p align="center">
  
![Exemplo de GIF](https://github.com/mateusvicentin/flume-e-kafka/blob/main/gif2.gif)
</p>

<p>No terminal à esquerda está o <code>Producer</code> e à direita está o <code>Consumer</code>.</p>
<h2>Criando Agentes do Flume</h2>

```shell
flume-ng agent --conf-file spool-to-logger.properties --name agent1 -Dflume.root.logger=WARN,console
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/bf2cd0e5-e63d-4c0c-8e04-7d4dd9f4e271" alt="img8">
</p>
<p>O Flume vai ler da pasta <code>Spool-Test</code>. Vou criar uma pasta chamada <code>Arquivos</code> e dentro dela vou colocar arquivos de texto onde escreverei algo. Após isso, vou arrastar esses arquivos entre as pastas.</p>

<p align="center">
  
![Exemplo de GIF](https://github.com/mateusvicentin/flume-e-kafka/blob/main/gif1.gif?raw=true)
</p>

<h2>Criando Tópico no Kafka para Comunicação com o Flume</h2>
<p>Vou criar um tópico chamado <code>flume-mateus</code> para utilizar nesta parte do projeto.</p>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic flume-mateus
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/d38692f4-1bc3-4ae9-b36f-092b1bdaa832" alt="img9">
</p>
<p>Configurando o <code>Spool-to-Kafka</code> para que ele funcione como <code>Producer</code> e <code>Consumer</code></p>

<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/54eed2f3-e90d-4168-8895-888e42d92823" alt="img10">
</p>
<p>Vou colocar em <code>agent2.sinks.sink1.topic =</code> 'flume-mateus' porque este é o nome do tópico que criei anteriormente.</p>

<h4>Acessando o Apache Flume</h4>

```shell
flume-ng agent --conf-file spool-to-kafka.properties --name agent2 -Dflume.root.logger=WARN,console
```
<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/d0579e76-878a-4188-a906-e52c188b371e" alt="img11">
</p>
<h4>Acessando o Consumer do tópico criado</h4>

<p align="center">
  <img src="https://github.com/mateusvicentin/flume-e-kafka/assets/31457038/1a5f8ae2-3aeb-4f96-b9b2-87e3baf0a69f" alt="img12">
</p>

```shell
sudo /home/puc/kafka_2.11-1.0.0/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic flume-mateus
```

<h2>Testando Comunicação</h2>

<p align="center">
  
![Exemplo de GIF](https://github.com/mateusvicentin/flume-e-kafka/blob/main/gif3.gif)
</p>
<p>No terminal à esquerda está o <code>Logger(Flume)</code> e à direita está o <code>Consumer</code>. O logger caso de algum erro ou problema irá mostrar alguma informação.</p>





