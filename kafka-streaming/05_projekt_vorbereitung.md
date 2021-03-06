![Datenstrom des Aktienprojektes.](./assets/aktien.png "Datenstrom des Aktienprojektes.")

<center style="font-size: 75%;">Datenstrom des Aktienprojektes.</center>

# _Topic_ anlegen

Bevor mit der Programmierung des _Producers_ (Börse) oder der _Consumer_ (Aktionär\*innen) begonnen wird, muss zunächst das _Topic_ "Aktienkurse" erstellt werden. Hierbei ist wichtig, dass `2` Partitionen angelegt werden, damit sich, wie in der obigen Abbildung, zwei Aktienkurse speichern lassen. Dazu kann das mit _Apache Kafka_ mitgelieferte Skript `kafka-topics.sh` verwendet werden:

```bash
kafka-topics.sh \
    --create \
    --topic "Aktienkurse" \
    --replication-factor 1 \
    --partitions 2 \
    --bootstrap-server=localhost:9092
```{{execute T1}}

Falls das _Topic_ erfolgreich angelegt wurde, erscheint folgende Ausgabe:

```bash
Created topic Aktienkurse.
```

Mit dem folgenden Aufruf kann verifiziert werden, dass zwei Partitionen in dem _Topic_ "Aktienkurse" erstellt worden sind.

```bash
kafka-topics.sh \
    --describe \
    --topic "Aktienkurse" \
    --bootstrap-server localhost:9092
```{{execute T1}}

Erwartete Ausgabe:

```bash
Topic: Aktienkurse      TopicId: xxxxxxxxxxxxxxxxxxxxxx PartitionCount: 2       ReplicationFactor: 1    Configs: segment.bytes=1073741824
        Topic: Aktienkurse      Partition: 0    Leader: 0       Replicas: 0     Isr: 0
        Topic: Aktienkurse      Partition: 1    Leader: 0       Replicas: 0     Isr: 0
```

# _Python-Paket_ installieren

Da die Umsetzung in _Python_ geschieht, wird das _Python-Paket_ `kafka-python` verwendet, um die Realisierung durchzuführen. Mit folgendem Befehl kann das Modul installiert werden:

`python3 -m pip install kafka-python==2.0.2`{{execute T1}}

Dieses Paket ist lediglich bis zur _Apache Kafka_ Version 2.4 getestet (benutzte Version: 3.1.0), allerdings ist das Server-Protokoll rückwärtskompatibel, weshalb dieses Paket keine Probleme bereiten sollte [5].
