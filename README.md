# IBM Integration Bus (IIB) v10 Extension

You must use this README along with the documentation for [IBM WebSphere MQ](https://docops.ca.com/ca-apm/10-7/en/implementing-agents/ibm-websphere-mq) on https://docops.ca.com/apm.

# Description

This extension helps monitor IBM Integration Bus v10 \(previously known as WebSphere Message Broker\). Both Queue Manager and IIB can be monitored together.

## What's New?

* Monitoring all the message flows irrespective of where they are deployed. For example: message flow deployed under execution group/application/message flow
* Change in metric names as per new IBM naming conventions
* Broker -&gt; Integration Node
* Execution Group -&gt; Integration Server

Extension binaries contain:

* `IIB10Fieldpack.zip` - All files required to monitor IIB10
* `EMJars.zip` - EM management module jars, typeviews, script files


## Supported third party versions
* This extension supports all the operating systems that MQ powerpack used to support.
* This extension supports both http and https communication with the broker.

## Limitations
* This extension is not backward compatible with IIB9 and older versions.

## License
[Eclipse Public License - v 1.0](LICENSE)

# Installation Instructions

## Prerequisites

1. Download the latest release from https://github.com/CA-APM/IIB10-Extension/releases.
2. Extract `IIB10Fieldpack.zip`.
3. Open startMQMonitor.bat file in the `IIB10Fieldpack` directory and set the `JAVA_HOME`
4. Copy all the required MQ/MB/third party dependent jars from the MQ/MB product installation lib to the `IIB10Fieldpack/lib` directory.

 - IntegrationAPI.jar \(Replacement for ConfigManagerProxy.jar\)
 - com.ibm.mq.jar
 - connector.jar
 - com.ibm.mqjms.jar
 - com.ibm.mq.jmqi.jar
 - dhbcore.jar
 - com.ibm.mq.headers.jar
 - com.ibm.mq.commonservices.jar
 - com.ibm.mq.pcf.jar/pcf6.1.jar
 - ibmjsseprovider2.jar
 - j2ee.jar

5. Also copy the following jetty jars available in `IBM/IIB/10.0.0.1/common/jetty/lib` directory:

 - jetty-io.jar
 - jetty-util.jar
 - websocket-api.jar
 - websocket-client.jar
 - websocket-common.jar

6. Extract `EMJars.zip` and copy/replace the contents of EMJars into `EM_HOME` root directory.

## Configuration

1. Complete all the configuration steps documented in [IBM WebSphere MQ](https://docops.ca.com/ca-apm/10-7/en/implementing-agents/ibm-websphere-mq) for IIB10 extension such as permissions on QM, User and Channels.
2. For Queue Manager monitoring follow the steps in [IBM WebSphere MQ](https://docops.ca.com/ca-apm/10-7/en/implementing-agents/ibm-websphere-mq).
3. For IIB monitoring, open `MBMonitor\_10.properties` file and configure the IIB10 details as below.  All properties in this file work in the same way as the existing `MQ Powerpack MBMonitor\_7.properties` file.
4. For each broker instance listed in the property `mq.broker.monitor.list`, add below additional property along with other existing QM properties.

For example,
```
mq.broker.monitor.list=broker1,broker2
broker1.broker.configfile=broker1.broker
broker2.broker.configfile=broker2.broker
```

Here, `broker1.broker` and `broker2.broker` are the IIB configuration filenames. For each Integration Node instance, one separate file needs to be created and placed in the `properties` directory. The sample `iib10.broker` config file is available in properties directory.

The sample broker config file content:
```
<?xml version="1.0" encoding="utf-8"?>
<IntegrationNodeConnectionParameters Version="10.0.0" host="hostname" listenerPort="4414" useSsl="true" sslTrustStorePassword="password" sslTrustStorePath="c:\keystore\broker.jks" />`
```

The file name extension must be `\*.broker`.

All the details provided in this file are specific to Integration Node.

 * **host** - host for the Integration Node
 * **listenerPort** -  web administration port for the Integration Node


5. If Admin Security is enabled, then to set the username and password using below system environment properties

```
MQSI\_CMP\_USERNAME=username
MQSI\_CMP\_PASSWORD=password
```

6. Make sure to enable Message flow stats and Resource stats for all the flows and execution groups.

7. If you want to get IntegrationAPI tracing log for debugging purpose, enable it by adding property `integrationapi.tracing.log=enable` to the `MBMonitor\_10.properties`. The tracing log file will be created with name `IntegrationAPITracing.log`.

8. After installation and configuration, start MQMonitor and EM.

9. If everything is properly configured metrics will be reported under IBM Integration Node. Also, IBM Integration Node management module will be loaded.

## Support
This document and associated tools are made available from CA Technologies, A Broadcom Company as examples and provided at no charge as a courtesy to the CA APM Community at large. This resource may require modification for use in your environment. However, please note that this resource is not supported by CA Technologies, and inclusion in this site should not be construed to be an endorsement or recommendation by CA Technologies. These utilities are not covered by the CA Technologies software license agreement and there is no explicit or implied warranty from CA Technologies. They can be used and distributed freely amongst the CA APM Community, but not sold. As such, they are unsupported software, provided as is without warranty of any kind, express or implied, including but not limited to warranties of merchantability and fitness for a particular purpose. CA Technologies does not warrant that this resource will meet your requirements or that the operation of the resource will be uninterrupted or error free or that any defects will be corrected. The use of this resource implies that you understand and agree to the terms listed herein.

Although these utilities are unsupported, please let us know if you have any problems or questions by adding a comment to the CA APM Community Site area where the resource is located, so that the author(s) may attempt to address the issue or question.


### Support URL
https://github.com/CA-APM/IIB10-Extension/issues

### Product Compatibility Matrix
http://pcm.ca.com/

## Categories
Middleware/ESB
