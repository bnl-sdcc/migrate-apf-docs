<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en_US" lang="en_US">

<body>

<p />
<h1><a name="AutoPyFactory_Reference_Manual"></a> AutoPyFactory: Reference Manual </h1>
<li> <a href="#1_About_this_Document">1  About this Document</a>
</li> <li> <a href="#2_Applicable_versions">2  Applicable versions</a>
</li> <li> <a href="#3_Format_of_the_configuration_fi">3  Format of the configuration files</a>
</li> <li> <a href="#4_sysconfig_autopyfactory">4  sysconfig/autopyfactory</a>
</li> <li> <a href="#5_autopyfactory_conf">5  autopyfactory.conf</a>
</li> <li> <a href="#6_queues_conf">6  queues.conf</a> <ul>
<li> <a href="#6_1_generic_variables">6.1  generic variables</a>
</li> <li> <a href="#6_2_WMS_Status_Plugin_variables">6.2 WMS Status Plugin variables</a>
</li> <li> <a href="#6_3_Batch_Status_Plugin_variable">6.3  Batch Status Plugin variables</a>
</li> <li> <a href="#6_4_Sched_Plugin_variables">6.4  Sched Plugin variables</a> <ul>
<li> <a href="#6_4_1_List_of_plugins">6.4.1  List of plugins</a>
</li> <li> <a href="#6_4_2_Plugins_parameters">6.4.2  Plugins parameters</a>
</li></ul> 
</li> <li> <a href="#6_5_Batch_Submit_Plugin_variable">6.5  Batch Submit Plugin variables</a>
</li></ul> 
</li> <li> <a href="#7_proxy_conf">7  proxy.conf</a>
</li> <li> <a href="#8_monitor_conf">8  monitor.conf</a>
</li></ul> 
<p />
<h1><a name="1_About_this_Document"></a> 1  About this Document </h1>
<p />
This document describes all configuration variables in <a href="AutoPyFactory.html" class="twikiLink">AutoPyFactory</a>
<p />
<p />
Conventions used in this document:
<p />
<p />
<font color="#808080">A <i>User Command Line</i> is illustrated by a green box that displays a prompt:</font>
<p />
<pre class="screen">
  [user@client ~]$
</pre>
<p />
<font color="#808080">A <i>Root Command Line</i> is illustrated by a red box that displays the <em>root</em> prompt:</font>
<p />
<pre class="rootscreen">
  [root@factory ~]$
</pre>
<p />
<font color="#808080"><i>Lines in a file</i> are illustrated by a yellow box that displays the desired lines in a file:</font>
<pre class="file">
priorities=1
</pre>
<p />
<p />
<p />
<h1><a name="2_Applicable_versions"></a> 2  Applicable versions </h1>
<p />
This documentation applies to the latest version of APF: 2.4.9
<p />
<p />
<h1><a name="3_Format_of_the_configuration_fi"></a> 3  Format of the configuration files </h1>
<p />
An explanation of the format of the configuration files in AutoPyFactory can be seen <a href="AutoPyFactoryConfiguration.html#3_Format_of_the_configuration_fi" target="_top">here</a>
<p />
<h1><a name="4_sysconfig_autopyfactory"></a> 4  sysconfig/autopyfactory </h1>
<p />
The first set of configuration for the factory is done via the <code>/etc/sysconfig/autopyfactory</code> config file. 
This is the configuration file used by the daemon service to decide how to run the factory. 
It set the following variables: <ul>
<li> log level: <strong>WARNING</strong>, <strong>INFO</strong> or <strong>DEBUG</strong>
</li> <li> time to sleep between internal cycles
</li> <li> the username to switch into to. Note the factory will not run as root, but as a non priviledged account
</li> <li> the log file
</li></ul> 
Also, any other modifications to the environment needed by the factory can be set in this file.
Example
<p />
<pre class="file">
OPTIONS="--debug --sleep=60 --runas=autopyfactory --log=/var/log/autopyfactory/autopyfactory.log"
CONSOLE_LOG=/var/log/autopyfactory/console.log
env | sort >> $CONSOLE_LOG
</pre>
<p />
<h1><a name="5_autopyfactory_conf"></a> 5  autopyfactory.conf </h1>
<!--
<strong>FIXME: check which variables are really mandatory and which ones are optional</strong>
<p />
<strong>FIXME: fix the format on the multiline cells</strong>
<p />
<strong>FIXME: fix the fake wiki words</strong>
-->
<p />
<table class="tg">
  <tr>
    <th class="tg-header">variable</th>
    <th class="tg-header">description</th>
    <th class="tg-header">comments</th>
  </tr>
<p />
<tr>
    <td class="tg-raw31">abort_no_queues</td>
    <td class="tg-raw32"> decides if the factory should shutdown when there are no active queues. <br> Default = False</td>
    <td class="tg-raw33"> new in 2.5</td>
  </tr>
<tr>
    <td class="tg-raw31">authmanager.enabled</td>
    <td class="tg-raw32">to determine if automatic credentials management is used or not.  Accepted values are True&verbar;False </td>
    <td class="tg-raw33"></td>
  </tr>
<tr>
    <td class="tg-raw31">authmanager.sleep</td>
    <td class="tg-raw32"> Sleep interval for authmanager thread.  </td>
    <td class="tg-raw33"></td>
  </tr>
<tr>
    <td class="tg-raw31">baseLogDir</td>
    <td class="tg-raw32"> where outputs from pilots are stored NOTE: No trailing '/'!!! </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">baseLogDirUrl</td>
    <td class="tg-raw32">where outputs from pilots are available via http.  NOTE: It must include the port.  NOTE: No trailing '/'!!! </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchstatus.condor.sleep</td>
    <td class="tg-raw32">time the Condor BatchStatus Plugin waits between cycles Value is in seconds. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchstatus.maxtime</td>
    <td class="tg-raw32">maximum time while the info is considered reasonable.  If info stored is older than that, is considered not valid, and some NULL output will be returned. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">cycles</td>
    <td class="tg-raw32">maximum number of times the queues will loop.  None means forever. 0 means the queue threads are not started.</td>
    <td class="tg-raw33">0 meaning the threads are not started is new in 2.4.14 </td>
  </tr>
<tr>
    <td class="tg-raw31">cleanlogs.keepdays</td>
    <td class="tg-raw32">maximum number of days the condor logs will be kept, in case they are placed in a subdirectory for an APFQueue that is not being currently managed by AutoPyFactory.  For example, an apfqueue that has been created and used for a short amount of time, and it does not exist anymore.  Still the created logs have to be cleaned at some point... </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">enablequeues</td>
    <td class="tg-raw32">default value to enable/disable all queues at once.  When True, its value will be overriden by the queue config variable 'enabled', queue by queue.  When False, all queues will stop working, but the factory will still be alive performing basic actions (eg. printing logs). </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">factoryId</td>
    <td class="tg-raw32">Name that the factory instance will have in the APF web monitor.  Make factoryId something descriptive and unique for your factory, for example <site>-<host>-<admin> (e.g. BNL-gridui11-jhover) </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">factoryAdminEmail</td>
    <td class="tg-raw32">Email of the local admin to contact in case of a problem with an specific APF instance. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">factorySMTPServer</td>
    <td class="tg-raw32">Server to use to send alert emails to admin.  </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">factoryUser</td>
    <td class="tg-raw32">account under which APF will run </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">factory.sleep</td>
    <td class="tg-raw32">sleep time between cycles in mainLoop in Factory object Value is in seconds. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">mappingsConf</td>
    <td class="tg-raw32">local path to the configuration file with the mappings: for example, globus2info, jobstatus2info, etc. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">maxperfactory.maximum</td>
    <td class="tg-raw32">maximum number of condor jobs to be running at the same time per Factory.  It is a global number, used by all APFQueues submitting pilots with condor.  The value will be used by MaxPerFactorySchedPlugin plugin </td>
    <td class="tg-raw33"> </td>
  </tr>

<tr>
    <td class="tg-raw31">monitorConf</td>
    <td class="tg-raw32">local path to the configuration file for Monitor plugins. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">logserver.allowrobots</td>
    <td class="tg-raw32">if false, creates a robots.txt file in the docroot.  Valid valudes are True&verbar;False </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">logserver.enabled</td>
    <td class="tg-raw32">determines if batch logs are exported via HTTP.  Valid values are True&verbar;False </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">logserver.index</td>
    <td class="tg-raw32">determines if automatic directory indexing is allowed when log directories are browsed.  Valid values are True&verbar;False </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">proxyConf</td>
    <td class="tg-raw32">local path to the configuration file for automatic proxy management.  NOTE: must be a local path, not a URI.  </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">proxymanager.enabled</td>
    <td class="tg-raw32">to determine if automatic proxy management is used or not.  Accepted values are True&verbar;False </td>
    <td class="tg-raw33"> deprecated in 2.4.10</td>
  </tr>
<tr>
    <td class="tg-raw31">proxymanager.sleep</td>
    <td class="tg-raw32"> Sleep interval for proxymanager thread.  </td>
    <td class="tg-raw33"> deprecated in 2.4.10</td>
  </tr>
<tr>
    <td class="tg-raw31">queueConf</td>
    <td class="tg-raw32">URI plus path to the configuration file for APF queues.  NOTE: Must be expressed as a URI (<a href="file://" target="_top">file://</a> or <a href="http://" target="_top">http://</a>) Cannot be used at the same time that queueDirConf </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">queueDirConf</td>
    <td class="tg-raw32">directory with a set of configuration files, all of them to be used at the same time.  i.e.  /etc/autopyfactory/queues.d/ Cannot be used at the same time that queueConf </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">reconfig</td>
    <td class="tg-raw32"> boolean to decide if queues configuration needs to be recalculated periodically  <br> Default=True.</td>
    <td class="tg-raw33"> New in 2.4.14 </td>
  </tr>
<tr>
    <td class="tg-raw31">wmsstatus.condor.sleep</td>
    <td class="tg-raw32">time to wait between cycles when WMS Status Plugin is Condor. Value is in seconds. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">wmsstatus.maximum</td>
    <td class="tg-raw32">maximum time while the info is considered reasonable.  If info stored is older than that, is considered not valid, and some NULL output will be returned. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">wmsstatus.panda.sleep</td>
    <td class="tg-raw32">time to wait between cycles when WMS Status Plugin is Panda. Value is in seconds. </td>
    <td class="tg-raw33"> </td>
  </tr>
</table>
<p />
<p />
<h1><a name="6_queues_conf"></a> 6  queues.conf </h1>
<p />
<!--
<strong>FIXME: check which variables are really mandatory and which ones are optional</strong>
<p />
<strong>FIXME: fix the format on the multiline cells</strong>
<p />
<strong>FIXME: fix the fake wiki words</strong>
-->
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="6_1_generic_variables"></a> 6.1  generic variables </span></h2>
<p />
<table class="tg">
  <tr>
    <th class="tg-header">variable</th>
    <th class="tg-header">description</th>
    <th class="tg-header">comments</th>
  </tr>
<tr> 
    <td class="tg-raw31">apfqueue.sleep</td> 
    <td class="tg-raw32"> sleep time between cycles in APFQueue object.  Value is in seconds.    </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchqueue</td> 
    <td class="tg-raw32"> the Batch system related queue name.  E.g. the PanDA queue name (formerly called nickname) </td> 
    <td class="tg-raw33"> mostly needed for the wrapper. Not needed with APF 2.4.7+ and wrapper-0.9.16+</td> 
  </tr>
<tr> 
    <td class="tg-raw31">cleanlogs.keepdays</td> 
    <td class="tg-raw32"> maximum number of days the condor logs will be kept </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">cloud</td> 
    <td class="tg-raw32"> is the cloud this queue is in. You should set this to suppress pilot submission when the cloud goes offline N.B. Panda clouds are UPPER CASE, e.g., UK </td> 
    <td class="tg-raw33"> Deprecated </td> 
  </tr>
<tr> 
    <td class="tg-raw31">enabled</td> 
    <td class="tg-raw32"> determines if each queue section must be used by AutoPyFactory or not. Allows to disable a queue without commenting out all the values.  Valid values are True&verbar;False. </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr>  
    <td class="tg-raw31">executable</td>
    <td class="tg-raw32"> path to the script which will be executed.  As the purpose of the factory is to submit jobs to the different resources (local batch queues, grid sites, etc.) an executable, with its corresponding list of input arguments, is needed.  This executable can be anything.  <br> In principle, details on how to install the executable and the list of arguments are out of the scope of this documentation. However, in the case of ATLAS experiment, executable documentation can be found <a href="AutoPyFactoryWorkflowAtlas.html#4_Wrapper" target="_top">here</a></td> 
    <td class="tg-raw33"> </td> 
  </tr> 
<tr>  
    <td class="tg-raw31">executable.arguments</td>
    <td class="tg-raw32"> input options to be passed verbatim to the executable script. </td> 
    <td class="tg-raw33"> </td>
  </tr> 
<tr>  
    <td class="tg-raw31">monitorsection</td>
    <td class="tg-raw32"> section in monitor.conf where info about the actual monitor plugin can be found.  The value can be a single section or a split by comma list of sections.  Monitor plugins handle job info publishing to one or more web monitor/dashboards.  </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr>  
    <td class="tg-raw31">status</td>
    <td class="tg-raw32"> can be "test", "offline" or "online" </td> 
    <td class="tg-raw33"> </td>
  </tr>  
<tr>  
    <td class="tg-raw31">wmsqueue</td>
    <td class="tg-raw32"> the WMS system queue name.  E.g. the PanDA siteid name </td> 
    <td class="tg-raw33"> </td>
  </tr> 
</table>
<p />
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="6_2_WMS_Status_Plugin_variables"></a> 6.2  WMS Status Plugin variables </span></h2>
<p />
<table class="tg">
  <tr>
    <th class="tg-header">variable</th>
    <th class="tg-header">description</th>
    <th class="tg-header">comments</th>
  </tr>
<p />
<tr>
    <td class="tg-raw31">wmsstatusplugin</td>
    <td class="tg-raw32"> WMS Status Plugin. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">wmsstatus.condor.queryargs</td>
    <td class="tg-raw32"> list of command line input options to be included in the query command <strong>verbatim</strong>. E.g.  wmsstatus.condorqueryargs = -name <schedd_name> ...  </td>
    <td class="tg-raw33"> </td>
  </tr>
</table>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="6_3_Batch_Status_Plugin_variable"></a> 6.3  Batch Status Plugin variables </span></h2>
<p />
<table class="tg">
  <tr>
    <th class="tg-header">variable</th>
    <th class="tg-header">description</th>
    <th class="tg-header">comments</th>
  </tr>
<p />
<tr>
    <td class="tg-raw31">batchstatusplugin</td>
    <td class="tg-raw32"> Batch Status Plugin. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchstatus.condor.queryargs</td>
    <td class="tg-raw32"> list of command line input options to be included in the query command <strong>verbatim</strong>. E.g.  batchstatus.condor.queryargs = -name <schedd_name> -pool &lt;centralmanagerhostname[:portnumber]&gt; </td>
    <td class="tg-raw33"> </td>
  </tr>
</table>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="6_4_Sched_Plugin_variables"></a> 6.4  Sched Plugin variables </span></h2>
<p />
Specific Scheduler Plugin implementing the algorithm deciding how many new pilots to submit next cycle.  
The value can be a single Plugin or a split by comma list of Plugins.  
In the case of more than one plugin, each one will acts as a filter with respect to the value returned by the previous one.  
By selecting the right combination of Plugins in a given order, a complex algorithm can be built. <BR> 
E.g., the algorithm can start by using Ready Plugin, which will determine the number of pilots based on the number of activated jobs in the WMS queue and the number of already submitted pilots.  
After that, this number can be filtered to a maximum (MaxPerCycleSchedPlugin) or a minimum (MinPerCycleSchedPlugin) number of pilots.  
Or even can be filtered to a maximum number of pilots per factory (MaxPerFactorySchedPlugin). 
Also it can be filtered depending on the status of the wmsqueue (StatusTestSchedPlugin, StatusOfflineSchedPlugin).
<p />
Once the plugin (or list of plugins) to be used has been set, the corresponding list of specific variables need to be set as well when needed. 
Example:
<p />
<pre class="file">
schedplugin = Ready, Scale, MaxPerCycle, MaxPending
sched.scale.factor = 0.25
sched.maxpercycle.maximum = 100
sched.maxpending.maximum = 10
</pre>
<p />
<h3><a name="6_4_1_List_of_plugins"></a> 6.4.1  List of plugins </h3>
<p />
The following table lists the complete list of available sched plugins.
<p />
<table class="tg">
  <tr>
    <th class="tg-header">plugin</th>
    <th class="tg-header">description</th>
    <th class="tg-header">comments</th>
  </tr>
<tr>
    <td class="tg-raw31">Fixed</td>
    <td class="tg-raw32">Always submits a fixed number of pilots. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">KeepNRunning</td>
    <td class="tg-raw32"> Strives to keep a certain number of pilots running, regardless anything else. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">MaxPending</td>
    <td class="tg-raw32">  If there are currently pending pilots, imposses a limit on how many more to submit. If there are no currently any pending pilots, that limit is not applied.   </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">MaxPerCycle</td>
    <td class="tg-raw32"> Imposses a limit on the maximum number of pilots to be submitted each cycle.  </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">MaxToRun</td>
    <td class="tg-raw32">  Imposses a maximum limit on the total number of pilots, including both the currently ones running and pending. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">MinPending</td>
    <td class="tg-raw32"> Submits enough pilots to try keepinig a minimum number of them pending. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">MinPerCycle</td>
    <td class="tg-raw32">  Imposses a limit on the minimum number of pilots to be submitted each cycle. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">Ready</td>
    <td class="tg-raw32">Checks the number of jobs ready to be run in the WMS service, the number of previously submitted pilot still in idle state, and calculates the difference. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">Scale</td>
    <td class="tg-raw32"> Multiplies by a factor the decision made by the previous plugin in the chain.  </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">StatusOffline</td>
    <td class="tg-raw32"> Makes a decission about how many pilots to submit when the WMS queue is in internal status <code>offline</code>. <br> This plugin was introduced mostly for the case when WMS Status plugin is <code>Panda</code>, so it is not too much helpful in other cases.  </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">StatusTest</td>
    <td class="tg-raw32"> Makes a decission about how many pilots to submit when the WMS queue is in internal status <code>test</code>. <br>This plugin was introduced mostly for the case when WMS Status plugin is <code>Panda</code>, so it is not too much helpful in other cases.  </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">Throttle</td>
    <td class="tg-raw32">  Reduces the number of pilots to be submitted if it detects a significative number of previously submitted pilots have finished too fast, as that may be an indication of a broken node in the target resource (a.k.a. a black hole). </td>
    <td class="tg-raw33"> New in 2.4.7</td>
  </tr>
<tr>
    <td class="tg-raw31">WeightedActivated</td>
    <td class="tg-raw32"> Similar to Ready, but applying multiply factors to the number of ready jobs and idle pilots. </td>
    <td class="tg-raw33"> </td>
  </tr>
</table>
<p />
<h3><a name="6_4_2_Plugins_parameters"></a> 6.4.2  Plugins parameters </h3>
<p />
Some of the sched plugins accept one or more parameters.
The following table lists all valid parameters for all sched plugins.
<p />
<table class="tg">
  <tr>
    <th class="tg-header">variable</th>
    <th class="tg-header">description</th>
    <th class="tg-header">comments</th>
  </tr>
<tr>
    <td class="tg-raw31">sched.fixed.pilotspercycle</td>
    <td class="tg-raw32"> fixed number of pilots to be submitted each cycle </td>
    <td class="tg-raw33"> Mandatory </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.keepnrunning.keep_running</td>
    <td class="tg-raw32"> number of total jobs to keep running and/or pending. <br> If None, then it changes the sense of input from  new jobs (relative) to a target number (absolute)</td>
    <td class="tg-raw33" > </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.maxpending.maximum</td>
    <td class="tg-raw32"> maximum number of pilots to be pending </td>
    <td class="tg-raw33"> Mandatory</td>
  </tr>
<tr>
    <td class="tg-raw31">sched.maxpercycle.maximum</td>
    <td class="tg-raw32"> maximum number of pilots to be submitted per cycle </td>
    <td class="tg-raw33"> Mandatory</td>
  </tr>
<tr>
    <td class="tg-raw31">sched.maxtorun.maximum</td>
    <td class="tg-raw32"> maximum number of pilots allowed at a time.  </td>
    <td class="tg-raw33"> Mandatory </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.minpending.minimum</td>
    <td class="tg-raw32"> minimum number of pilots to be pending </td>
    <td class="tg-raw33"> Mandatory</td>
  </tr>
<tr>
    <td class="tg-raw31">sched.minpercycle.minimum</td>
    <td class="tg-raw32"> minimum number of pilots to be submitted per cycle </td>
    <td class="tg-raw33"> Mandatory </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.ready.offset</td>
    <td class="tg-raw32"> the minimum value in the number of ready jobs to trigger submission. </td>
    <td class="tg-raw33"> Optional </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.scale.factor</td>
    <td class="tg-raw32"> scale factor to correct the previous value of the number of pilots.  <br> Value is a float number. <br> Default = 1.0 </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.statusoffline.pilots</td>
    <td class="tg-raw32"> number of pilots to submit when the wmsqueue or the cloud is in status = offline <br> Default = 0 </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.statustest.pilots</td>
    <td class="tg-raw32"> number of pilots to submit when the wmsqueue is in status = test <br> Default = 0 </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.throttle.interval</td>
    <td class="tg-raw32"> the time windows we observe. <br> Value in seconds. <br> Default = last hour </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.throttle.maxtime</td>
    <td class="tg-raw32"> maximum WallTime for a pilot to be declared "too short". <br> Value in seconds. <br> Default = 10 </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.throttle.ratio</td>
    <td class="tg-raw32"> minimum ratio too short pilots over total pilots to decide there is a blackhole <br> Default = 0.5 </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.throttle.submit</td>
    <td class="tg-raw32"> number of pilots to submit when a blackhole is detected <br> Default = 1 </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.weightedactivated.activated</td>
    <td class="tg-raw32"> weight to be applied to the number of jobs activated <br> Value is a float <br> Default = 1.0 </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">sched.weightedactivated.pending</td>
    <td class="tg-raw32"> weight to be applied to the number of pilots pending <br> Value is a float <br> Default = 1.0 </td>
    <td class="tg-raw33"> </td>
  </tr>
</table>
<p />
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="6_5_Batch_Submit_Plugin_variable"></a> 6.5  Batch Submit Plugin variables </span></h2>
<p />
<table class="tg">
  <tr>
    <th class="tg-header">variable</th>
    <th class="tg-header">description</th>
    <th class="tg-header">comments</th>
  </tr>
<p />
<tr>
    <td class="tg-raw31">batchsubmitplugin</td>
    <td class="tg-raw32"> Batch Submit Plugin.  Currently available options are: CondorGT2, CondorGT5, CondorCREAM, CondorLocal, CondorLSF, CondorEC2, CondorDeltaCloud. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td colspan="3" class="tg-splitheader">Configuration when batchsubmitplugin is condorgt2</td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorgt2.condor_attributes</td>
    <td class="tg-raw32"> list of condor attributes, splited by comma, to be included in the condor submit file <strong>verbatim</strong> <BR>e.g. +Experiment = "ATLAS",+VO = "usatlas",+Job_Type = "cas" Can be used to include any line in the Condor-G file that is not otherwise added programmatically by AutoPyFactory.  Note the following directives are added by default: <BR>transfer_executable=True <BR>stream_output=False <BR>stream_error=False <BR>notification=Error <BR>copy_to_spool=false </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorgt2.environ</td>
    <td class="tg-raw32"> list of environment variables, splitted by white spaces, to be included in the condor attribute environment <strong>verbatim</strong> Therefore, the format should be env1=var1 env2=var2 envN=varN split by whitespaces. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorgt2.gridresource</td>
    <td class="tg-raw32"> name of the CE (e.g. gridtest01.racf.bnl.gov/jobmanager-condor) </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorgt2.proxy</td>
    <td class="tg-raw32"> name of the proxy handler in proxymanager for automatic proxy renewal (See etc/proxy.conf) None if no automatic proxy renewal is desired. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorgt2.submitargs</td>
    <td class="tg-raw32"> list of command line input options to be included in the submission command <strong>verbatim</strong> e.g.  batchsubmit.condorgt2.submitargs = -remote my_schedd will drive into a command like condor_submit -remote my_schedd submit.jdl </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td colspan="3" class="tg-splitheader">GlobusRSL GRAM2 variables</td>
  </tr>
<tr>
    <td class="tg-raw31">gram2</td>
    <td class="tg-raw32"> The following are GRAM2 RSL variables.  They are just used to build batchsubmit.condorgt2.globusrsl (if needed) The globusrsl directive in the condor submission file looks like globusrsl=(jobtype=single)(queue=short) Documentation can be found here: <a href="http://www.globus.org/toolkit/docs/2.4/gram/gram_rsl_parameters.html" target="_top">http://www.globus.org/toolkit/docs/2.4/gram/gram_rsl_parameters.html</a> </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">globusrsl.gram2.&#60;argument&#62;</td>
    <td class="tg-raw32"> globusrsl.gram2.count <BR> globusrsl.gram2.directory <BR> globusrsl.gram2.dryRun <BR> globusrsl.gram2.environment <BR> globusrsl.gram2.executable <BR> globusrsl.gram2.gramMyJob <BR> globusrsl.gram2.hostCount <BR> globusrsl.gram2.jobType <BR> globusrsl.gram2.maxCpuTime <BR> globusrsl.gram2.maxMemory <BR> globusrsl.gram2.maxTime <BR> globusrsl.gram2.maxWallTime <BR> globusrsl.gram2.minMemory <BR> globusrsl.gram2.project <BR> globusrsl.gram2.queue <BR> globusrsl.gram2.remote_io_url <BR> globusrsl.gram2.restart <BR> globusrsl.gram2.save_state <BR> globusrsl.gram2.stderr <BR> globusrsl.gram2.stderr_position <BR> globusrsl.gram2.stdin <BR> globusrsl.gram2.stdout <BR> globusrsl.gram2.stdout_position <BR> globusrsl.gram2.two_phase </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">globusrsl.gram2.globusrsl</td>
    <td class="tg-raw32"> GRAM RSL directive.  If this variable is not setup, then it will be built programmatically from all non empty globusrsl.gram2.XYZ variables.  If this variable is setup, then its value will be taken <strong>verbatim</strong>, and all possible values for globusrsl.gram2.XYZ variables will be ignored.  </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">globusrsl.gram2.globusrsladd</td>
    <td class="tg-raw32"> custom fields to be added <strong>verbatim</strong> to the GRAM RSL directive, after it has been built either from globusrsl.gram2.globusrsl value or from all globusrsl.gram2.XYZ variables.  e.g. (condorsubmit=('+AccountingGroup' '\"group_atlastest.usatlas1\"')('+Requirements' 'True')) </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td colspan="3" class="tg-splitheader">Configuration when batchsubmitplugin is condorgt5</td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorgt5.condor_attributes</td>
    <td class="tg-raw32"> list of condor attributes, splited by comma, to be included in the condor submit file <strong>verbatim</strong> e.g. +Experiment = "ATLAS",+VO = "usatlas",+Job_Type = "cas" Can be used to include any line in the Condor-G file that is not otherwise added programmatically by AutoPyFactory.  Note the following directives are added by default: <BR>transfer_executable=True <BR>stream_output=False <BR>stream_error=False <BR>notification=Error <BR>copy_to_spool=false </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorgt5.environ</td>
    <td class="tg-raw32"> list of environment variables, splitted by white spaces, to be included in the condor attribute environment <strong>verbatim</strong> Therefore, the format should be env1=var1 env2=var2 envN=varN split by whitespaces. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorgt5.gridresource</td>
    <td class="tg-raw32"> name of the CE (e.g. gridtest01.racf.bnl.gov/jobmanager-condor) </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorgt5.proxy</td>
    <td class="tg-raw32"> name of the proxy handler in proxymanager for automatic proxy renewal (See etc/proxy.conf) None if no automatic proxy renewal is desired. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorgt5.submitargs</td>
    <td class="tg-raw32"> list of command line input options to be included in the submission command <strong>verbatim</strong> e.g.  batchsubmit.condorgt2.submitargs = -remote my_schedd will drive into a command like condor_submit -remote my_schedd submit.jdl </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td colspan="3" class="tg-splitheader">GlobusRSL GRAM5 variables</td>
  </tr>
<tr>
    <td class="tg-raw31">gram5</td>
    <td class="tg-raw32"> The following are GRAM5 RSL variables.  They are just used to build batchsubmit.condorgt5.globusrsl (if needed) The globusrsl directive in the condor submission file looks like globusrsl=(jobtype=single)(queue=short) Documentation can be found here: <a href="http://www.globus.org/toolkit/docs/5.2/5.2.0/gram5/user/#gram5-user-rsl" target="_top">http://www.globus.org/toolkit/docs/5.2/5.2.0/gram5/user/#gram5-user-rsl</a>  </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">globusrsl.gram5.&#60;argument&#62;</td>
    <td class="tg-raw32"> globusrsl.gram5.count <BR> globusrsl.gram5.directory <BR> globusrsl.gram5.dry_run <BR> globusrsl.gram5.environment <BR> globusrsl.gram5.executable <BR> globusrsl.gram5.file_clean_up <BR> globusrsl.gram5.file_stage_in <BR> globusrsl.gram5.file_stage_in_shared <BR> globusrsl.gram5.file_stage_out <BR> globusrsl.gram5.gass_cache <BR> globusrsl.gram5.gram_my_job <BR> globusrsl.gram5.host_count <BR> globusrsl.gram5.job_type <BR> globusrsl.gram5.library_path <BR> globusrsl.gram5.loglevel <BR> globusrsl.gram5.logpattern <BR> globusrsl.gram5.max_cpu_time <BR> globusrsl.gram5.max_memory <BR> globusrsl.gram5.max_time <BR> globusrsl.gram5.max_wall_time <BR> globusrsl.gram5.min_memory <BR> globusrsl.gram5.project <BR> globusrsl.gram5.proxy_timeout <BR> globusrsl.gram5.queue <BR> globusrsl.gram5.remote_io_url <BR> globusrsl.gram5.restart <BR> globusrsl.gram5.rsl_substitution <BR> globusrsl.gram5.savejobdescription <BR> globusrsl.gram5.save_state <BR> globusrsl.gram5.scratch_dir <BR> globusrsl.gram5.stderr <BR> globusrsl.gram5.stderr_position <BR> globusrsl.gram5.stdin <BR> globusrsl.gram5.stdout <BR> globusrsl.gram5.stdout_position <BR> globusrsl.gram5.two_phase <BR> globusrsl.gram5.username </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">globusrsl.gram5.globusrsl</td>
    <td class="tg-raw32"> GRAM RSL directive.  If this variable is not setup, then it will be built programmatically from all non empty globusrsl.gram5.XYZ variables.  If this variable is setup, then its value will be taken <strong>verbatim</strong>, and all possible values for globusrsl.gram5.XYZ variables will be ignored.  </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">globusrsl.gram5.globusrsladd</td>
    <td class="tg-raw32"> custom fields to be added <strong>verbatim</strong> to the GRAM RSL directive, after it has been built either from globusrsl.gram5.globusrsl value or from all globusrsl.gram5.XYZ variables.  e.g. (condorsubmit=('+AccountingGroup' '\"group_atlastest.usatlas1\"')('+Requirements' 'True')) </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td colspan="3" class="tg-splitheader">Configuration when batchsubmitplugin is condorcream</td>
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorcream.batch</td> 
    <td class="tg-raw32"> local batch system (pbs, sge...) </td> <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorcream.condor_attributes</td> 
    <td class="tg-raw32"> list of condor attributes, splited by comma, to be included in the condor submit file <strong>verbatim</strong> e.g. +Experiment = "ATLAS",+VO = "usatlas",+Job_Type = "cas" Can be used to include any line in the Condor-G file that is not otherwise added programmatically by AutoPyFactory.  Note the following directives are added by default: <BR>transfer_executable=True <BR>stream_output=False <BR>stream_error=False <BR>notification=Error <BR>copy_to_spool=false </td> <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorcream.environ</td> 
    <td class="tg-raw32"> list of environment variables, splitted by white spaces, to be included in the condor attribute environment <strong>verbatim</strong> Therefore, the format should be env1=var1 env2=var2 envN=varN split by whitespaces. </td> <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorcream.gridresource</td> 
    <td class="tg-raw32"> grid resource, built from other vars using interpolation: batchsubmit.condorcream.gridresource = %(batchsubmit.condorcream.webservice)s:%(batchsubmit.condorcream.port)s/ce-cream/services/CREAM2 %(batchsubmit.condorcream.batch)s %(batchsubmit.condorcream.queue)s </td> <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorcream.port</td> 
    <td class="tg-raw32"> port number. </td> <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorcream.proxy</td> 
    <td class="tg-raw32"> name of the proxy handler in proxymanager for automatic proxy renewal (See etc/proxy.conf) None if no automatic proxy renewal is desired. </td> <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorcream.queue</td> 
    <td class="tg-raw32"> queue within the local batch system (e.g. short) </td> <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorcream.submitargs</td> 
    <td class="tg-raw32"> list of command line input options to be included in the submission command <strong>verbatim</strong> e.g.  batchsubmit.condorgt2.submitargs = -remote my_schedd will drive into a command like condor_submit -remote my_schedd submit.jdl </td> <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorcream.webservice</td> 
    <td class="tg-raw32"> web service address (e.g. ce04.esc.qmul.ac.uk:8443/ce-cream/services/CREAM2) </td> <td class="tg-raw33"> </td> 
  </tr>
<tr>
    <td colspan="3" class="tg-splitheader">Configuration when batchsubmitplugin is condorosgce</td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorosgce.condor_attributes</td>
    <td class="tg-raw32"> list of condor attributes, splited by comma, to be included in the condor submit file <strong>verbatim</strong> </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorosgce.gridresource</td>
    <td class="tg-raw32"> to be used in case schedd and collector are the same </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorosgce.port</td>
    <td class="tg-raw32"> port number of the remote HTCondor-CE <br> default=9619 </td>
    <td class="tg-raw33"> new in 2.4.7</td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorosgce.proxy</td>
    <td class="tg-raw32"> name of the proxy handler in proxymanager for automatic proxy renewal (See etc/proxy.conf) None if no automatic proxy renewal is desired. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td colspan="3" class="tg-splitheader">Configuration when batchsubmitplugin is condorec2</td>
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorec2.gridresource</td> 
    <td class="tg-raw32"> ec2 service's URL (e.g. <a href="https://ec2.amazonaws.com/" target="_top">https://ec2.amazonaws.com/</a> ) </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorec2.submitargs</td> 
    <td class="tg-raw32"> list of command line input options to be included in the submission command <strong>verbatim</strong> e.g.  batchsubmit.condorgt2.submitargs = -remote my_schedd will drive into a command like condor_submit -remote my_schedd submit.jdl </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorec2.condor_attributes</td> 
    <td class="tg-raw32"> list of condor attributes, splited by comma, to be included in the condor submit file <strong>verbatim</strong> </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorec2.environ</td> 
    <td class="tg-raw32"> list of environment variables, splitted by white spaces, to be included in the condor attribute environment <strong>verbatim</strong> Therefore, the format should be env1=var1 env2=var2 envN=varN split by whitespaces. </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorec2.ami_id</td> 
    <td class="tg-raw32"> identifier for the VM image, previously registered in one of Amazon's storage service (S3 or EBS) </td>
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorec2.ec2_spot_price</td> 
    <td class="tg-raw32"> max price to pay, in dollars to three decimal places. e.g. .040 </td>
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorec2.instance_type</td> 
    <td class="tg-raw32"> hardware configurations for instances to run on, .e.g m1.medium </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorec2.user_data</td> 
    <td class="tg-raw32"> up to 16Kbytes of contextualization data.  This makes it easy for many instances to share the same VM image, but perform different work. </td>
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorec2.access_key_id</td> 
    <td class="tg-raw32"> path to file with the EC2 Access Key ID </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorec2.secret_access_key</td> 
    <td class="tg-raw32"> path to file with the EC2 Secret Access Key </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condorec2.proxy</td> 
    <td class="tg-raw32"> name of the proxy handler in proxymanager for automatic proxy renewal (See etc/proxy.conf) None if no automatic proxy renewal is desired. </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr>
    <td colspan="3" class="tg-splitheader">Configuration when batchsubmitplugin is condordeltacloud</td>
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condordeltacloud.gridresource</td> 
    <td class="tg-raw32"> ec2 service's URL (e.g. <a href="https://deltacloud.foo.org/api" target="_top">https://deltacloud.foo.org/api</a> ) </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condordeltacloud.username</td> 
    <td class="tg-raw32"> credentials in DeltaCloud </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condordeltacloud.password_file</td> 
    <td class="tg-raw32"> path to the file with the password </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condordeltacloud.image_id</td> 
    <td class="tg-raw32"> identifier for the VM image, previously registered with the cloud service. </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condordeltacloud.keyname</td> 
    <td class="tg-raw32"> in case of using SSH, the command keyname specifies the identifier of the SSH key pair to use.  </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condordeltacloud.realm_id</td> 
    <td class="tg-raw32"> selects one between multiple locations the cloud service may have. </td>
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condordeltacloud.hardware_profile</td> 
    <td class="tg-raw32"> selects one between the multiple hardware profiles the cloud service may provide </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condordeltacloud.hardware_profile_memory</td> 
    <td class="tg-raw32"> customize the hardware profile </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condordeltacloud.hardware_profile_cpu</td> 
    <td class="tg-raw32"> customize the hardware profile </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condordeltacloud.hardware_profile_storage</td> 
    <td class="tg-raw32"> customize the hardware profile </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">batchsubmit.condordeltacloud.user_data</td> 
    <td class="tg-raw32"> contextualization data </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr>
    <td colspan="3" class="tg-splitheader">Configuration when batchsubmitplugin is condorlocal</td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorlocal.condor_attributes</td>
    <td class="tg-raw32"> list of condor attributes, splited by comma, to be included in the condor submit file <strong>verbatim</strong> e.g. +Experiment = "ATLAS",+VO = "usatlas",+Job_Type = "cas" Can be used to include any line in the Condor-G file that is not otherwise added programmatically by AutoPyFactory.  Note the following directives are added by default: <BR>universe = vanilla <BR>transfer_executable = True <BR>should_transfer_files = IF_NEEDED <BR>+TransferOutput = "" <BR>stream_output=False <BR>stream_error=False <BR>notification=Error <BR>periodic_remove = (JobStatus <code>= 5 &amp;&amp; (CurrentTime - EnteredCurrentStatus) &gt; 3600) &verbar;&verbar; (JobStatus =</code> 1 &amp;&amp; globusstatus <code>!</code> 1 &amp;&amp; (CurrentTime - EnteredCurrentStatus) &gt; 86400) <BR>To be used in CondorLocal Batch Submit Plugin. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorlocal.environ</td>
    <td class="tg-raw32"> list of environment variables, splitted by white spaces, to be included in the condor attribute environment <strong>verbatim</strong> To be used by CondorLocal Batch Submit Plugin.  Therefore, the format should be env1=var1 env2=var2 envN=varN split by whitespaces. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorlocal.proxy</td>
    <td class="tg-raw32"> name of the proxy handler in proxymanager for automatic proxy renewal (See etc/proxy.conf) None if no automatic proxy renewal is desired. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorlocal.submitargs</td>
    <td class="tg-raw32"> list of command line input options to be included in the submission command <strong>verbatim</strong> e.g.  batchsubmit.condorgt2.submitargs = -remote my_schedd will drive into a command like condor_submit -remote my_schedd submit.jdl </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td colspan="3" class="tg-splitheader">Configuration when batchsubmitplugin is condorlsf</td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorlsf.proxy</td>
    <td class="tg-raw32"> name of the proxy handler in proxymanager for automatic proxy renewal (See etc/proxy.conf) None if no automatic proxy renewal is desired. </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td colspan="3" class="tg-splitheader">Configuration when batchsubmitplugin is nordugrid</td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condornordugrid.gridresource</td>
    <td class="tg-raw32"> name of the ARC CE i.e. lcg-lrz-ce2.grid.lrz.de </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">nordugridrsl</td>
    <td class="tg-raw32"> Entire RSL line.  i.e. (jobname = 'prod_pilot')(queue=lcg)(runtimeenvironment = APPS/HEP/ATLAS-SITE-LCG)(runtimeenvironment = ENV/PROXY ) (environment = ('APFFID' 'voatlas94') ('PANDA_JSID' 'voatlas94') ('GTAG' 'http://voatlas94.cern.ch/pilots/2012-11-19/LRZ-LMU_arc/$(Cluster).$(Process).out') ('RUCIO_ACCOUNT' 'pilot') ('APFCID' '$(Cluster).$(Process)') ('APFMON' 'http://apfmon.lancs.ac.uk/mon/') ('FACTORYQUEUE' 'LRZ-LMU_arc')  </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">nordugridrsladd</td>
    <td class="tg-raw32"> A given tag to be added to the Nordugrid RSL line </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">nordugridrsl.addenv.<XYZ></td>
    <td class="tg-raw32"> A given tag to be added within the 'environment' tag to the Nordugrid RSL line i.e. nordugridrsl.addenv.RUCIO_ACCOUNT = pilot will be added as ('RUCIO_ACCOUNT' 'pilot' ) </td>
    <td class="tg-raw33"> </td>
  </tr>
</table>
<p />
<!--
<tr>
    <td class="tg-raw31">batchsubmit.condorosgce.remote_condor_collector</td>
    <td class="tg-raw32"> condor collector </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">batchsubmit.condorosgce.remote_condor_schedd</td>
    <td class="tg-raw32"> condor schedd  </td>
    <td class="tg-raw33"> </td>
  </tr>
-->
<p />
<p />
<!--
 This variable can be built making use of an auxiliar variable called executable.defaultarguments This proposed ancilla works as a template, and its content is created on the fly from the value of other variables.  This mechanism is called "interpolation", docs can be found here: <a href="http://docs.python.org/library/configparser.html" target="_top">http://docs.python.org/library/configparser.html</a> 
An example of this type of templates (included in the DEFAULTS block) is like this: executable.defaultarguments = --wrappergrid=%(grid)s \ --wrapperwmsqueue=%(wmsqueue)s \ --wrapperbatchqueue=%(batchqueue)s \ --wrappervo=%(vo)s \ --wrappertarballurl=http://dev.racf.bnl.gov/dist/wrapper/wrapper.tar.gz \ --wrapperserverurl=http://pandaserver.cern.ch:25080/cache/pilot \ --wrapperloglevel=debug executable.defaultarguments =  -s %(wmsqueue)s \ -h %(batchqueue)s -p 25443 \ -w <a href="https://pandaserver.cern.ch" target="_top">https://pandaserver.cern.ch</a>  -j false  -k 0  -u user |
-->
<p />
<h1><a name="7_proxy_conf"></a> 7  proxy.conf </h1>
<p />
<!--
<strong>FIXME: check which variables are really mandatory and which ones are optional</strong>
<p />
<strong>FIXME: fix the format on the multiline cells</strong>
<p />
<strong>FIXME: fix the fake wiki words</strong>
-->
<p />
<table class="tg">
  <tr>
    <th class="tg-header">variable</th>
    <th class="tg-header">description</th>
    <th class="tg-header">comments</th>
  </tr>
<tr> 
    <td class="tg-raw31">baseproxy</td> 
    <td class="tg-raw32"> If used, create a very long-lived proxy, e.g. grid-proxy-init -valid 720:0 -out /tmp/plainProxy Note that maintenance of this proxy must occur completely outside of APF. </td> 
    <td class="tg-raw33"> Only needed if flavor=voms</td> 
  </tr>
<tr> 
    <td class="tg-raw31">checktime</td> 
    <td class="tg-raw32"> How often to check proxy validity, in seconds</td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">flavor</td> 
    <td class="tg-raw32"> voms or myproxy. voms directly generates proxy using cert or baseproxy myproxy retrieves a proxy from myproxy, then generates the target proxy against voms using it as baseproxy.</td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">initdelay</td> 
    <td class="tg-raw32"> In seconds, how long to wait before generating. Needed for MyProxy when using cert authentication--we need to allow time for the auth credential to be generated (by another proxymanager profile). </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">interruptcheck</td> 
    <td class="tg-raw32"> Frequency to check for keyboard/signal interrupts, in seconds</td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">lifetime</td> 
    <td class="tg-raw32"> Initial lifetime, in seconds (604800 = 7 days)</td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">minlife</td> 
    <td class="tg-raw32"> Minimum lifetime of VOMS attributes for a proxy (renew if less) in seconds</td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">myproxy_hostname</td> 
    <td class="tg-raw32"> Myproxy server host. </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">myproxy_passphrase</td> 
    <td class="tg-raw32"> Passphrase for proxy retrieval from MyProxy</td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">myproxy_username</td> 
    <td class="tg-raw32"> User name to be used on MyProxy service</td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">owner</td> 
    <td class="tg-raw32"> If running standalone (as root) and you want the proxy to be owned by another account. </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">proxyfile</td> 
    <td class="tg-raw32"> Target proxy path.</td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">renew</td> 
    <td class="tg-raw32"> If you do not want to use ProxyManager to renew proxies, set this False and only define 'proxyfile' If renew is set to false, then no grid client setup is necessary. </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">retriever_profile</td> 
    <td class="tg-raw32"> A list of other proxymanager profiles to be used to authorize proxy retrieval from MyProxy. </td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">usercert</td> 
    <td class="tg-raw32"> Path to the user grid certificate file</td>
    <td class="tg-raw33"> Only needed if flavor=voms</td> 
  </tr>
<tr> 
    <td class="tg-raw31">userkey</td> 
    <td class="tg-raw32"> Path to the user grid key file</td> 
    <td class="tg-raw33"> Only needed if flavor=voms</td> 
  </tr>
<tr> 
    <td class="tg-raw31">voms.args</td> 
    <td class="tg-raw32"> Any extra arbitrary input option to be added to voms-proxy-init command</td> 
    <td class="tg-raw33"> </td> 
  </tr>
<tr> 
    <td class="tg-raw31">vorole</td> 
    <td class="tg-raw32"> User VO role for target proxy. MyProxy Retrieval Functionality: Assumes you have created a long-lived proxy in a MyProxy server, out of band. </td> 
    <td class="tg-raw33"> mandatory</td> 
  </tr>
</table>
<p />
<!--
<p />
<tr> 
    <td class="tg-raw31">remote_group</td> 
    <td class="tg-raw32"> Proxy Maintenance (assumes you have enabled ssh-agent key-based access to the remote host where you want to maintain a proxy file). If connect user is root, what group should own the file </td>
    <td class="tg-raw33"> optional</td> 
  </tr>
<tr> 
    <td class="tg-raw31">remote_host</td> 
    <td class="tg-raw32"> Proxy Maintenance (assumes you have enabled ssh-agent key-based access to the remote host where you want to maintain a proxy file). Copy proxyfile to same path on remote host </td> 
    <td class="tg-raw33"> optional</td> 
  </tr>
<tr> 
    <td class="tg-raw31">remote_owner</td> 
    <td class="tg-raw32"> Proxy Maintenance (assumes you have enabled ssh-agent key-based access to the remote host where you want to maintain a proxy file). If connect user is root, what account should own the file </td> 
    <td class="tg-raw33"> optional</td> 
  </tr>
<tr> 
    <td class="tg-raw31">remote_user</td> 
    <td class="tg-raw32"> Proxy Maintenance (assumes you have enabled ssh-agent key-based access to the remote host where you want to maintain a proxy file). User to connect as </td> 
    <td class="tg-raw33"> optional</td> 
  </tr>
<p />
-->
<p />
<p />
<h1><a name="8_monitor_conf"></a> 8  monitor.conf </h1>
<p />
<!--
<strong>FIXME: check which variables are really mandatory and which ones are optional</strong>
<p />
<strong>FIXME: fix the format on the multiline cells</strong>
<p />
<strong>FIXME: fix the fake wiki words</strong>
-->
<p />
<p />
<table class="tg">
  <tr>
    <th class="tg-header">variable</th>
    <th class="tg-header">description</th>
    <th class="tg-header">comments</th>
  </tr>
<tr>
    <td class="tg-raw31">monitorplugin</td>
    <td class="tg-raw32"> the type of plugin to handle this monitor instance </td>
    <td class="tg-raw33"> </td>
  </tr>
<tr>
    <td class="tg-raw31">monitorURL</td>
    <td class="tg-raw32"> URL for the web monitor </td>
    <td class="tg-raw33"> </td>
  </tr>
</table> 

</body></html>
