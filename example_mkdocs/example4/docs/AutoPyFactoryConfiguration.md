</div>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<h1><a name="Configuration_of_AutoPyFactory"></a>  Configuration of AutoPyFactory </h1>
<li> 1  About this Document</a>
</li> <li> 2  Applicable versions</a>
</li> <li> 3  Format of the configuration files</a>
</li> <li> 4  Reference Manual</a>
</li> <li> 5  Configuration files</a> <ul>
<li> 5.1  sysconfig/autopyfactory</a>
</li> <li> 5.2  logrotation</a>
</li> <li> 5.3  autopyfactory.conf</a>
</li> <li> 5.4  queues.conf</a>
</li> <li> 5.5  proxy.conf</a>
</li> <li> 5.6  monitor.conf</a>
</li> <li> 5.7  mappings.conf</a>
</li></ul> 
</li> <li> 6  Workflows</a>
</li></ul> 
<p />
<h1><a name="1_About_this_Document"></a> 1  About this Document </h1>
<p />
This document describes all configuration variables in <a href="../index.html" class="twikiLink">AutoPyFactory</a>
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
The format of the configuration files is similar to the Microsoft INI files. The configuration file consists of sections, led by a <code>[section]</code> header and followed by <code>name: value</code> entries, with continuations in the style of <a href="http://tools.ietf.org/html/rfc822.html" target="_top">RFC 822</a> (see section 3.1.1, LONG HEADER FIELDS); <code>name=value</code> is also accepted. Note that leading whitespace is removed from values. Additional defaults can be provided on initialization and retrieval. Lines beginning with '#' or ';' are ignored and may be used to provide comments.
<p />
It accepts interpolation. This means values can contain format strings which refer to other values in the same section, or values in a special <code>[DEFAULT]</code> section.
<p />
Configuration files may include comments, prefixed by specific characters (# and ;). Comments may appear on their own in an otherwise empty line, or may be entered in lines holding values or section names. In the latter case, they need to be preceded by a whitespace character to be recognized as a comment. (For backwards compatibility, only ; starts an inline comment, while # does not.)
<p />
As the python package ConfigParser is being used to digest the configuration files, a wider explanation and examples can be found in the <a href="https://docs.python.org/2/library/configparser.html" target="_top">python documentation page</a>. 
<p />
<h1><a name="4_Reference_Manual"></a> 4  Reference Manual </h1>
<p />
A detailed description of all configuration variables used in AutoPyFactory can be found <a href="../AutoPyFactoryReferenceManual/">here</a>
<p />
<h1><a name="5_Configuration_files"></a> 5  Configuration files </h1>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="5_1_sysconfig_autopyfactory"></a> 5.1  sysconfig/autopyfactory </span></h2>
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
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="5_2_logrotation"></a> 5.2  logrotation </span></h2>
<p />
The default location of the log files, as they are in the <code>/etc/sysconfig/autopyfactory</code> config file, are 
<pre class="rootscreen">
[root@factory ~]$ ls /var/log/autopyfactory/
/var/log/autopyfactory/autopyfactory.log
/var/log/autopyfactory/console.log (when used)
</pre>
<p />
However, if the location of the files is different and the <code>/etc/sysconfig/autopyfactory</code> file has been modified, 
then the logrotation file <code>/etc/logrotate.d/autopyfactory</code> needs to be adjusted accordingly.
<p />
<pre class="file">
/var/log/autopyfactory/autopyfactory.log {   <b>\lt&;-- CHANGE THIS IF NEEDED. </b>
  missingok
  notifempty
  sharedscripts
  size 100M
  rotate 10
  prerotate
    [ -e /etc/init.d/autopyfactory ] && /etc/init.d/autopyfactory stop >/dev/null 2>&1 || true
    sleep 5
  endscript
  postrotate
    sleep 5
    [ -e /etc/profile ] && . /etc/profile >/dev/null 2>&1 || true
    [ -e /etc/init.d/autopyfactory ] && /etc/init.d/autopyfactory start >/dev/null 2>&1 || true
  endscript
}
/var/log/autopyfactory/console.log {    <b>\lt&;-- CHANGE THIS IF NEEDED. </b>
  missingok
  notifempty
  sharedscripts
  size 50M
  rotate 2
  prerotate
    [ -e /etc/init.d/autopyfactory ] && /etc/init.d/autopyfactory stop >/dev/null 2>&1 || true
    sleep 5
  endscript
  postrotate
    sleep 5
    [ -e /etc/profile ] && . /etc/profile >/dev/null 2>&1 || true
    [ -e /etc/init.d/autopyfactory ] && /etc/init.d/autopyfactory start >/dev/null 2>&1 || true
  endscript
}

</pre>

<p />
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="5_3_autopyfactory_conf"></a> 5.3  autopyfactory.conf </span></h2>
<p />
It is the configuration file where the generic parameters for the whole factory are set. 
Its location and name, <code>/etc/autopyfactory/autopyfactory.conf</code>, is set in the daemon init script, <code>/etc/init.d/autopyfactory</code>, and therefore cannot be changed. 
<p />
A detailed description of this file variables can be found in the <a href="AutoPyFactoryReferenceManual.html#5_autopyfactory_conf" target="_top">Reference Manual</a>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="5_4_queues_conf"></a> 5.4  queues.conf </span></h2>
<p />
It is the file where each APFQueue is defined. 
AutoPyFactory is built around the concept of APFQueues. An APFQueue can be seen as the unique combination of these concepts: <ul>
<li> a wms system queue: where the payload jobs are waiting. The concept of wms queue is an abstraction, and it will depend on what WMS System is being used each case. Whe the WMS System is <code>PanDA</code>, the wms queue is the panda queue. When the WMS System is a condor pool, a wms queue is each different value of the classad <code>+MATCH_APF_QUEUE</code> that jobs in that condor pool carry.
</li> <li> a batch system queue: where the pilots will be submitted. It could be a CE based on globus GT5, or GT2. It could be a CE based on CREAM. A CE based on HTCondor-CE. It could be an Amazon EC2 workflow managed by condor. It can be a remote condor pool. For all possibilities, chech the <a href="../AutoPyFactoryReferenceManual/" target="_top">Reference Manual</a>
</li></ul> 
<p />
That, plus an algorithm to decide how many pilots to submit each cycle.
<p />
Therefore, each APFQueue in AutoPyFactory needs to set, at least, these elements: <ul>
<li> the plugin to query the wms service being used
</li> <li> the name of the queue in that wms service (wmsqueue)
</li> <li> the plugin to query the status of previous pilots in the batch system
</li> <li> the plugin to submit new pilots to the batch system
</li> <li> the plugin(s) to decide how many new pilots to submit each cycle 
</li></ul> 
<p />
The complete list of configuration parameters can be found <a href="../AutoPyFactoryReferenceManual/" target="_top">Reference Manual</a>
<p />
For each APFQueue, a section is set in this file. All APFQueues sections become threads running asynchronously. 
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="5_5_proxy_conf"></a> 5.5  proxy.conf </span></h2>
<p />
It is the file where the x509 proxy files are set, when needed. 
The complete list of configuration parameters can be found <a href="../AutoPyFactoryReferenceManual/" target="_top">Reference Manual</a>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="5_6_monitor_conf"></a> 5.6  monitor.conf </span></h2>
<p />
It is the file to setup, if needed, where the factory will send information periodically for web monitor. 
The complete list of configuration parameters can be found <a href="../AutoPyFactoryReferenceManual/" target="_top">Reference Manual</a>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="5_7_mappings_conf"></a> 5.7  mappings.conf </span></h2>
<p />
This file is being used internally by the factory to translate external tools information into internal nomenclature. 
It defines completely the behavior of the factory. Unless you are an expert on the factory, <strong>DO NOT TOUCH IT</strong>.
<p />
<h1><a name="6_Workflows"></a> 6  Workflows </h1>
<p /> <ul>
<li> Configuration for WMS being PanDA <a href="../AutoPyFactoryWorkflowPanda/" target="_top">here</a>
</li> <li> glideins based workflows. Some details on how to configure the factory to work in a glidein style are <a href="../AutoPyFactoryWorkflowGlidein/" target="_top">here</a>
</li> <li> Submission to CREAM CE <a href="../AutoPyFactoryWorkflowCream/" target="_top">here</a>
</li> <li> Submission to NorduGrid CE <a href="../AutoPyFactoryWorkflowNorduGrid/" target="_top">here</a>
</li> <li> Submission to EC2 <a href="../AutoPyFactoryWorkflowEC2/" target="_top">here</a>
</li></ul>  

</body></html>
