Apache Zeppelin is an interactive analytics notebook compatible with
several backends, including [DCOS
Spark](https://docs.mesosphere.com/spark-1-7/).

DCOS Zeppelin bundles Apache Zeppelin with the DCOS Spark
distribution.  For complete Apache Zeppelin docs, please visit [their
website](https://zeppelin.incubator.apache.org/).

# Install

```
$ dcos package install zeppelin
```

## Install on DCOS 1.7 and above
On DCOS >= 1.7, you can now access Zeppelin at
`http://<dcos_url>/service/zeppelin/` after running the above command.

**Note:** Zeppelin uses websockets, which communicate over raw TCP
sockets, rather than HTTP.  On AWS DCOS 1.7, the ELB to which
`<dcos_url>` resolves will proxy HTTP traffic, but not TCP.  For
Zeppelin to work properly, you must downgrade the ELB port 80 proxy to
TCP.

## Install on a public agent
 
Alternately, you can deploy Zeppelin on a public agent by setting the
`acceptedResourceRoles` field of the Marathon app to
`["slave_public"]`.  You can then access Zeppelin By combining the
public IP address of the agent running Zeppelin with the `$PORT0` of
its marathon app.

# Usage

## Spark

Zeppelin is bundled with the Spark distribution specified by the
configuration variable `spark.uri`.  DCOS Zeppelin fetches this
distribution and sets the `SPARK_HOME` of its Spark client to this
location.  Executors will run in the docker image specified by
`spark.executor_docker_image`.  By default, these are configured to be
a recent DCOS Spark distribution.

### Testing

To test that Spark support in Zeppelin is working properly, run the
following minimal Spark job in a new Zeppelin note:

```
val rdd = sc.parallelize(1 to 5)
rdd.sum()
```

After a short period, "15" should be printed.

**NOTE**: The first Spark job you run on a new DCOS cluster may take
some time as the docker images are being pulled down.