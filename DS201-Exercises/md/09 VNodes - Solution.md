# Exercise 09 – VNodes

In this exercise, you will:

- Understand how VNodes support partition distribution.

The ring of tokens and nodes make Apache Cassandra™ scalable and fault-tolerant, but
managing partitions on solely physical nodes causes problems. For example, when a physical
node goes down, it is necessary to redistribute partitions. This is where virtual nodes (or VNodes)
come in. VNodes help even the load when redistributing partitions across physical nodes.

In this exercise, we are going to change from using single token nodes to using vnodes.
Apache Cassandra™ doesn't allow changing the num_tokens settings after a node has joined
the cluster, so we must work around this a bit to make it work.

# Steps

```
1) Be sure neither of your nodes is running. Use the nodetool status command to check
their running status. If either is running, be sure to stop them using the nodetool
stopdaemon command.
```
```
/home/ubuntu/node1/resources/cassandra/bin/nodetool stopdaemon
/home/ubuntu/node2/resources/cassandra/bin/nodetool stopdaemon
```
```
2) Let's investigate the /home/ubuntu/node1/data/ directory. This is where we
configured Apache Cassandra™ to store all your data.
```
```
ubuntu@ds201-node1:~/node/data$ ls /home/ubuntu/node1/data
commit-log data hints saved-caches
```
```
NOTE: Deleting the data directory resets the node back to the initial state as it was
before we originally started it. However, we will retain our configuration settings since
these setting are stored elsewhere.
```
```
Delete the data/ directories and everything under them for BOTH nodes using the
following commands.
```
```
rm -rf /home/ubuntu/node1/data/
rm -rf /home/ubuntu/node2/data/
```

3) Edit cassandra.yaml. Uncomment num_tokens if necessary and set it to 128.
Comment out initial_token. Do this for both node1 and node2.

4) Restart /home/ubuntu/node1/bin/dse cassandra. Once it's up and running, start
your second node as well.

Notice both nodes logged the auto-generated token values that they are responsible for.

```
INFO [main] 2018- 01 - 22 20:02:54,198 BootStrapper.java:256 -
Generated random tokens. tokens are [3280177274095888020, -
5158766665979173809, -7886354694303886349, -2501223810857736082,
8422447039277277113, -316742407139291953, -4485056041791143981,
8137642830367557604, 3459375249450498851, - 46308853519293653 3, -
6638059177852600641, -7061402998660868729, 1005557257705443780,
2783102267919721916, -8965712838671361808, -4423000219300010471,
```
- 1707683781097732557, -3275488443846116208, -8489001999001053107,
- 4846093204303171732, -6036133155426227416, 2067160951308082487,
- 5811425082597558978, -7812169278445788215, 7551895539638164221,
- 8186705850729706345, 2543070728881051532, 7042356306829718634,
8887808421687547601, 8804336148487922110, -2167126362159814736, -
3811854764188651459, 1494901775105875074, 1427541021084505712,
8587727320636350576, 7412778408063927403, -691124132079339315,
8446395546700906257, -6712145296761140901, 3314423833765764848,
2789983795472271523, 5754671309836476536, -6409356491952196811,
2026143630593086411, -1080426218270477133, - 168568378091 0950810,
158763648126648627, 8010176010599555787, -1434870051236113913, -
562911142638631435, 7859024534560003294, -2183313046659894317,
1999234309368817769, 7025776015513968332, -1266269768355314174,
3669184028772933220, 2169059205780483577, - 7507933611484 494647,
9180271562234811886, -1201841407913735093, -7749018128518795642,
6410837384454082312, -4059349046558581028, -8883880718542901245,
- 8480953676445519026, 933418169284866419, -8023394544166368547,
6478356945185321666, -2237760891136255570, 2500932270541727332, -
5930712816769399620, 4688123977423727675, 3450446612031308820, -
8995763165883909032, -693410676877393915, -2742943296069400743,
276985317978405368, 6179661657117312842, -5688941169262227272,
5336304168996485054, 124504956738951558, - 536201373801 5302608,
5412346567329028339, -3635652649552254329, -6493150931698205441,
...

5) Run:

/home/ubuntu/node1/resources/cassandra/bin/nodetool status

Notice each node now has 128 tokens.


```
Datacenter: Cassandra
=====================
Status=Up/Down
|/ State=Normal/Leaving/Joining/Moving
-- Address Load Tokens Owns Host ID Rack
UN 127.0.0.1 138.6 KiB 128? 30cdb721-74e2- 4335 - af61-74d8b9fb445f rack
UN 127.0.0.2 111.38 KiB 128? 574dbde2-62db-48b8-9abb-d3e4bd2e1c0f rack
```
```
6) Now execute:
```
```
/home/ubuntu/node1/resources/cassandra/bin/nodetool ring
```
```
Notice that each node is responsible for several smaller sections of the ring.
```
Datacenter: Cassandra
==========
Address Rack Status State Load Owns Token
127.0.0.2 rack1 Up Normal 111.38 KiB? - 9169344042875408040
127.0.0.1 rack1 Up Normal 138.6 KiB? - 9071025037380885488
127.0.0.2 rack1 Up Normal 111.38 KiB? - 8995763165883909032
127.0.0.2 rack1 Up Normal 111.38 KiB? - 8965712838671361808
127.0.0.2 rack1 Up Normal 111.38 KiB? - 8883880718542901245
127.0.0.1 rack1 Up Normal 138.6 KiB? - 8821006236605210718
...


