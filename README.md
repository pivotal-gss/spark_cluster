# spark_cluster #

**Vagrant template to provision a Spark Cluster**

# Details #

- See `Vagrantfile` for details and to make changes.
- Spark running as a standalone cluster. Tested with Spark 2.1.x (check Spark Connector Compatability).
- One head node Centos 7.4 machine and `N` worker (slave) machines [0-9].
- Spark running in standalone cluster mode.

# Prerequsites #

* [Vagrant](https://www.vagrantup.com/downloads.html)
* [Virtual Box](https://www.virtualbox.org/wiki/Downloads)
* Vagrant Hosts Plugin: `vagrant plugin install vagrant-hosts`
  * This allows us to provision the hosts files for all the instances.

# Usage #

* Clone this repository.

* [Download a pre-built Spark package](https://spark.apache.org/downloads.html) and place it into this directory; symlink as "spark.tgz".

* [Download the Spark Connector](https://network.pivotal.io/products/pivotal-gpdb) and place it into this directory.

* [Optional: Download the PostgreSQL JDBC Driver](https://jdbc.postgresql.org/download.html).

* Open up `Vagrantfile` in a text editor.

* Optional: Change the `N_WORKERS` to the number of desired worker hosts [0-9].

* Feel free to make other changes, e.g. RAM and CPU for each of the machines.

* When you're ready, just run `vagrant up` in the directory the `Vagrantfile` is in. 
  * By Default: Vagrant will spin up one "head node" and `N` worker nodes in a Spark standalone cluster.
  * You can start a standalone instance using: vagrant up hn0

# Testing #

* SSH in using `vagrant ssh hn0` or `vagrant ssh wn0`.

* Spark is running as `root`.

* The Spark WebUI should be available at `http://192.168.99.200:8080`.

```
sudo jps -ml
PID org.apache.spark.deploy.master.Master --host 192.168.99.200 --port 7077 --webui-port 8080 -h 192.168.99.200
```

* Setup Environment Vairables
  * `GSC_JAR=$(ls /vagrant/greenplum-spark_2.11-*.jar)`
  * `POSTGRES_JAR=$(ls /vagrant/postgresql-*.jar)`

* Run SCALA 

  Spark Connector 1.2: Read from Greenplum with Spark Connector / Write to Greenplum with JDBC: 

  * `sudo spark-shell --jars "${GSC_JAR},${POSTGRES_JAR}" --driver-class-path ${POSTGRES_JAR}`
  
  Spark Connector 1.3+: Read and Write to Greenplum with Spark Connector:

  * `sudo spark-shell --jars "${GSC_JAR}"`

# Cleanup #

* Shut down the cluster with `vagrant halt` and delete it with `vagrant destroy`. 

* You can always run `vagrant up` to turn on or build a brand new cluster.

# License #

See the LICENSE.txt file.
