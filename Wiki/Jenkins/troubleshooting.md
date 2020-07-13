# Symptoms
- Jenkins takes a long time to start
- Jenkins hangs on startup
# Diagnostic/Treatment
The following recommendation and techniques help to improve and troubleshoot performance issues on startup.

## Preconditions
Before troubleshooting any further, we recommend to go through the following recommendations that address common causes of slowness on startup.

## Apply JVM Recommendation
The speed of Jenkins startup depends on the amount of initial memory allocated. It is important that you follow the recommendation:

- Ensure that the recommended settings are applied to the process running Jenkins, see Prepare Jenkins for Support.
- Ensure that an appropriate initial heap size of memory is allocated directly on startup (i.e. -Xms). If this is not specified, the process starts with a low portion of the available physical memory (that can vary depending on OS distribution and properties as explained in Garbage Collector Ergonomics)
## Quiet Down mode on startup
Any build that were in the queue when Jenkins was stopped as well as any resumable builds (i.e. Pipeline) will be restarted on startup. This can contribute to the startup time and load, especially in environments where nodes are provisioned on demand. To avoid this:

- Set the master in Quiet Down mode on startup

Note: You may cancel the quiet down mode anytime when the master has started.

## Remove Custom Loggers
Loggers are a good troubleshooting tool but they can impact the performances of your instance if they do not target a specific component or if they are specified at a high level package like for example hudson.model or hudson.security.

- If you have created custom loggers, remove them.
## Review Hook scripts
Check whether there is any post-initialization scripts under $JENKINS_HOME/init.groovy.d/ that could impact the startup of your instance.

Note: Hook scripts time are actually captured by the startup log mentioned above.

## Data Collection
In order to troubleshoot the startup performance further:

- Activate the startup performance logging by adding the system property -Djenkins.model.Jenkins.logStartupPerformance=true on startup
- Restart Jenkins

- Collect performance data

## Considerations
Please also consider the following scenario in which the Jenkins startup can be impacted.

### Upgrades
After an upgrade, it is not unusual to experience slower startup caused by plugins upgrades and migration processes. Look at the logs to understand if Jenkins is doing something and what it is doing.

### Cleanup Large Instances
#### Cleanup Strategy
- The startup time is proportional to the size of your instance (number of items, nodes, installed plugins).

#### Obsolete plugins
- An instance with a large amount of plugins may take a lot of time to load all the components. It is a good practice to review plugin usage and remove obsolete ones.

### Storage File System
Jenkins configuration files are stored on disk. Startup performances - and also overall performances for that matter - can be severely impacted if the storage File System holding $JENKINS_HOME is not tuned properly. If the $JENKINS_HOME does not resides in a local File System, ensure that the File System mount is tuned for performances.