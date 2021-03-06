---
title: collectd MySQL plugin
brief: Use this plugin to collect metrics from MySQL.
---

# MySQL collectd Plugin

_This is a directory consolidate all the metadata associated with the MySQL collectd plugin. The relevant code for the plugin can be found [here](https://github.com/signalfx/collectd/blob/master/src/mysql.c)_

- [Description](#description)
- [Requirements and Dependencies](#requirements-and-dependencies)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Metrics](#metrics)
- [License](#license)

### DESCRIPTION

This file describes the MySQL plugin for collectd. Use it to monitor MySQL database performance.

This plugin connects to a MySQL instance and reports on the values returned by a `SHOW STATUS` command. This includes the following:

  - Number of commands processed
  - Table and row operations (handlers)
  - State of the query cache
  - Status of MySQL threads
  - Network traffic

### REQUIREMENTS AND DEPENDENCIES

### Version information

| Software  | Version        |
|-----------|----------------|
| collectd  |  3.6 or later  |
| MySQL     |  4.x or later  |

### INSTALLATION

Follow these steps to install and configure this plugin:

1. Install the plugin.

  **Ubuntu 12.04, 14.04, & 15.04 and Debian 7 & 8:**

  This plugin is included with [SignalFx's collectd package](https://support.signalfx.com/hc/en-us/articles/208080123).

  **RHEL/CentOS 6.x & 7.x, and Amazon Linux 2014.09, 2015.03 & 2015.09**:

  Run the following command to install this plugin:

  ```
  yum install collectd-mysql
  ```

1. Download SignalFx's [sample configuration file](./10-mysql.conf) for this plugin.
1. Modify the sample configuration file as described in [Configuration](#configuration), below.
1. Add the following line to `/etc/collectd.conf`, replacing the example path with the location of the configuration file:

  ```
  include '/path/to/10-mysql.conf'
  ```

1. Restart collectd.

### CONFIGURATION

Using the example configuration file [`10-mysql.conf`](././10-mysql.conf) as a guide, provide values for the configuration options listed below that make sense for your environment and allow you to connect to the MySQL instance to be monitored.

| configuration option | definition | example value |
| ---------------------|------------|---------------|
| Database (in block declaration) | The value of the dimension `plugin_instance` that will be recorded for this database. | hostA_database1 |
| Host  | The host on which MySQL is running. | "10.128.8.2" |
| Socket | A socket that collectd can use to connect to the database. You may be able to find this value by looking at the command used to run MySQL on your server as follows: <code>ps auwxxx &#124; grep mysql<code> | "/var/run/mysqld/mysqld.sock" |
| User | A valid username that collectd can use to connect to MySQL. | "root"
| Password | Password for the username given in User. | "abcdABCD1." |
| Database (within block) | The name of the MySQL database to monitor. | "mysql_one" |

#### Note: Monitoring multiple instances
The sample configuration file is configured to illustrate how to configure this plugin to monitor multiple databases, on the same host or on different hosts.

To monitor just one database, include just one Database block and delete the others.

#### Note: Two different directives called "Database"
This plugin configuration file uses directives called “Database” in two different places: one in each block declaration, and one within each block.

The value of “Database” in the block declaration indicates the value of the  `plugin_instance` dimension that will be recorded for this database. The value of “Database” within the block indicates the `db_name` of the MySQL database to monitor using this configuration.

To illustrate the difference between these two uses of "Database", the example configuration given in [`10-mysql.conf`](././10-mysql.conf) directs collectd to collect metrics for three total MySQL databases: the databases named `mysql_one` and `mysql_two` on host 10.128.8.2, and the database named `mysql_one` on host 10.128.8.3.

### USAGE

Below are screen captures of dashboards created for this plugin by SignalFx, illustrating the metrics emitted by this plugin. The dashboards are included in this repository and can be imported into SignalFx or other monitoring products. [Click here to download](././Page_MySQL.json).

For general reference on how to monitor MySQL performance using this plugin, see [documentation on collectd.org](https://collectd.org/wiki/index.php/Plugin:MySQL).

**Monitoring multiple MySQL nodes**

![Example dashboard showing MySQL nodes](././img/MySQL nodes dashboard.png)

*Example dashboard showing performance of multiple MySQL nodes.*

**Monitoring a single MySQL node**

![Example dashboard showing a single MySQL host](././img/MySQL node dashboard.png)

*Example dashboard showing performance of a single MySQL node.*

### METRICS

For documentation of the metrics and dimensions emitted by this plugin, [click here](././docs).

#### Note: This plugin may not emit all listed metrics

This plugin will not emit metrics about features that are not used. For example, this plugin will not emit a count of an operation that has never occurred. For another example, this plugin will not emit metrics about the query cache if MySQL is not configured to use the query cache.

### LICENSE

License for this plugin can be found [in the header of the plugin](https://github.com/signalfx/collectd/blob/master/src/mysql.c).
