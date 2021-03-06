

[::Go back to Oozie Documentation Index::](index.html)

# Hadoop Configuration

## Hadoop Services Whitelisting

Oozie supports whitelisting Hadoop services (JobTracker, HDFS), via 2 configuration properties:


```
...
    <property>
        <name>oozie.service.HadoopAccessorService.jobTracker.whitelist</name>
        <value> </value>
        <description>
            Whitelisted job tracker for Oozie service.
        </description>
    </property>
    <property>
        <name>oozie.service.HadoopAccessorService.nameNode.whitelist</name>
        <value> </value>
        <description>
            Whitelisted job tracker for Oozie service.
        </description>
    </property>
...
```

The value must follow the pattern `[AUTHORITY,...]`. Where `AUTHORITY` is the `HOST:PORT` of
the Hadoop service (JobTracker, HDFS).

If the value is empty any HOST:PORT is accepted. Empty is the default value.

## Hadoop Default Configuration Values

Oozie supports Hadoop configuration equivalent to the Hadoop `*-site.xml` files.

The configuration property in the `oozie-site.xml` is `oozie.service.HadoopAccessorService.hadoop.configurations`
and its value must follow the pattern `[<AUTHORITY>=<HADOOP_CONF_DIR>,]*`. Where `<AUTHORITY>` is the `HOST:PORT` of
the Hadoop service (JobTracker, HDFS). The `<HADOOP_CONF_DIR>` is a Hadoop configuration directory. If the specified
 directory is a relative path, it will be looked under the Oozie configuration directory. And absolute path can
 also be specified. Oozie will load the Hadoop `*-site.xml` files in the following order: core-site.xml, hdfs-site.xml,
 mapred-site.xml, yarn-site.xml, hadoop-site.xml, ssl-client.xml.

In addition to explicit authorities, a '*' wildcard is supported. The configuration file associated with the wildcard
will be used as default if there is no configuration for the requested Hadoop service.

For example, the configuration in the `oozie-site.xml` would look like:


```
...
    <property>
        <name>oozie.service.HadoopAccessorService.hadoop.configurations</name>
        <value>*=hadoop-conf,jt-bar:8021=bar-cluster,nn-bar:8020=bar-cluster</value>
    </property>
...
```

The Hadoop configuration files use the Hadoop configuration syntax.

By default Oozie defines `*=hadoop-conf` and the default values of the `hadoop-site.xml` file are:


```
<configuration>
   <property>
        <name>mapreduce.jobtracker.kerberos.principal</name>
        <value>mapred/_HOST@LOCALREALM</value>
    </property>
    <property>
      <name>yarn.resourcemanager.principal</name>
      <value>yarn/_HOST@LOCALREALM</value>
    </property>
    <property>
        <name>dfs.namenode.kerberos.principal</name>
        <value>hdfs/_HOST@LOCALREALM</value>
    </property>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

## Limitations

All actions in a workflow application must interact with the same Hadoop JobTracker and NameNode.

[::Go back to Oozie Documentation Index::](index.html)


