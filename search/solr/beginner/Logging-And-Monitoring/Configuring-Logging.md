# Configuring Logging

Solr logs provide valuable insights into the system's activities and are crucial for monitoring its performance. Modifying the default logging configuration can be done in several ways.

## Customizing Logging Configuration

In addition to the methods described below, you can also configure which request parameters, such as those sent with queries, are logged using an additional parameter called `logParamsList`. 

## Adjusting Temporary Logging Settings

You can modify the logging output in Solr temporarily through the Admin Web interface. Simply navigate to the LOGGING section. Keep in mind that changes made here are only applicable during the current session and are not saved for future sessions. 

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/c78fc118-62f1-4bc4-b18c-c98cf792848f)

In the Admin Web interface of Solr, you can easily adjust the logging level for various log categories. Fortunately, if a category's logging level is not explicitly set, it inherits the logging level from its parent category. This means that adjusting the logging level of a parent category can impact multiple sub-categories at once.

When you choose the "Level" option, you'll encounter the following menu:

![image](https://github.com/shannee-07/Apache-Solr-doc/assets/121802518/0770a6f2-402f-4f85-a505-b559168876f0)

Directories are displayed along with their current logging levels. The Log Level Menu appears over these directories. To adjust the logging level for a specific directory, simply select it and click on the corresponding log level button.

Here are the available log-level settings:

| Level  | Result                                 |
|--------|----------------------------------------|
| FINEST | Reports everything.                    |
| FINE   | Reports everything but the least important messages. |
| CONFIG | Reports configuration errors.          |
| INFO   | Reports everything but normal status. |
| WARN   | Reports all warnings.                  |
| SEVERE | Reports only the most severe warnings. |
| OFF    | Turns off logging.                     |
| UNSET  | Removes the previous log setting.     |

## Overview of Log4j in SolrCloud

In Apache SolrCloud, Log4j is the commonly used logging framework. Here's a brief overview of how it operates:

### Configuration

Log4j in SolrCloud is configured via an XML configuration file (log4j2.xml stored inside SOLR_HOME/server/resources). This file specifies where log messages should be output, their format, and the conditions under which they should be logged.

### Loggers

Loggers are named entities that _categorize log messages_, associating each with a specific part of the Solr codebase. Solr employs various loggers for different components and functionalities.

### Appenders

Appenders define the _destination of log messages_. They can send logs to consoles, files, databases, or remote services. Solr typically configures appenders to output logs to files within the logs directory of the Solr installation.

### Levels

Log levels determine the severity of a log message. Log4j defines several standard levels like DEBUG, INFO, WARN, ERROR, and FATAL. Each logger can be set to log messages at a specific level or above.



### Log Statements

Developers insert log statements throughout the codebase to record events or provide diagnostic information. These statements specify the log level and include the actual message to be logged.

### Runtime Behavior

During runtime, Log4j checks its configuration when a log statement is executed to determine if the message should be logged based on the logger's level and appender configuration.

### Output

Log messages that meet the configured criteria are sent to the defined appenders, which handle them accordingly—writing messages to files or sending them to monitoring services.

By configuring Log4j appropriately, SolrCloud administrators can control log verbosity, route logs to various destinations, and ensure critical events are properly recorded for monitoring and troubleshooting.

## Log Rotation Size Threshold

Rotation size refers to the maximum size a log file can reach before it is rotated or archived. When a log file reaches this size threshold, it is typically renamed, and a new log file is created to continue logging events.

In `log4j2.xml`, if the default log rotation size threshold of 32MB is too small for production servers, it should be increased to a larger value (e.g., 100MB or more). 

```xml
<SizeBasedTriggeringPolicy size="100 MB"/>
```

