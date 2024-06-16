<h1 align="center">Executando arquvios com Flume e Kafka </h1>
<p>O projeto tem como objetivo apresentar um relatório utilizando Flume e Kafka. Esse é um projeto da materia de Processamento de fluxos discretos e contínuos de dados, que será feito por meio de uma máquina no VirtualBox, contendo os seguintes itens:</p>
<ul>
  <li><strong>Introdução</strong> - Apresentação do tema e das ferramentas utilizadas na prática.</li>
  <li><strong>Objetivos</strong> - Metas a serem alcançadas com a execução da prática.</li>
  <li><strong>Experimentos</strong> - Descrição dos experimentos realizados durante a prática.</li>
  <li><strong>Conclusão</strong> - Análise dos resultados obtidos e considerações finais sobre a prática executada.</li>
</ul>


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



