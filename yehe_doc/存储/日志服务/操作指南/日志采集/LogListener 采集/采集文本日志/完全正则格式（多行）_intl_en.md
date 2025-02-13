## Overview
The multi-line - full regular expression mode is a log parsing mode where multiple key-value pairs can be extracted from a complete piece of log data that spans multiple lines in a log text file (such as Java program logs) based on a regular expression. If you don't need to extract key-value pairs, please configure it as instructed in [Collecting Logs with Full Text in Multi Lines](https://intl.cloud.tencent.com/document/product/614/32284).
When configuring the multi-line - full regular expression mode, you need to enter a sample log first and then customize your regular expression. After the configuration is completed, the system will extract the corresponding key-value pairs according to the capture group in the regular expression.

This document describes how to collect logs in multi-line - full regular expression mode.

>! To collect logs in multi-line - full regular expression mode, you need to upgrade to LogListener 2.4.5 as instructed in [LogListener Installation Guide](https://intl.cloud.tencent.com/document/product/614/17414).

## Prerequisites

Suppose your raw log data is:
```log
[2018-10-01T10:30:01,000] [INFO] java.lang.Exception: exception happened
    at TestPrintStackTrace.f(TestPrintStackTrace.java:3)
    at TestPrintStackTrace.g(TestPrintStackTrace.java:7)
    at TestPrintStackTrace.main(TestPrintStackTrace.java:16)
```

The first-line regular expression is:
```
\[\d+-\d+-\w+:\d+:\d+,\d+]\s\[\w+]\s.*
```

The configured custom regular expression is:
```
\[(\d+-\d+-\w+:\d+:\d+,\d+)\]\s\[(\w+)\]\s(.*)
```

After the system extracts the corresponding key-value pairs according to the `()` capture group, you can customize the key name of each group as shown below:
```
time: 2018-10-01T10:30:01,000`
level: INFO`
msg: java.lang.Exception: exception happened
    at TestPrintStackTrace.f(TestPrintStackTrace.java:3)
    at TestPrintStackTrace.g(TestPrintStackTrace.java:7)
    at TestPrintStackTrace.main(TestPrintStackTrace.java:16)
```

## Directions

### Logging in to console

1. Log in to the [CLS console](https://console.cloud.tencent.com/cls).
2. On the left sidebar, click **Logset** to enter the logset management page.

### Creating log topic

1. Select the logset for which to create a log topic and click the logset ID/name to enter the logset information page.
2. Click **Create Log Topic**.
3. In the pop-up dialog box, enter "test-multi" as the **Log Topic Name** and click **OK** as shown below:

### Managing machine group

1. After the log topic is created successfully, enter the log topic management page.
2. Select the **Collection Configuration** tab, and click the format in which you need to collect logs.
3. On the **Machine Group Management** page, select the server group to which to bind the current log topic and click **Next** to proceed to collection configuration.
For more information, please see [Machine Group Management](https://intl.cloud.tencent.com/document/product/614/17412?from_cn_redirect=1&lang=en&pg=#creating-a-server-group).

### Configuring collection

#### Configuring log file collection path

On the **Collection Configuration** page, enter the **Collection Path** according to the log collection path format as shown below:
Log collection path format: `[directory prefix expression]/**/[filename expression]`.
After the log collection path is entered, LogListener will match all common prefix paths that meet the **[directory prefix expression]** rule and listen on all log files in the directories (including subdirectories) that meet the **[filename expression]** rule. The parameters are as detailed below:

| Field     | Description                                                         |
| -------- | ------------------------------------------------------------ |
| Directory prefix | Directory structure of the log file prefix. Only wildcards `*` and `?` are supported<ul style="margin: 0;"><li>\* indicates to match any number of characters</li><li>? indicates to match any single character</li></ul> |
| /\*\*/     | It represents the current directory and all subdirectories                                   |
| Filename   | Log file name. Only wildcards `*`  and `?` are supported<ul style="margin: 0;"><li>\* indicates to match any number of  characters</li><li>? indicates to match any single character</li></ul> |

Common configuration modes are as follows:
- [Common directory prefix]/\*\*/[common filename prefix]\*
- [Common directory prefix]/\**/*[common filename suffix]
- [Common directory prefix]/\*\*/[common filename prefix]\*[common filename suffix]
- [Common directory prefix]/\*\*/\*[Common string]\*

Below is an example:

| No. | Directory Prefix Expression | Filename Expression | Description                                                         |
| ---- | -------------- | ------------ | ------------------------------------------------------------ |
| 1.   | /var/log/nginx | access.log   | In this example, the log path is configured as `/var/log/nginx/**/access.log`. LogListener will listen on log files named `access.log` in all subdirectories in the `/var/log/nginx` prefix path |
| 2.   | /var/log/nginx | *.log        | In this example, the log path is configured as `/var/log/nginx/**/*.log`. LogListener will listen on log files suffixed with `.log` in all subdirectories in the `/var/log/nginx` prefix path |
| 3.   | /var/log/nginx | error*       | In this example, the log path is configured as `/var/log/nginx/**/error*`. LogListener will listen on log files prefixed with `error` in all subdirectories in the `/var/log/nginx` prefix path |

> !
> - Only LogListener 2.3.9 or above allows adding multiple collection paths.
> - By default, a log file can only be collected by one log topic. If you want to have multiple collection configurations for the same file, please add a soft link to the source file and add it to another collection configuration.


#### Configuring multi-line - full regular expression mode

1. On the **Collection Configuration** page, set **Extraction Mode** to **Multi-line - Full regular expression** and enter a sample log in the **Log Sample** text box as shown below:
2. Define a regular expression according to the following rules.
You can choose **Auto-Generate** or **Enter Manually** to define a first-line regular expression in order to determine the boundary for multi-line logs. After the expression is verified successfully, the system will determine the number of logs that match the first-line regular expression.
 - Auto-Generate: click **Auto-Generate**, and the system will automatically generate the first-line regular expression in the grayed-out text box as shown below:
 - Enter Manually: in the text box, enter the first-line regular expression, and click **Verify**. The system will determine whether the expression has passed as shown below:
3. Extract the regular expression.
The system offers two ways to define a regular expression: **manual mode** and **auto mode**. You can manually enter the expression to extract key-value pairs for verification or click **Auto-Generate Regular Expression** to switch to auto mode. The system will extract key-value pairs to verify the regular expression according to the mode you selected and the regular expression you defined.
 - **Manual Mode**:
 1. Enter the regular expression in the **Regular Expression** text box.
    2. Click **Verify**, and the system will determine whether the sample log matches the regular expression.
 - **Auto Mode** (click **Auto-Generate Regular Expression** to switch):
    1. <span id="stap_a"></span>In the **Auto-Generate Regular Expression** pop-up view, select the log content from which to extract key-value pairs based on your actual search and analysis needs, enter the key name in the pop-up text box, and click **Confirm** as shown below:
        The system will automatically extract a regular expression from the content, and the **Automatic Extraction Result** will appear in the key-value table as shown below:
	2. Repeat [step a](#stap_a) until all key-value pairs are extracted as shown below:
    3. Click **OK**, and the system will automatically generate a complete regular expression according to the extracted key-value pairs as shown below:
    
 >? No matter whether in auto mode or manual mode, the extraction result will be displayed in the **Extraction Result** after the regular mode is defined and verified successfully. You only need to define the key name of each key-value pair for use in log search and analysis. 
>

#### Verifying manually

1. If your log data is complex, you can set **Manual Verification** to <img src="https://main.qcloudimg.com/raw/d7ba8412e263386b627369741b457f2e.png" /> to enable manual verification.
2. Enter multiple sample logs and click **Verify**. The system will verify the pass rate of the regular expression for the logs.


#### Configuring collection time

- Log time is measured in milliseconds
- The time attribute of a log is defined as follows:
 - Collection time: it is the default time attribute of a log.
 - Original timestamp: set **Use Collection Time** to <img src="https://main.qcloudimg.com/raw/b4558d9e42043de2669e1b071103836b.png" /> and enter the time key of the original timestamp and the corresponding time parsing format.
 For more information on the time parsing format, please see [Configuring the Time Format](https://intl.cloud.tencent.com/document/product/614/32942).
- Collection time: the time attribute of a log is determined by the time when CLS collects it.
- Original timestamp: the time attribute of a log is determined by the timestamp in the raw log.

Below are examples of how to enter a time parsing format:
- Example 1:
If the original timestamp of the sample log is `10/Dec/2017:08:00:00.000`, then the parsing format is `%d/%b/%Y:%H:%M:%S.%f`.
- Example 2:
If the original timestamp of the sample log is `2017-12-10 08:00:00.000`, then the parsing format is `%Y-%m-%d %H:%M:%S.%f`.
- Example 3:
If the original timestamp of the sample log is `12/10/2017, 08:00:00.000`, then the parsing format is `%m/%d/%Y, %H:%M:%S.%f`.

>! You can set "millisecond" as the log time unit. If you enter a log time with incorrect format, the collection time will be used as the log time.
>

### Setting filters

Filters are designed to help you extract valuable log data by adding log collection filter rules based on your business needs. If the filter rule is a Perl regular expression, the created filter rule will be a hit rule; in other words, only logs that match the regular expression will be collected and reported.

To collect logs in full regular expression mode, you need to configure a filter rule according to the defined custom key-value pair. For example, if you want to collect all log data with a `status` field whose value is 400 or 500 after the sample log is parsed in full regular expression mode, you need to configure `key` as `status` and the filter rule as `400|500`.

>! The relationship between multiple filter rules is logic "AND". If multiple filter rules are configured for the same key name, previous rules will be overwritten.
>

### Configuring index

1. Click **Next** to enter the **Index Configuration** page.
2. On the **Index Configuration** page, set the following information.
 - Index Status: select whether to enable it.
 - Full-Text Index: select whether to set it to case-sensitive.
 - Full-Text Delimiters: they are "@&()='",;:<>[]{}/ \n\t\r" by default and can be modified as needed.
 - Key-Value Index: it is enabled by default. You can configure the field type, delimiters, and whether to enable statistical analysis according to the key name as needed. If you don't need to enable key-value index, you can set <img src="https://main.qcloudimg.com/raw/d7ba8412e263386b627369741b457f2e.png" /> to <img src="https://main.qcloudimg.com/raw/bd22396a4acfbf6d96def87060207a46.png" />.
5. Click **Submit** as shown below:


## Relevant Operations

### Searching for logs

1. Log in to the [CLS console](https://console.cloud.tencent.com/cls).
2. On the left sidebar, click **Search and Analysis** to enter the search and analysis page.
3. Select the region, logset, and log topic as needed and click **Search and Analysis** to search for logs according to the set query criteria.
>! Index configuration must be enabled first before you can perform searches.
>

