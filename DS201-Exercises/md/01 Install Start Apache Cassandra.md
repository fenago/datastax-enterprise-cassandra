# Exercise 1 – Install and Start Apache Cassandra™

In this exercise, you will:

- SSH into your cloud instance using the IP provided by your instructor
- In root directory, find and install DataStax Enterprise from a tarball instance
- Start DataStax Enterprise and verify that it is running with nodetool

To do the remaining exercises, you must complete this one successfully. If you have any
questions, alert your instructor.

# Steps

If you are taking this course in a class environment, your instructor will assign you and IP address
of a cloud instance you will SSH into.

```
1) SSH to the cloud instance provided using the IP address provided to you by the instructor.
```
```
ssh -i your-key-file-name ubuntu@<student ip>
```
```
2) In your home directory, list the contents and notice the presence of dse- 6 .0.0-
bin.tar.gz. For this course, we will install DataStax Enterprise by extracting it locally.
Do so by running the following command:
```
```
tar -xf dse-6.0.0-bin.tar.gz
```
```
3) List the contents of the directory again. You now have a dse-6.0.0/ directory. Rename
this directory to node/ by executing the following command:
```
```
mv dse-6.0.0 node
```
```
4) We have a simple auto-configuration script for you to tweak some values so that we can
run Apache Cassandra™ multiple times on the same virtual machine instance. Run the
script like so:
```
```
labwork/config_node
```
```
5) CD into the directory with DataStax Enterprise by running:
```
```
cd node/bin
```

```
6) Start DataStax Enterprise (and thus, Apache Cassandra™) from here by running:
```
```
./dse cassandra
```
```
DataStax Enterprise might take a few minutes to start up! Be patient. You may need to
hit enter to get back a prompt.
```
```
7) To check to see if DataStax Enterprise has started, run the following:
```
```
./dsetool status
```
If it started up properly, you will see something like:

```
DC: Cassandra Workload: Cassandra Graph: no
======================================================
Status=Up/Down
|/ State=Normal/Leaving/Joining/Moving
-- Address Load Owns Token Rack Health [0,1]
UN 127.0.0.1 109.61 KiB? 0 rack1 0.
```
```
8) Leave it up and running for the next exercise. Congrats! You installed and started
DataStax Enterprise.
```

