# Hibernate Logging

Logs are your best friend when it comes to Hibernate and its plethora of obscure error codes and situations.  Hibernate is incredible until it's not.  However, we definitely encourage you to tweak your server whenever you need deeper understanding of what's going on under the hood.

## ORM Util Logging

We have provided a handy method in our ORM Utility object to help you set the logging level of hibernate globally and redirect the output of those logs to CommandBox (Lucee only for now). This must be done once the application loads in order to seed the levels and configuration and done only once. Therefore we recommend you update your `Application.cfc` with the following in the `onApplicationStart()` method

```javascript
public boolean function onApplicationStart(){
	
	// SETUP THE LOGGING
	new cborm.models.util.ORMUtilSupport().setupHibernateLogging( "INFO" );
	
	application.cbBootstrap = new coldbox.system.Bootstrap(
		COLDBOX_CONFIG_FILE,
		COLDBOX_APP_ROOT_PATH,
		COLDBOX_APP_KEY,
		COLDBOX_APP_MAPPING
	);
	application.cbBootstrap.loadColdbox();
	return true;
}
```

The `setupHibernateLogging( level )` method accepts a logging level so you can control the output of the logs.

| Level     | Description                                                                                               |
| --------- | --------------------------------------------------------------------------------------------------------- |
| **ALL**   | All levels including custom levels.                                                                       |
| **DEBUG** | Designates fine-grained informational events that are most useful to debug an application.                |
| **INFO**  | Designates informational messages that highlight the progress of the application at coarse-grained level. |
| **WARN**  | Designates potentially harmful situations.                                                                |
| **ERROR** | Designates error events that might still allow the application to continue running.                       |
| **FATAL** | Designates very severe error events that will presumably lead the application to abort.                   |
| **OFF**   | The highest possible rank and is intended to turn off logging.                                            |
| **TRACE** | Designates finer-grained informational events than the DEBUG.                                             |

This basic setup will get you to almost 90% of all your logging needs.

### Lucee Location Logs

The log files for hibernate are located in the following locations for Lucee:

```
# Web Context Logs
{engine}/WEB-INF/lucee-web/orm.log
# Server Context Logs
{engine}/WEB-INF/lucee-server/context/logs/orm.log
```

{% hint style="info" %}
Please note that if you use the `setupHibernateLogging()` then all the log output shoudl appear in the CommandBox out logs.
{% endhint %}

## Adobe Logging

In Adobe engines you can also tweak the log4j configuration to get even more out of your logs.  Let's start with the most important question, where are they?

```
{engine}/WEB-INF/cfusion/logs/hibernatesql.log
```

Ok, now the file we need to update to fine tune our logging is:

```
{engine}/WEB-INF/cfusion/lib/log4j.properties
```

The file is self explanatory and we have reviewed the log4j levels before, so just get in there and make stuff happen.  Here is our full debugging resource we use for ContentBox CMS Development:

```properties
# Set root category priority to INFO and its only appender to CONSOLE.
log4j.rootCategory=INFO,CONSOLE
#log4j.rootCategory=INFO, CONSOLE, LOGFILE

### setup ESAPI Log
log4j.logger.org.owasp.esapi=ERROR, ESAPILOGFILE
log4j.additivity.org.owasp.esapi=false

### Setup Axis Log
log4j.logger.org.apache.axis=WARN, AXISCONSOLE
log4j.additivity.org.apache.axis=false

### Setup Axis2 Log
log4j.logger.org.apache.axis2=ERROR, AXIS2CONSOLE
log4j.additivity.org.apache.axis2=false

# Set the enterprise logger category to FATAL and its only appender to CONSOLE.
log4j.logger.org.apache.axis.enterprise=FATAL, AXISCONSOLE

### Setup JetS3t Log
log4j.logger.org.jets3t.service=ERROR, CONSOLE


###--------------- ORTUS Hibernate Log Settings ------
### Set Hibernate log
log4j.logger.org.hibernate=INFO, HIBERNATECONSOLE

### log just the SQL
log4j.logger.org.hibernate.SQL=DEBUG, HIBERNATECONSOLE
log4j.additivity.org.hibernate.SQL=true
### Also log the parameter binding to the prepared statements.
log4j.logger.org.hibernate.type=DEBUG


### log schema export/update ###
log4j.logger.org.hibernate.tool.hbm2ddl=DEBUG, HIBERNATECONSOLE

### log cache activity ###
log4j.logger.org.hibernate.cache=WARN, HIBERNATECONSOLE
#---------------------------------------------

###--------------- Jetty Log Settings ------
### Set Jetty log
log4j.logger.org.eclipse.jetty=ERROR, JETTYCONSOLE

log4j.logger.net.sf.ehcache=ERROR

# CONSOLE is set to be a ConsoleAppender using a PatternLayout.
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.Threshold=INFO
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=%d{MM/dd HH:mm:ss} [%t] %-5p %m%n

# AXISCONSOLE is set to be a ConsoleAppender for Axis using a PatternLayout.
log4j.appender.AXISCONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.AXISCONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.AXISCONSOLE.layout.ConversionPattern=%d [%t] AXIS %-5p %c - %m%n

# AXIS2CONSOLE is set to be a File appender using a PatternLayout.
log4j.appender.AXIS2CONSOLE= org.apache.log4j.FileAppender
log4j.appender.AXIS2CONSOLE.File=${coldfusion.rootDir}/logs/axis2.log
log4j.appender.AXIS2CONSOLE.Append=true
log4j.appender.AXIS2CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.AXIS2CONSOLE.layout.ConversionPattern=%d [%t] AXIS2 %-5p %c - %m%n

# LOGFILE is set to be a File appender using a PatternLayout.
log4j.appender.LOGFILE=org.apache.log4j.FileAppender
log4j.appender.LOGFILE.File=axis.log
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.Threshold=INFO
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%-4r [%t] %-5p %c %x - %m%n

# ESAPICONSOLE is set to be a ConsoleAppender for ESAPI using a PatternLayout.
log4j.appender.ESAPILOGFILE= org.apache.log4j.FileAppender
log4j.appender.ESAPILOGFILE.File=${coldfusion.rootDir}/logs/esapiconfig.log
log4j.appender.ESAPILOGFILE.Append=true
log4j.appender.ESAPILOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.ESAPILOGFILE.layout.ConversionPattern=%d{MM/dd HH:mm:ss} [%t] ESAPI %-5p - %m%n%n

# HibernateConsole is set to be a RollingFileAppender for Hibernate message using a PatternLayout.
log4j.appender.HIBERNATECONSOLE= org.apache.log4j.RollingFileAppender
log4j.appender.HIBERNATECONSOLE.File=${coldfusion.rootDir}/logs/hibernatesql.log
log4j.appender.HIBERNATECONSOLE.Append=true
log4j.appender.HIBERNATECONSOLE.MaxFileSize=5000KB
log4j.appender.HIBERNATECONSOLE.MaxBackupIndex=3
log4j.appender.HIBERNATECONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.HIBERNATECONSOLE.layout.ConversionPattern=%d{MM/dd HH:mm:ss} [%t] HIBERNATE %-5p - %m%n%n

# JETTYCONSOLE is set to be a ColsoleAppender for jetty message  using a PatternLayout.
log4j.appender.JETTYCONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.JETTYCONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.JETTYCONSOLE.layout.ConversionPattern=%d{MM/dd HH:mm:ss} [%t] JETTY %-5p - %m%n%n

# For Quartz logging
log4j.logger.org.quartz=ERROR
log4j.logger.org.apache=ERROR
```
