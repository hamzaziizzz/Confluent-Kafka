# Installing Confluent-Kafka Using Docker

## Step - 1) Installing Docker

Check if Docker is already installed by running the following command.

```docker
docker --version
```

Your output should be similar to the following:

```terminal
Docker version 20.10.5, build 55c4c88
```

And, if docker is not already installed then follow this tutorial by `DigitalOcean` to install docker on your system: [How To Install and Use Docker on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)

## Step - 2) Installing Docker Compose

Check if Docker Compose is already installed by running the following command.

```docker
docker-compose --version
```

Your output should be similar to the following:

```terminal
docker-compose version 1.29.2, build 5becea4c
```

And, if docker-compose is not already installed then follow this tutorial by `DigitalOcean` to install docker-compose on your system: [How To Install and Use Docker Compose on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04)

## Step - 3) Installing Confluent-Kafka

### Create Directory

Create a directory by running the following command in your terminal.

```bash
mkdir Confluent-Kafka
```

### Download `docker-compose.yml`

Download `docker-compose.yml` file for *confluent-kafka* from the following link.

<https://github.com/confluentinc/cp-all-in-one/blob/7.4.0-post/cp-all-in-one/docker-compose.yml>

And move this file to `Confluent-Kafka` directory by running the following command.

```bash
mv <path_where_docker-compose.yml_file_downloaded> ~/Confluent-Kafka/docker-compose.yml
```

Then, change the directory

```bash
cd Confluent-Kafka/
```

### Custom Changes in `docker-compose.yml`

Create a new file named as `server.properties` by running the following command.

```bash
nano server.properties
```

Changes made in `server.properties` file are as follows:

**Line Number 34**

```properties
listeners=PLAINTEXT://:9092
```

**And, Line Number 38**

```properties
advertised.listeners=PLAINTEXT://<host-machine-ip>:9092
```

***Note:*** *Replace `<host-machine-ip>` with the `actual ip address`*

Now, copy the following and paste it into the nano editor.

```properties
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

#
# This configuration file is intended for use in ZK-based mode, where Apache ZooKeeper is required.
# See kafka.server.KafkaConfig for additional details and defaults
#

############################# Server Basics #############################

# The id of the broker. This must be set to a unique integer for each broker.
broker.id=0

############################# Socket Server Settings #############################

# The address the socket server listens on. If not configured, the host name will be equal to the value of
# java.net.InetAddress.getCanonicalHostName(), with PLAINTEXT listener name, and port 9092.
#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
listeners=PLAINTEXT://:9092

# Listener name, hostname and port the broker will advertise to clients.
# If not set, it uses the value for "listeners".
advertised.listeners=PLAINTEXT://<host-machine-ip>:9092

# Maps listener names to security protocols, the default is for them to be the same. See the config documentation for more details
listener.security.protocol.map=PLAINTEXT:PLAINTEXT,SSL:SSL,SASL_PLAINTEXT:SASL_PLAINTEXT,SASL_SSL:SASL_SSL

# The number of threads that the server uses for receiving requests from the network and sending responses to the network
num.network.threads=3

# The number of threads that the server uses for processing requests, which may include disk I/O
num.io.threads=8

# The send buffer (SO_SNDBUF) used by the socket server
socket.send.buffer.bytes=102400

# The receive buffer (SO_RCVBUF) used by the socket server
socket.receive.buffer.bytes=102400

# The maximum size of a request that the socket server will accept (protection against OOM)
socket.request.max.bytes=104857600


############################# Log Basics #############################

# A comma separated list of directories under which to store log files
log.dirs=/var/lib/kafka

# The default number of log partitions per topic. More partitions allow greater
# parallelism for consumption, but this will also result in more files across
# the brokers.
num.partitions=1

# The number of threads per data directory to be used for log recovery at startup and flushing at shutdown.
# This value is recommended to be increased for installations with data dirs located in RAID array.
num.recovery.threads.per.data.dir=1

############################# Internal Topic Settings  #############################
# The replication factor for the group metadata internal topics "__consumer_offsets" and "__transaction_state"
# For anything other than development testing, a value greater than 1 is recommended to ensure availability such as 3.
offsets.topic.replication.factor=1
transaction.state.log.replication.factor=1
transaction.state.log.min.isr=1

############################# Log Flush Policy #############################

# Messages are immediately written to the filesystem but by default we only fsync() to sync
# the OS cache lazily. The following configurations control the flush of data to disk.
# There are a few important trade-offs here:
#    1. Durability: Unflushed data may be lost if you are not using replication.
#    2. Latency: Very large flush intervals may lead to latency spikes when the flush does occur as there will be a lot of data to flush.
#    3. Throughput: The flush is generally the most expensive operation, and a small flush interval may lead to excessive seeks.
# The settings below allow one to configure the flush policy to flush data after a period of time or
# every N messages (or both). This can be done globally and overridden on a per-topic basis.

# The number of messages to accept before forcing a flush of data to disk
#log.flush.interval.messages=10000

# The maximum amount of time a message can sit in a log before we force a flush
#log.flush.interval.ms=1000

############################# Log Retention Policy #############################

# The following configurations control the disposal of log segments. The policy can
# be set to delete segments after a period of time, or after a given size has accumulated.
# A segment will be deleted whenever *either* of these criteria are met. Deletion always happens
# from the end of the log.

# The minimum age of a log file to be eligible for deletion due to age
log.retention.hours=168

# A size-based retention policy for logs. Segments are pruned from the log unless the remaining
# segments drop below log.retention.bytes. Functions independently of log.retention.hours.
#log.retention.bytes=1073741824

# The maximum size of a log segment file. When this size is reached a new log segment will be created.
#log.segment.bytes=1073741824

# The interval at which log segments are checked to see if they can be deleted according
# to the retention policies
log.retention.check.interval.ms=300000

############################# Zookeeper #############################

# Zookeeper connection string (see zookeeper docs for details).
# This is a comma separated host:port pairs, each corresponding to a zk
# server. e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002".
# You can also append an optional chroot string to the urls to specify the
# root directory for all kafka znodes.
zookeeper.connect=localhost:2181

# Timeout in ms for connecting to zookeeper
zookeeper.connection.timeout.ms=18000

##################### Confluent Metrics Reporter #######################
# Confluent Control Center and Confluent Auto Data Balancer integration
#
# Uncomment the following lines to publish monitoring data for
# Confluent Control Center and Confluent Auto Data Balancer
# If you are using a dedicated metrics cluster, also adjust the settings
# to point to your metrics kakfa cluster.
#metric.reporters=io.confluent.metrics.reporter.ConfluentMetricsReporter
#confluent.metrics.reporter.bootstrap.servers=localhost:9092
#
# Uncomment the following line if the metrics cluster has a single broker
#confluent.metrics.reporter.topic.replicas=1

############################# Group Coordinator Settings #############################

# The following configuration specifies the time, in milliseconds, that the GroupCoordinator will delay the initial consumer rebalance.
# The rebalance will be further delayed by the value of group.initial.rebalance.delay.ms as new members join the group, up to a maximum of max.poll.interval.ms.
# The default value for this is 3 seconds.
# We override this to 0 here as it makes for a better out-of-the-box experience for development and testing.
# However, in production environments the default value of 3 seconds is more suitable as this will help to avoid unnecessary, and potentially expensive, rebalances during application startup.
group.initial.rebalance.delay.ms=0


############################# Confluent Authorizer Settings  #############################

# Uncomment to enable Confluent Authorizer with support for ACLs, LDAP groups and RBAC
#authorizer.class.name=io.confluent.kafka.security.authorizer.ConfluentServerAuthorizer
# Semi-colon separated list of super users in the format <principalType>:<principalName>
#super.users=
# Specify a valid Confluent license. By default free-tier license will be used
#confluent.license=
# Replication factor for the topic used for licensing. Default is 3.
confluent.license.topic.replication.factor=1

# Uncomment the following lines and specify values where required to enable CONFLUENT provider for RBAC and centralized ACLs
# Enable CONFLUENT provider
#confluent.authorizer.access.rule.providers=ZK_ACL,CONFLUENT
# Bootstrap servers for RBAC metadata. Must be provided if this broker is not in the metadata cluster
#confluent.metadata.bootstrap.servers=PLAINTEXT://127.0.0.1:9092
# Replication factor for the metadata topic used for authorization. Default is 3.
confluent.metadata.topic.replication.factor=1

# Replication factor for the topic used for audit logs. Default is 3.
confluent.security.event.logger.exporter.kafka.topic.replicas=1

# Listeners for metadata server
#confluent.metadata.server.listeners=http://0.0.0.0:8090
# Advertised listeners for metadata server
#confluent.metadata.server.advertised.listeners=http://127.0.0.1:8090

############################# Confluent Data Balancer Settings  #############################

# The Confluent Data Balancer is used to measure the load across the Kafka cluster and move data
# around as necessary. Comment out this line to disable the Data Balancer.
confluent.balancer.enable=true

# By default, the Data Balancer will only move data when an empty broker (one with no partitions on it)
# is added to the cluster or a broker failure is detected. Comment out this line to allow the Data
# Balancer to balance load across the cluster whenever an imbalance is detected.
#confluent.balancer.heal.uneven.load.trigger=ANY_UNEVEN_LOAD

# The default time to declare a broker permanently failed is 1 hour (3600000 ms).
# Uncomment this line to turn off broker failure detection, or adjust the threshold
# to change the duration before a broker is declared failed.
#confluent.balancer.heal.broker.failure.threshold.ms=-1

# Edit and uncomment the following line to limit the network bandwidth used by data balancing operations.
# This value is in bytes/sec/broker. The default is 10MB/sec.
#confluent.balancer.throttle.bytes.per.second=10485760

# Capacity Limits -- when set to positive values, the Data Balancer will attempt to keep
# resource usage per-broker below these limits.
# Edit and uncomment this line to limit the maximum number of replicas per broker. Default is unlimited.
#confluent.balancer.max.replicas=10000

# Edit and uncomment this line to limit what fraction of the log disk (0-1.0) is used before rebalancing.
# The default (below) is 85% of the log disk.
#confluent.balancer.disk.max.load=0.85

# Edit and uncomment these lines to define a maximum network capacity per broker, in bytes per
# second. The Data Balancer will attempt to ensure that brokers are using less than this amount
# of network bandwidth when rebalancing.
# Here, 10MB/s. The default is unlimited capacity.
#confluent.balancer.network.in.max.bytes.per.second=10485760
#confluent.balancer.network.out.max.bytes.per.second=10485760

# Edit and uncomment this line to identify specific topics that should not be moved by the data balancer.
# Removal operations always move topics regardless of this setting.
#confluent.balancer.exclude.topic.names=

# Edit and uncomment this line to identify topic prefixes that should not be moved by the data balancer.
# (For example, a "confluent.balancer" prefix will match all of "confluent.balancer.a", "confluent.balancer.b",
# "confluent.balancer.c", and so on.)
# Removal operations always move topics regardless of this setting.
#confluent.balancer.exclude.topic.prefixes=

# The replication factor for the topics the Data Balancer uses to store internal state.
# For anything other than development testing, a value greater than 1 is recommended to ensure availability.
# The default value is 3.
confluent.balancer.topic.replication.factor=1

################################## Confluent Telemetry Settings  ##################################

# To start using Telemetry, first generate a Confluent Cloud API key/secret. This can be done with
# instructions at https://docs.confluent.io/current/cloud/using/api-keys.html. Note that you should
# be using the '--resource cloud' flag.
#
# After generating an API key/secret, to enable Telemetry uncomment the lines below and paste
# in your API key/secret.
#
#confluent.telemetry.enabled=true
#confluent.telemetry.api.key=<CLOUD_API_KEY>
#confluent.telemetry.api.secret=<CCLOUD_API_SECRET>
```

Save the the file by pressing `Ctrl+X` &rarr; `Y` &rarr; `Enter`.

Now, edit the `docker-compose.yml` file as follows:

```bash
nano docker-compose.yml
```

Replace these lines *(i.e., line number 26 and 27)*

```yaml
KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://broker:29092,PLAINTEXT_HOST://localhost:9092
```

with the following lines

```yaml
KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,my_listener:PLAINTEXT
KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://broker:29092,my_listener://<host-machine-ip>:9092
```

***Note:*** *Replace `<host-machine-ip>` with the `actual ip address`*

Add the following lines at the end of the broker service

```yaml
    volumes:
      - ./server.properties:/etc/kafka/server.properties
```

## Step - 4) Start the docker-compose.yml file

Start the Confluent Platform stack with the `-d` option to run in detached mode:

```docker
docker-compose up -d
```

Each Confluent Platform component starts in a separate container. Your output should resemble:

```terminal
Starting broker ... done
Starting schema-registry ... done
Starting connect         ... done
Starting rest-proxy      ... done
Starting ksqldb-server   ... done
Starting ksql-datagen    ... done
Starting ksqldb-cli      ... done
Starting control-center  ... done
```

Verify that the services are up and running:

```docker
docker-compose ps
```

Your output should resemble:

```terminal
   Name                    Command               State                               Ports
--------------------------------------------------------------------------------------------------------------
broker            /etc/confluent/docker/run        Up      0.0.0.0:9092->9092/tcp,:::9092->9092/tcp,
                                                         0.0.0.0:9101->9101/tcp,:::9101->9101/tcp
connect           /etc/confluent/docker/run        Up      0.0.0.0:8083->8083/tcp,:::8083->8083/tcp, 9092/tcp
control-center    /etc/confluent/docker/run        Up      0.0.0.0:9021->9021/tcp,:::9021->9021/tcp
ksql-datagen      bash -c echo Waiting for K ...   Up
ksqldb-cli        /bin/sh                          Up
ksqldb-server     /etc/confluent/docker/run        Up      0.0.0.0:8088->8088/tcp,:::8088->8088/tcp
rest-proxy        /etc/confluent/docker/run        Up      0.0.0.0:8082->8082/tcp,:::8082->8082/tcp
schema-registry   /etc/confluent/docker/run        Up      0.0.0.0:8081->8081/tcp,:::8081->8081/tcp
```

After a few minutes, if the state of any component isn't **Up**, run the `docker-compose up -d` command again, or try `docker-compose restart <image-name>`, for example:

```docker
docker-compose restart control-center
```
