# Logging

The Logging page in the Solr admin UI displays recent messages logged by the Solr node.

## Overview of Log4j in SolrCloud

In Apache SolrCloud, Log4j is the commonly used logging framework. Here's a brief overview of how it operates:

### Configuration

Log4j in SolrCloud is configured via properties files (e.g., log4j.properties) or XML configuration files (log4j.xml). These files specify where log messages should be output, their format, and the conditions under which they should be logged.

### Loggers

Loggers are named entities that _categorize log messages_, associating each with a specific part of the Solr codebase. Solr employs various loggers for different components and functionalities.

### Appenders

Appenders define the _destination of log messages_. They can send logs to consoles, files, databases, or remote services. Solr typically configures appenders to output logs to files within the logs directory of the Solr installation.

### Levels

Log levels determine the severity of a log message. Log4j defines several standard levels like DEBUG, INFO, WARN, ERROR, and FATAL. Each logger can be set to log messages at a specific level or above.

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

### Log Statements

Developers insert log statements throughout the codebase to record events or provide diagnostic information. These statements specify the log level and include the actual message to be logged.

### Runtime Behavior

During runtime, Log4j checks its configuration when a log statement is executed to determine if the message should be logged based on the logger's level and appender configuration.

### Output

Log messages that meet the configured criteria are sent to the defined appenders, which handle them accordingly—writing messages to files or sending them to monitoring services.

By configuring Log4j appropriately, SolrCloud administrators can control log verbosity, route logs to various destinations, and ensure critical events are properly recorded for monitoring and troubleshooting.

## Log Rotation Size Threshold

In `log4j2.xml`, if the default log rotation size threshold of 32MB is too small for production servers, it should be increased to a larger value (e.g., 100MB or more).

```xml
<SizeBasedTriggeringPolicy size="100 MB"/>
```

