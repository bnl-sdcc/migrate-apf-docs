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
<p />
<h1><a name="Questions_and_Answers_related_Au"></a>  Questions and Answers related AutoPyFactory </h1>


<p />
<h1><a name="1_About_this_Document"></a> 1  About this Document </h1>
<p />
This document is just a somehow free-format list of questions asked about <a href="../index.html" class="twikiLink">AutoPyFactory</a>, and some of the answers provided. 
The answer may be straight forward, or may be just some hints about where to look.
<p />
It does not include the complete list of questions, as this documented was started on August 10th, 2015. 
It is not a FAQ page, therefore.
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
<h1><a name="2_Applicable_versions"></a> 2  Applicable versions </h1>
This document mostly applies to the version of AutoPyFactory 2.4.5 and higher. Some answer may be valid for older versions too, but that is not guaranteed. 
<p />
<h1><a name="3_Questions_and_answers"></a> 3  Questions and answers </h1>
<p />
<!--
<p />
Maybe we need an entry on "condor configuration" ???
<p />
-->
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_1_how_do_I_interpret_comments"></a> 3.1  how do I interpret comments like "fixed in version X" at the same time that "This documentation applies to the latest version of APF: Y"? </span></h2>
<p />
When the versions X and Y are different, usually that means that a bug was found in the version Y -the one the current documentation is for-, 
and a fix for that bug will be released in a future version -X or higher-, but not yet. 
<br>
It can also mean that we forgot to update the message "This documentation applies to the latest version of APF... " <img src="https://twiki.grid.iu.edu/twiki/pub/TWiki/SmiliesPlugin/wink.gif" alt="wink" title="wink" border="0" /> 
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_2_what_do_I_do_if_I_want_to_ch"></a> 3.2  what do I do if I want to change the path of the log files? </span></h2>
<p />
Some explanation can be found <a href="../AutoPyFactoryConfiguration/" target="_top">here</a> 
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_3_how_can_I_submit_to_a_remote"></a> 3.3  how can I submit to a remote HTCondor schedd? </span></h2>
<p />
It is not needed the APF host also be an HTCondor schedd. 
Remote submission is allowed. 
The queryargs -for the BatchStatus Plugin- and the submitargs -for the Submit Plugin- can be used to pass any arbitrary options, for example to refer to a remote schedd.
<p />
<pre class="file">
    batchsubmitplugin = CondorLocal
    batchsubmit.condorlocal.submitargs = -remote &lt;remote_schedd&gt;:&lt;port&gt;
 
    batchstatusplugin = Condor
    batchstatus.condor.queryargs = -pool &lt;remote_pool&gt; -name &lt;remote_schedd&gt;
</pre>
<p />
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_4_why_pilots_submitted_to_a_CR"></a> 3.4  why pilots submitted to a CREAM CE fail? </span></h2>
<p />
If your pilots being submitted to a CREAM CE fail, with this type of content in the condor log file:
<p />
<pre class="file">
        009 (45454545.000.000) 12/10 20:35:01 Job was aborted by the user.
            CREAM error: BLAH error: submission command failed (exit code = 1) (stdout:) (stderr:) N/A (jobId = CREAM123456789)
</pre>
<p />
it is possible that the grid_resource line in the condor submit file is not correct. 
AutoPyFactory creates a line like this
<p />
<pre class="file">
grid_resource = cream matrix.net:8443/ce-cream/services/CREAM2 pbs queuename
</pre>
<p />
while perhaps the CE needs a line like
<p />
<pre class="file">
grid_resource = cream matrix.net:8443/queuename
</pre>
<p />
If that is the case, or similar, there is dirty trick to fix it. Add this line to your queue configuration (typically in queues.conf):
<p />
<pre class="file">
batchsubmit.condorcream.condor_attributes.grid_resource = cream matrix.net:8443/queuename
</pre>
<p />
<p />
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_5_to_which_type_of_resource_ta"></a> 3.5  to which type of resource targets I can submit pilots with AutoPyFactory? </span></h2>
<p />
In principle, to those supported by HTCondor. That includes globus GT2, globus GT5, HTCondor-CE, CREAM CE, NorduGrid CE, Amazon EC2, a local or remote HTCondor cluster,...
If HTCondor cannot submit to a given resource, then AutoPyFactory cannot either.
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_6_how_do_I_find_out_the_versio"></a> 3.6  how do I find out the version of AutoPyFactory I have installed? </span></h2>
<p />
A rpm command will give the answer:
<p />
<pre class="rootscreen">
[root@factory ~]$ rpm -qa | grep autopyfactory
autopyfactory-tools-1.0.1-1.noarch
autopyfactory-2.4.1-1.noarch
</pre>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_7_how_should_I_configure_the_f"></a> 3.7  how should I configure the factory if I want to renew the proxies myself? </span></h2>
<p />
If you don't want the factory to renew the x509 proxies for you because you have in place a different mechanism for that, you only need to disable the renewal feature. 
But, if the factory is configured to submit to grid sites or any other resource that requires an x509 file, the proxymanager still must be enabled anyways. 
So, in your <code><b>autopyfactory.conf</b></code> file:
<p />
<pre class="file">
proxymanager.enabled = True
</pre>
<p />
but in your <code>proxy.conf</code> file:
<pre class="file">
renew = False
</pre>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_8_how_can_I_add_environment_va"></a> 3.8  how can I add environment variables to the condor submit file? </span></h2>
<p />
Using the configuration variable <code>batchsubmit.&lt;plugin&gt;.environ</code>. 
For details, check the <a href="../AutoPyFactoryReferenceManual/" target="_top">Reference Manual</a>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_9_how_can_I_add_any_arbitrary"></a> 3.9  how can I add any arbitrary line to the condor submit file? </span></h2>
<p />
Using the configuration variable <code>batchsubmit.&lt;plugin&gt;.condor_attributes</code>. 
For details, check the <a href="../AutoPyFactoryReferenceManual/" target="_top">Reference Manual</a>
<p />
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_10_how_can_I_tune_the_number_o"></a> 3.10  how can I tune the number of pilots to be submitted depending on the number of activated jobs in PanDA and the number of pilots still pending? </span></h2>
<p />
The sched plugin <code>Ready</code> checks the number of activated jobs, the number of idle pilots, and returns the difference. 
For details on the sched plugins parameters, check the <a href="../AutoPyFactoryReferenceManual/" target="_top">Reference Manual</a>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_11_how_can_I_run_a_factory_wit"></a> 3.11  how can I run a factory with a special setup? </span></h2>
<p />
If you need to ensure some environment variables are set when running the factory, for example, you can add any custom setup in the <code>/etc/sysconfig/autopyfactory</code> file. It is sourced by the daemon init script <code>/etc/init.d/autopyfactory</code> before start running.  
<p />
<pre class="file">
OPTIONS="--debug --sleep=60 --runas=autopyfactory --log=/var/log/autopyfactory/autopyfactory.log"
CONSOLE_LOG=/var/log/autopyfactory/console.log
env | sort >> $CONSOLE_LOG

source /path/to/my/custom/factory/setup.sh
</pre>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_12_my_factory_is_not_submittin"></a> 3.12  my factory is not submitting pilots anymore for a given queue, why? </span></h2>
<p />
First check that queue has no <code><b>enabled=False</b></code> set in the configuration file <code>/etc/autopyfactory/queues.conf</code>
If that was not the problem, there are several main reasons why this can happens:
<p /> <ul>
<li> One reason is that there are no ready jobs in the WMS system (for example, in PanDA). Standard factory configuration does not submit pilots when there are no jobs.  
</li></ul> 
<p /> <ul>
<li> Another reason may be data corruption from the CE/gatekeeper. Sometimes it happens the CE/gatekeeper reports back to condor a fake number of IDLE and RUNNING pilots. If the number of IDLE pilots returned is high enough (even if that number is not real), the factory most probably will stop submitting and wait for those pending pilot to start running and finish. 
</li></ul> 
<p /> <ul>
<li> The queue in the WMS system is disabled.
</li></ul> 
<p /> <ul>
<li> There is no WMSStatus or BatchStatus information available.
</li></ul> 
<p />
How to check?
<p /> <ul>
<li> for the first case, it is enough to check the AutoPyFactory log files, with a command like this:
</li></ul> 
<p />
<pre class="rootscreen">
[root@factory ~]$ grep ANALY_BNL_SHORT-gridgk04-htcondor /var/log/autopyfactory/autopyfactory.log | grep schedplugin
...
2015-08-10 18:38:20,363 (UTC) [ DEBUG ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] ReadySchedPlugin.py:34 calcSubmitNum(): Starting.
2015-08-10 18:38:20,364 (UTC) [ DEBUG ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] ReadySchedPlugin.py:60 _calc(): pending = 10 running = 17 offset = 0
2015-08-10 18:38:20,364 (UTC) [ INFO ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] ReadySchedPlugin.py:68 _calc(): input=0; activated=83; offset=0 pending=10; running=17; Return=73
2015-08-10 18:38:20,364 (UTC) [ DEBUG ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] ScaleSchedPlugin.py:31 calcSubmitNum(): Starting with n=73
2015-08-10 18:38:20,364 (UTC) [ INFO ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] ScaleSchedPlugin.py:36 calcSubmitNum(): Return=19
2015-08-10 18:38:20,364 (UTC) [ DEBUG ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] MaxPerCycleSchedPlugin.py:23 calcSubmitNum(): Starting with n=19
2015-08-10 18:38:20,365 (UTC) [ INFO ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] MaxPerCycleSchedPlugin.py:35 calcSubmitNum(): input=19; Return=19
2015-08-10 18:38:20,365 (UTC) [ DEBUG ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] MinPerCycleSchedPlugin.py:24 calcSubmitNum(): Starting with n=19
2015-08-10 18:38:20,365 (UTC) [ INFO ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] MinPerCycleSchedPlugin.py:34 calcSubmitNum(): Return=19
2015-08-10 18:38:20,365 (UTC) [ DEBUG ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] StatusTestSchedPlugin.py:26 calcSubmitNum(): Starting.
2015-08-10 18:38:20,366 (UTC) [ DEBUG ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] StatusTestSchedPlugin.py:37 calcSubmitNum(): site status is online
2015-08-10 18:38:20,366 (UTC) [ INFO ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] StatusTestSchedPlugin.py:45 calcSubmitNum(): [Queue is not test] input=19; Return=19
2015-08-10 18:38:20,366 (UTC) [ DEBUG ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] StatusOfflineSchedPlugin.py:29 calcSubmitNum(): Starting.
2015-08-10 18:38:20,366 (UTC) [ DEBUG ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] StatusOfflineSchedPlugin.py:51 calcSubmitNum(): site status is online
2015-08-10 18:38:20,367 (UTC) [ INFO ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] StatusOfflineSchedPlugin.py:66 calcSubmitNum(): [Queue is not offline] input=19; Return=19
2015-08-10 18:38:20,367 (UTC) [ DEBUG ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] MaxPendingSchedPlugin.py:25 calcSubmitNum(): Starting with n=19
2015-08-10 18:38:20,367 (UTC) [ DEBUG ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] MaxPendingSchedPlugin.py:36 calcSubmitNum(): Pending is 10
2015-08-10 18:38:20,367 (UTC) [ INFO ] main.schedplugin[ANALY_BNL_SHORT-gridgk04-htcondor] MaxPendingSchedPlugin.py:53 calcSubmitNum(): Return=0 
</pre>
<p />
Pay attention to the messages coming from the ReadySchedPlugin. Field <strong>Activated</strong> will tell you is there are jobs or not. 
<p /> <ul>
<li> For the second scenario, best way is to run condor_q and see how it says. When possible, print the JobStatus information, or equivalent:
</li></ul> 
<p />
<pre class="rootscreen">
[root@factory ~]$ condor_q  -format '%s.' ClusterId -format '%s ' ProcId -format ' MATCH_APF_QUEUE=%s' match_apf_queue -format ' JobStatus=%d\n' JobStatus 
1230.0 MATCH_APF_QUEUE=BNL_PROD-gridgk01-htcondor JobStatus=1
1231.0 MATCH_APF_QUEUE=BNL_ATLAS_2-gridgk04-htcondor JobStatus=2
1232.0 MATCH_APF_QUEUE=ANALY_BNL_LONG-gridgk01-htcondor JobStatus=2
1233.0 MATCH_APF_QUEUE=BNL_ATLAS_2-gridgk01-htcondor JobStatus=2
1234.0 MATCH_APF_QUEUE=BNL_ATLAS_2-gridgk07-htcondor JobStatus=2
1235.0 MATCH_APF_QUEUE=BNL_PROD_MCORE-gridgk07-htcondor JobStatus=1
</pre>
<p />
Also, when it is installed, try running command apf-queue-status
<p />
<pre class="rootscreen">
[root@factory ~]$ apf-queue-status
Mon Aug 10 15:12:24 2015
-----------------------------------------------------------------
ANALY_BNL_LONG-gridgk04-htcondor 	UNSUB = 0	IDLE = 4	RUNNING = 29	COMPLETE = 0	HELD = 0	ERROR = 0	REMOVED = 0	
ANALY_BNL_LONG-gridgk05      	UNSUB = 0	PENDING = 10	STAGE_IN = 0	ACTIVE = 10	STAGE_OUT = 0	SUSP = 0	DONE = 0	FAILED = 0	
</pre>
<p />
<p /> <ul>
<li> the 3rd case applies mostly to PanDA. Check if the PanDA queue is <strong>offline</strong>. In that case, AutoPyFactory usually does not submit pilots.
</li></ul> 
<p /> <ul>
<li> Besides all of that, you can also check the queue object has actually been created. A grep command like this may help:
</li></ul> 
<p />
<pre class="rootscreen">
[root@factory ~]$ grep "APFQueue: Initializing object..." /var/log/autopyfactory/autopyfactory.log
...
2015-08-10 19:31:51,271 (UTC) [ DEBUG ] main.apfqueue[ANALY_BNL_LONG-gridgk02-htcondor] factory.py:870 __init__(): APFQueue: Initializing object...
2015-08-10 19:31:51,557 (UTC) [ DEBUG ] main.apfqueue[BNL_ATLAS_RCF-gridgk02-htcondor] factory.py:870 __init__(): APFQueue: Initializing object...
2015-08-10 19:31:51,859 (UTC) [ DEBUG ] main.apfqueue[BNL_ATLAS_RCF-gridgk06-htcondor] factory.py:870 __init__(): APFQueue: Initializing object...
...
</pre>
<p /> <ul>
<li> When there is no WMS Status or Batch Status information available, the Sched Plugin Ready returns 0. To check if that is the case, search for a line like this in the logs file:
</li></ul> 
<p />
<pre class="file">
2015-11-30 03:33:14,326 (UTC) [ WARNING ] main.schedplugin[A_QUEUE_AT_A_SOTE] ReadySchedPlugin.py:39 calcSubmitNum(): Missing info. wmsinfo is WMSQueueInfo: notready=0, ready=532,     running=314, done=0, failed=0, unknown=0 batchinfo is None
</pre>
<p />
If the WMSQueueInfo is None, something is broken in the communication with the WMS service (for example, with a PanDA server).
If the batchinfo is None, most probably the condor daemon is not running and command condor_q does not work.
<p />
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_13_I_do_not_want_my_factory_to"></a> 3.13  I do not want my factory to show up in the generic AutoPyFactory web monitor </span></h2>
<p />
Solution currently for this is to remove the config variable <code><b>monitorsection</b></code> from file <code>/etc/autopyfactory/queues.conf</code>
<p />
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_14_how_can_I_increase_the_verb"></a> 3.14  how can I increase the verbosity level in the log files? </span></h2>
<p />
Edit file <code>/etc/sysconfig/autopyfactory</code> and change the loglevel from INFO level to DEBUG level:
<p />
<pre class="file">
OPTIONS="--debug --sleep=60 --runas=autopyfactory --log=/var/log/autopyfactory/autopyfactory.log"
</pre>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_15_How_can_I_change_the_non_ro"></a> 3.15  How can I change the non-root running user? </span></h2>
<p />
The default configuration sets the factory to run as user <strong>autopyfactory</strong>.
That can be changed in file <code>/etc/sysconfig/autopyfactory</code>, via the configuration variable <code><b>--runas</b></code>
<p />
<pre class="file">
OPTIONS="--debug --sleep=60 --runas=autopyfactory --log=/var/log/autopyfactory/autopyfactory.log"
</pre>
<p />
Note that the RPM creates user account <strong>autopyfactory</strong> during the installation process. If you want to run as a different user, you will need to create that account manually.
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_16_How_can_I_force_the_value_o"></a> 3.16  How can I force the value of variable arguments in the condor submit file to be enclosed in between quotes? </span></h2>
<p />
If you want the condor submit file to look like this
<p />
<pre class="file">
arguments = "--wrapperloglevel=debug ...-p 25443 -u user -u managed"
</pre>
<p />
you only need to add the quoutes in the queues configuration file. Typically, like this:
<p />
<pre class="file">
[DEFAULT]
executable.defaultarguments = --wrapperloglevel=debug ...-p 25443 -u user

[ANALY_QUEUE]
executable.arguments = "%(executable.defaultarguments)s -u managed"
</pre>
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_17_I_have_variables_in_the_DEF"></a> 3.17  I have variables in the <code>[DEFAULT]</code> section whose value requires interpolation, and they fail on some sections </span></h2>
<p />
When the <code>[DEFAULT]</code> section in a given configuration file contains variables like this
<p />
<pre class="file">
[DEFAULT]
foo = %(bar)s blah
</pre>
<p />
the variable <code>bar</code> is expected to be defined in every section of that configuration file.
When that is not the case, a traceback like this is printed out:
<p />
<pre class="rootscreen">
    self._interpolate_some(option, L, rawval, section, vars, 1)
  File "/usr/lib64/python2.6/ConfigParser.py", line 646, in _interpolate_some
    option, section, rest, var)
InterpolationMissingOptionError: Bad value substitution:
section: [ANALY_TRIUMF_ARC-ce3]
option : foo
key    : bar
rawval : blah
</pre>
<p />
This will mostly ocurr in the case of queues configuration files, where the variable <code>foo</code> is being resolved in a section where it is not needed. 
The simplest solution is to provide for a fake value for token <code>bar</code> to allow the interpolation to work.
If that is not desired because it is confusing, or there are too many sections where it happens, then a good idea is to split the configuration file into two separate files, each one with the right <code>[DEFAULT]</code> section, and list both of them, split by comma, in the factory configuration file.
<p />
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="3_18_what_do_I_do_when_I_don_t_w"></a> 3.18  what do I do when I don't want to use an X509 proxy? </span></h2>
<p />
To avoid AutoPyFactory from trying to get X509 credentials, set the variable <code>batchsumit.&lt;your_submit_plugin&gt;.proxy</code> to <code>None</code>
<p />
<h1><a name="4_Troubleshooting"></a> 4  Troubleshooting </h1>
<p />
This section contains reported tracebacks and other misbehavior, and their possible explanation.
They mean there is a bug in the code, otherwise, no traceback should appear in the log files. But bugs happen. And, even if they are fixed in the next release, the problem persist for those instances running old versions. 
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="4_1_qcl_dir"></a> 4.1  qcl_dir </span></h2>
<p />
<pre class="file">
2017-03-03 08:29:53,565 (UTC) [ ERROR ] main factory.py:435 run(): Traceback (most recent call last):
  File "/usr/lib/python2.6/site-packages/autopyfactory/factory.py", line 411, in run
    f.run()
  File "/usr/lib/python2.6/site-packages/autopyfactory/factory.py", line 657, in run
    self.reconfig()
  File "/usr/lib/python2.6/site-packages/autopyfactory/factory.py", line 695, in reconfig
    tmpqcl = config_plugin.getConfig()
  File "/usr/lib/python2.6/site-packages/autopyfactory/plugins/factory/config/File.py", line 69, in getConfig
    raise ConfigFailure('Failed to create queues ConfigLoader: %s' %err)
ConfigFailure: Failed to create queues ConfigLoader: local variable 'qcl_dir' referenced before assignment
</pre>
<p />
A traceback related variable <code>qcl_dir</code> usually means the configuration variable <code>queuesDirConf</code> has been set in file <code>autopyfactory.con</code>, but the directory does not actually exists, or it is empty. 
<br>
Most probably, after a fresh deployment, it is set as <code>queuesDirConf = /etc/autopyfactory/queues.d/</code>, but that directory does not exists.
<br>
Note: fixed in version 2.4.10
<p />
<h2 class="twikinetRoundedAttachments"><span class="twikinetHeader"><a name="4_2_condor_q_against_remote_pool"></a> 4.2  condor_q against remote pool </span></h2>
<p />
<pre class="file">
2017-03-10 17:47:20,581 (UTC) [ WARNING ] main.condor condor.py:356 querycondor(): Leaving with bad return code. rc=1 err=Error: Collector has no record of schedd/submitter
</pre>
<p />
This happens due a bug in the code that creates the condor_q command line incorrectly, with a missing white space between arguments.
<br>
Note: fixed in version 2.4.10
<p /> 


</body></html>
