
# Radware Cloud Services
<~XSIAM>  
This pack includes Cortex XSIAM content.  
It supports the following Radware Cloud Services event types: 
- [Radware Cloud WAAP Service Access Logs](https://support.radware.com/app/answers/answer_view/a_id/1018351/~/cloud-waf-service-access-log-integration-guide).
  - Access logs provide detailed data regarding client access to applications protected by Cloud WAAP. 
- [Radware AppWall Cloud WAF Security Event Logs](https://support.radware.com/app/answers/answer_view/a_id/1018635/~/reviewing-security-events).
  - A security event is generated whenever Cloud WAF detects an attack, when an ongoing attack
is still active, or when an ongoing attack status has changed. The generated security event
includes the information relevant to the specific attack or security breach.

See the documentation below for the configuration required for each event type to collect into Cortex XSIAM.  


## Collect Radware Cloud WAAP Service Access Logs

### AWS - S3 bucket

**On AWS**

Sign in to your AWS account and create a dedicated Amazon S3 bucket, which collects the generic logs that you want to capture.

See this [doc](https://docs-cortex.paloaltonetworks.com/r/Cortex-XSIAM/Cortex-XSIAM-Administrator-Guide/Ingest-Generic-Logs-from-Amazon-S3) for further instructions on how to create a S3 bucket.

**On Radware Cloud Services**

1. Contact the Radware Support team in order to enable Access logs.
2. Navigate to **Account Settings** and fill the below attributes:
   - Bucket Name
   - Region
   - Access Key
   - Secret Key
   - Prefix (optional)
3. Click the **Advanced** tab and enable **Access log**.
4. Select the application to which to export all Access logs.

For additional information, refer to the official Radware [documentation](https://support.radware.com/).

**On XSIAM:**

1. Navigate to **Settings** -> **Data Sources** -> **Add Data Source**.
2. Click **Amazon S3**.
3. Click **Connect** or **Connect Another Instance**.
4. Set the following values:
   - SQS URL - Refer to Configure an Amazon Simple Queue Service (SQS) [here](https://docs-cortex.paloaltonetworks.com/r/Cortex-XSIAM/Cortex-XSIAM-Administrator-Guide/Ingest-Generic-Logs-from-Amazon-S3). 
   - Name as `Radware Access Log`
   - AWS Client ID
   - AWS Client Secret
   - Log Type as `Generic`
   - Log Format as `JSON`
   - Vendor as `radware`
   - Product as `access_logs`
   - Compression as `gzip`
5. Select the Security Events checkbox.

For additional information, see this [doc](https://docs-cortex.paloaltonetworks.com/r/Cortex-XSIAM/Cortex-XSIAM-Administrator-Guide/Ingest-Generic-Logs-from-Amazon-S3).

### Obtain Amazon SQS Credentials 
**On Radware Cloud WAF Portal**
1. Connect to your Radware account on the Radware [Cloud WAF Portal](https://portal.cwp.radwarecloud.com/#/login).
2. Navigate to **Account** &rarr; **Account Settings**. 
3. Click **Download SIEM Configuration** to download a configuration file which includes the details of the SQS event queues and your credentials for accessing them. This file has the name convention of *siemConfigFetchConfig_\<ID\>.txt*. Use this file in the next section when configuring Logstash (or any other log-collection solution you want to use).

### Configure Cortex XSIAM Broker VM Syslog Server  
You will need to use the information described [here](https://docs-cortex.paloaltonetworks.com/r/Cortex-XDR/Cortex-XDR-Pro-Administrator-Guide/Configure-the-Broker-VM).

You can configure the specific vendor and product for this instance.

1. Navigate to **Settings** &rarr; **Configuration** &rarr; **Data Broker** &rarr; **Broker VMs**. 
2. Go to the **Apps** column under the **Brokers** tab and add the **Syslog** app for the relevant broker instance. If the **Syslog** app already exists, hover over it and click **Configure**.
3. Click **Add New**.
3. When configuring the Syslog Collector, set the following parameters:
   | Parameter     | Value    
   | :---          | :---                    
   | `Protocol`    | Select **UDP** for the default forwarding.
   | `Port`        | Enter the syslog service port that Cortex XSIAM Broker VM should listen on for receiving Radware AppWall Cloud WAF security events that are forwarded from Logstash. 
   | `Vendor`      | Enter **Radware**. 
   | `Product`     | Enter **AppWall**. 

For additional information regarding SIEM integration with Radware AppWall Cloud WAF events, refer to the [official Radware documentation](https://support.radware.com/).

</~XSIAM>