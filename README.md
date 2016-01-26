
Source
======

This repository has been forked from   [influxdb-java](https://github.com/influxdata/influxdb-java) with
a single goal of renaming the package from `org.influxdb`  to  `org.influxdb08` because
I needed to be able to build shaded jar with InfluxDb java client libraries for both
InfluxDb 0.8 and 0.9 (and later). Libraries for 0.8 and 0.9 in the original project
are very different and completely incompatible but share the same name of the package
and many classes so they can't be used simultaneously.


influxdb-java
=============

A pure Java library to access the REST API of a InfluxDB database.

This implementation is meant as a Java rewrite of the influxdb-go package.
All low level REST Api calls are available.

Typical usage looks like:

```java
InfluxDB influxDB = InfluxDBFactory.connect("http://172.17.0.2:8086", "root", "root");

influxDB.createDatabase("aTimeSeries");

Serie serie1 = new Serie.Builder("serie2Name")
			.columns("column1", "column2")
			.values(System.currentTimeMillis(), 1)
			.values(System.currentTimeMillis(), 2)
			.build();
Serie serie2 = new Serie.Builder("serie2Name")
			.columns("column1", "column2")
			.values(System.currentTimeMillis(), 1)
			.values(System.currentTimeMillis(), 2)
			.build();
influxDB.write("aTimeSeries", TimeUnit.MILLISECONDS, serie1, serie2);

```

### Maven
```
		<dependency>
			<groupId>org.influxdb</groupId>
			<artifactId>influxdb-java</artifactId>
			<version>1.5</version>
		</dependency>
```


For additional usage examples have a look at [InfluxDBTest.java](https://github.com/influxdb/influxdb-java/blob/master/src/test/java/org/influxdb/InfluxDBTest.java "InfluxDBTest.java")

### Build Requirements

* Java 1.6+
* Maven 3.0+
* Docker daemon running

Maven will run tests during build process using a docker image with influxdb actual image is majst01/influxdb-java.
This docker image is pulled during the first test run which will take some time. So the first test execution will fail because the image to pull is not there.
You can check with:

```
    $ docker images | grep majst01
majst01/influxdb-java      latest              50256afac0c9        About an hour ago   298.7 MB

```

Then you can build influxdb-java with all tests with:

    $ mvn clean install

If you don't have Docker running locally, you can skip tests with -DskipTests flag set to true:

    $ mvn clean install -DskipTests=true



### Publishing

This is a
[link](https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide)
to the sonatype oss guide to publishing. I'll update this section once
the [jira ticket](https://issues.sonatype.org/browse/OSSRH-9728) is
closed and I'm able to upload artifacts to the sonatype repositories.
