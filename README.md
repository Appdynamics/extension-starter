# Extension Starter
## Usage
This is a template to get started with extensions. The `doRun` method in the `ExtStarterMonitor` class is the entry method to get started with your custom extension. This class does the initialization and triggers the execution of the task `ExtStarterMonitorTask` which contains the business logic. Use the `run` method to carry out your main tasks, the tasks are executed in a thread pool. The task class fetches the data through either an HTTP Call / JMX / JDBC connection and then reports metrics to the metric browser.  
 
## Prerequisites
1. Before the extension is installed, the prerequisites mentioned [here](https://community.appdynamics.com/t5/Knowledge-Base/Extensions-Prerequisites-Guide/ta-p/35213) need to be met. Please do not proceed with the extension installation if the specified prerequisites are not met.

2. Download and install [Apache Maven](https://maven.apache.org/) which is configured with `Java 8` to build the extension artifact from source. You can check the java version used in maven using command `mvn -v` or `mvn --version`. If your maven is using some other java version then please download java 8 for your platform and set JAVA_HOME parameter before starting maven.

3. The extension needs to be able to connect to the Product that you want to monitor in order to collect and send metrics. To do this, you will have to either establish a remote connection in between the extension and the product, or have an agent on the same machine running the product in order for the extension to collect and send the metrics.

## Installation
1. Clone this repository to your local repository using `git clone <repo-url>` command.
2. Make the necessary changes to monitor the product for which you are building the extension.
3. Once you have developed your extension, run `mvn clean install` and find the `<YourExtension>.zip` file in the `target` folder.
4. Unzip and copy that directory to `<MACHINE_AGENT_HOME>/monitors`

Please place the extension in the "__monitors__" directory of your __Machine Agent__ installation directory. Do not place the extension in the "__extensions__" directory of your __Machine Agent__ installation directory.

## Configuration
### config.yml
Configure the extension by editing the `config.yml` file in `<MACHINE_AGENT_HOME>/monitors/<YourExtension>/`
1. Configure the "tier" under which the metrics need to be reported. This can be done by changing the value of `<TIER ID>` in

     `metricPrefix: "Server|Component:<TIER ID>|Custom Metrics|<YourExtensionName>"`

More details around metric prefix can be found [here](https://community.appdynamics.com/t5/Knowledge-Base/How-do-I-troubleshoot-missing-custom-metrics-or-extensions/ta-p/28695).

2. Configure the instances you want to monitor with all required fields.<br/>For example,
 
     ```
     servers:
      - uri: https://www.appdynamics.com/
        displayName: AppDynamics
        username: ""
        password: ""

      - uri: https://www.yahoo.com/
        displayName: Yahoo
        username: ""
        password: ""
        passwordEncrypted: ""
    ```
 3. Configure the encyptionKey for encryptionPasswords(only if password encryption required).<br/>For example,
    ```
    encryptionKey: welcome
    ```
 4. Configure the `numberOfThreads` depending on the number of concurrent tasks. For example, if you are monitoring three instances, and each task for each server runs as a single thread then use `numberOfThreads: 3`.
 5. Configure the metrics section. You can look at [Redis Monitoring Extension](https://github.com/Appdynamics/redis-monitoring-extension) `config.yml` for reference.
 6. Configure the path to the `config.yml` file by editing the `<task-arguments>` in the `monitor.xml` file in the `<MACHINE_AGENT_HOME>/monitors/SolrMonitor/<YourExtension>/` directory. Below is the sample,
    ```
    <task-arguments>
            <argument name="config-file" is-required="true" default-value="monitors/<YourExtension>/config.yml" />
    </task-arguments>
    ```
 
Please copy all the contents of the config.yml file and go to http://www.yamllint.com/ . On reaching the website, paste the contents and press the “Go” button on the bottom left.

## Metrics
By default, the metrics will be reported under the following metric tree:

`Application Infrastructure Performance|Custom Metrics|<YourExtension>|SERVERNAME`

This will register metrics to all tiers within the application. We strongly recommend using the tier specific metric prefix so that metrics are reported only to a specified tier. Please change the metric prefix in your `config.yaml`

`metricPrefix: "Server|Component:<TierID>|Custom Metrics|<YourExtension>|"`

For instructions on how to find the tier ID, please refer to the `Metric Path` subsection [here](https://community.appdynamics.com/t5/Knowledge-Base/How-do-I-troubleshoot-missing-custom-metrics-or-extensions/ta-p/28695).

Metrics will now be seen under the following metric tree:

`Application Infrastructure Performance|$TIER|Custom Metrics|<YourExtension>|SERVERNAME|`

## Credentials Encryption
Please visit [this page](https://community.appdynamics.com/t5/Knowledge-Base/How-to-use-Password-Encryption-with-Extensions/ta-p/29397) to get detailed instructions on password encryption. The steps in this document will guide you through the whole process.

## Extensions Workbench
Workbench is an inbuilt feature provided with each extension in order to assist you to fine tune the extension setup before you actually deploy it on the controller. Please review the following document on [How to use the Extensions WorkBench](https://community.appdynamics.com/t5/Knowledge-Base/How-to-use-the-Extensions-WorkBench/ta-p/30130)

## Troubleshooting
Please follow the steps listed in this [troubleshooting-document](https://community.appdynamics.com/t5/Knowledge-Base/How-to-troubleshoot-missing-custom-metrics-or-extensions-metrics/ta-p/28695) in order to troubleshoot your issue. These are a set of common issues that customers might have faced during the installation of the extension.


## Contributing
Always feel free to fork and contribute any changes directly via [GitHub](https://github.com/Appdynamics/extension-starter).

## Version
Name |	Version
---|---
Extension Version |	2.2.4
Last Update |	12/10/2021
ChangeList | [ChangeLog](https://github.com/Appdynamics/extension-starter/blob/master/CHANGELOG.md)
