Questions and Answers related AutoPyFactory
===========================================

1 About this Document
=====================

This document is just a somehow free-format list of questions asked
about [AutoPyFactory](AutoPyFactory.html), and some of the answers
provided. The answer may be straight forward, or may be just some hints
about where to look.

It does not include the complete list of questions, as this documented
was started on August 10th, 2015. It is not a FAQ page, therefore.

Conventions used in this document:

A *User Command Line* is illustrated by a green box that displays a
prompt:

``` {.screen}
  [user@client ~]$
```

A *Root Command Line* is illustrated by a red box that displays the
*root* prompt:

``` {.rootscreen}
  [root@factory ~]$
```

*Lines in a file* are illustrated by a yellow box that displays the
desired lines in a file:

``` {.file}
priorities=1
```

2 Applicable versions
=====================

This document mostly applies to the version of AutoPyFactory 2.4.5 and
higher. Some answer may be valid for older versions too, but that is not
guaranteed.

3 Questions and answers
=======================

3.1 how do I interpret comments like "fixed in version X" at the same time that "This documentation applies to the latest version of APF: Y"?
---------------------------------------------------------------------------------------------------------------------------------------------

When the versions X and Y are different, usually that means that a bug
was found in the version Y -the one the current documentation is for-,
and a fix for that bug will be released in a future version -X or
higher-, but not yet. \
 It can also mean that we forgot to update the message "This
documentation applies to the latest version of APF... "
![wink](https://twiki.grid.iu.edu/twiki/pub/TWiki/SmiliesPlugin/wink.gif "wink")

3.2 what do I do if I want to change the path of the log files?
---------------------------------------------------------------

Some explanation can be found
[here](AutoPyFactoryConfiguration.html#5_2_logrotation)

3.3 how can I submit to a remote HTCondor schedd?
-------------------------------------------------

It is not needed the APF host also be an HTCondor schedd. Remote
submission is allowed. The queryargs -for the BatchStatus Plugin- and
the submitargs -for the Submit Plugin- can be used to pass any arbitrary
options, for example to refer to a remote schedd.

``` {.file}
    batchsubmitplugin = CondorLocal
    batchsubmit.condorlocal.submitargs = -remote <remote_schedd>:<port>
 
    batchstatusplugin = Condor
    batchstatus.condor.queryargs = -pool <remote_pool> -name <remote_schedd>
```

3.4 why pilots submitted to a CREAM CE fail?
--------------------------------------------

If your pilots being submitted to a CREAM CE fail, with this type of
content in the condor log file:

``` {.file}
        009 (45454545.000.000) 12/10 20:35:01 Job was aborted by the user.
            CREAM error: BLAH error: submission command failed (exit code = 1) (stdout:) (stderr:) N/A (jobId = CREAM123456789)
```

it is possible that the grid\_resource line in the condor submit file is
not correct. AutoPyFactory creates a line like this

``` {.file}
grid_resource = cream matrix.net:8443/ce-cream/services/CREAM2 pbs queuename
```

while perhaps the CE needs a line like

``` {.file}
grid_resource = cream matrix.net:8443/queuename
```

If that is the case, or similar, there is dirty trick to fix it. Add
this line to your queue configuration (typically in queues.conf):

``` {.file}
batchsubmit.condorcream.condor_attributes.grid_resource = cream matrix.net:8443/queuename
```

3.5 to which type of resource targets I can submit pilots with AutoPyFactory?
-----------------------------------------------------------------------------

In principle, to those supported by HTCondor. That includes globus GT2,
globus GT5, HTCondor-CE, CREAM CE, NorduGrid CE, Amazon EC2, a local or
remote HTCondor cluster,... If HTCondor cannot submit to a given
resource, then AutoPyFactory cannot either.

3.6 how do I find out the version of AutoPyFactory I have installed?
--------------------------------------------------------------------

A rpm command will give the answer:

``` {.rootscreen}
[root@factory ~]$ rpm -qa | grep autopyfactory
autopyfactory-tools-1.0.1-1.noarch
autopyfactory-2.4.1-1.noarch
```

3.7 how should I configure the factory if I want to renew the proxies myself?
-----------------------------------------------------------------------------

If you do not want the factory to renew the x509 proxies for you because
you have in place a different mechanism for that, you only need to
disable the renewal feature. But, if the factory is configured to submit
to grid sites or any other resource that requires an x509 file, the
proxymanager still must be enabled anyways. So, in your
`autopyfactory.conf` file:

``` {.file}
proxymanager.enabled = True
```

but in your `proxy.conf` file:

``` {.file}
renew = False
```

3.8 how can I add environment variables to the condor submit file?
------------------------------------------------------------------

Using the configuration variable `batchsubmit.<plugin>.environ`. For
details, check the [Reference
Manual](AutoPyFactoryReferenceManual.html#6_5_Batch_Submit_Plugin_variable)

3.9 how can I add any arbitrary line to the condor submit file?
---------------------------------------------------------------

Using the configuration variable
`batchsubmit.<plugin>.condor_attributes`. For details, check the
[Reference
Manual](AutoPyFactoryReferenceManual.html#6_5_Batch_Submit_Plugin_variable)

3.10 how can I tune the number of pilots to be submitted depending on the number of activated jobs in PanDA and the number of pilots still pending?
---------------------------------------------------------------------------------------------------------------------------------------------------

The sched plugin `Ready` checks the number of activated jobs, the number
of idle pilots, and returns the difference. For details on the sched
plugins parameters, check the [Reference
Manual](AutoPyFactoryReferenceManual.html#6_4_Sched_Plugin_variables)

3.11 how can I run a factory with a special setup?
--------------------------------------------------

If you need to ensure some environment variables are set when running
the factory, for example, you can add any custom setup in the
`/etc/sysconfig/autopyfactory` file. It is sourced by the daemon init
script `/etc/init.d/autopyfactory` before start running.

``` {.file}
OPTIONS="--debug --sleep=60 --runas=autopyfactory --log=/var/log/autopyfactory/autopyfactory.log"
CONSOLE_LOG=/var/log/autopyfactory/console.log
env | sort >> $CONSOLE_LOG

source /path/to/my/custom/factory/setup.sh
```

3.12 my factory is not submitting pilots anymore for a given queue, why?
------------------------------------------------------------------------

First check that queue has no `enabled=False` set in the configuration
file `/etc/autopyfactory/queues.conf` If that was not the problem, there
are several main reasons why this can happens:

-   One reason is that there are no ready jobs in the WMS system (for
    example, in PanDA). Standard factory configuration does not submit
    pilots when there are no jobs.

-   Another reason may be data corruption from the CE/gatekeeper.
    Sometimes it happens the CE/gatekeeper reports back to condor a fake
    number of IDLE and RUNNING pilots. If the number of IDLE pilots
    returned is high enough (even if that number is not real), the
    factory most probably will stop submitting and wait for those
    pending pilot to start running and finish.

-   The queue in the WMS system is disabled.

-   There is no WMSStatus or BatchStatus information available.

How to check?

-   for the first case, it is enough to check the AutoPyFactory log
    files, with a command like this:

``` {.rootscreen}
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
```

Pay attention to the messages coming from the ReadySchedPlugin. Field
**Activated** will tell you is there are jobs or not.

-   For the second scenario, best way is to run condor\_q and see how it
    says. When possible, print the JobStatus information, or equivalent:

``` {.rootscreen}
[root@factory ~]$ condor_q  -format '%s.' ClusterId -format '%s ' ProcId -format ' MATCH_APF_QUEUE=%s' match_apf_queue -format ' JobStatus=%d\n' JobStatus 
1230.0 MATCH_APF_QUEUE=BNL_PROD-gridgk01-htcondor JobStatus=1
1231.0 MATCH_APF_QUEUE=BNL_ATLAS_2-gridgk04-htcondor JobStatus=2
1232.0 MATCH_APF_QUEUE=ANALY_BNL_LONG-gridgk01-htcondor JobStatus=2
1233.0 MATCH_APF_QUEUE=BNL_ATLAS_2-gridgk01-htcondor JobStatus=2
1234.0 MATCH_APF_QUEUE=BNL_ATLAS_2-gridgk07-htcondor JobStatus=2
1235.0 MATCH_APF_QUEUE=BNL_PROD_MCORE-gridgk07-htcondor JobStatus=1
```

Also, when it is installed, try running command apf-queue-status

``` {.rootscreen}
[root@factory ~]$ apf-queue-status
Mon Aug 10 15:12:24 2015
-----------------------------------------------------------------
ANALY_BNL_LONG-gridgk04-htcondor    UNSUB = 0   IDLE = 4    RUNNING = 29    COMPLETE = 0    HELD = 0    ERROR = 0   REMOVED = 0 
ANALY_BNL_LONG-gridgk05         UNSUB = 0   PENDING = 10    STAGE_IN = 0    ACTIVE = 10 STAGE_OUT = 0   SUSP = 0    DONE = 0    FAILED = 0  
```

-   the 3rd case applies mostly to PanDA. Check if the PanDA queue is
    **offline**. In that case, AutoPyFactory usually does not submit
    pilots.

-   Besides all of that, you can also check the queue object has
    actually been created. A grep command like this may help:

``` {.rootscreen}
[root@factory ~]$ grep "APFQueue: Initializing object..." /var/log/autopyfactory/autopyfactory.log
...
2015-08-10 19:31:51,271 (UTC) [ DEBUG ] main.apfqueue[ANALY_BNL_LONG-gridgk02-htcondor] factory.py:870 __init__(): APFQueue: Initializing object...
2015-08-10 19:31:51,557 (UTC) [ DEBUG ] main.apfqueue[BNL_ATLAS_RCF-gridgk02-htcondor] factory.py:870 __init__(): APFQueue: Initializing object...
2015-08-10 19:31:51,859 (UTC) [ DEBUG ] main.apfqueue[BNL_ATLAS_RCF-gridgk06-htcondor] factory.py:870 __init__(): APFQueue: Initializing object...
...
```

-   When there is no WMS Status or Batch Status information available,
    the Sched Plugin Ready returns 0. To check if that is the case,
    search for a line like this in the logs file:

``` {.file}
2015-11-30 03:33:14,326 (UTC) [ WARNING ] main.schedplugin[A_QUEUE_AT_A_SOTE] ReadySchedPlugin.py:39 calcSubmitNum(): Missing info. wmsinfo is WMSQueueInfo: notready=0, ready=532,     running=314, done=0, failed=0, unknown=0 batchinfo is None
```

If the WMSQueueInfo is None, something is broken in the communication
with the WMS service (for example, with a PanDA server). If the
batchinfo is None, most probably the condor daemon is not running and
command condor\_q does not work.

3.13 I do not want my factory to show up in the generic AutoPyFactory web monitor
---------------------------------------------------------------------------------

Solution currently for this is to remove the config variable
`monitorsection` from file `/etc/autopyfactory/queues.conf`

3.14 how can I increase the verbosity level in the log files?
-------------------------------------------------------------

Edit file `/etc/sysconfig/autopyfactory` and change the loglevel from
INFO level to DEBUG level:

``` {.file}
OPTIONS="--debug --sleep=60 --runas=autopyfactory --log=/var/log/autopyfactory/autopyfactory.log"
```

3.15 How can I change the non-root running user?
------------------------------------------------

The default configuration sets the factory to run as user
**autopyfactory**. That can be changed in file
`/etc/sysconfig/autopyfactory`, via the configuration variable `--runas`

``` {.file}
OPTIONS="--debug --sleep=60 --runas=autopyfactory --log=/var/log/autopyfactory/autopyfactory.log"
```

Note that the RPM creates user account **autopyfactory** during the
installation process. If you want to run as a different user, you will
need to create that account manually.

3.16 How can I force the value of variable arguments in the condor submit file to be enclosed in between quotes?
----------------------------------------------------------------------------------------------------------------

If you want the condor submit file to look like this

``` {.file}
arguments = "--wrapperloglevel=debug ...-p 25443 -u user -u managed"
```

you only need to add the quoutes in the queues configuration file.
Typically, like this:

``` {.file}
[DEFAULT]
executable.defaultarguments = --wrapperloglevel=debug ...-p 25443 -u user

[ANALY_QUEUE]
executable.arguments = "%(executable.defaultarguments)s -u managed"
```

3.17 I have variables in the `[DEFAULT]` section whose value requires interpolation, and they fail on some sections
-------------------------------------------------------------------------------------------------------------------

When the `[DEFAULT]` section in a given configuration file contains
variables like this

``` {.file}
[DEFAULT]
foo = %(bar)s blah
```

the variable `bar` is expected to be defined in every section of that
configuration file. When that is not the case, a traceback like this is
printed out:

``` {.rootscreen}
    self._interpolate_some(option, L, rawval, section, vars, 1)
  File "/usr/lib64/python2.6/ConfigParser.py", line 646, in _interpolate_some
    option, section, rest, var)
InterpolationMissingOptionError: Bad value substitution:
section: [ANALY_TRIUMF_ARC-ce3]
option : foo
key    : bar
rawval : blah
```

This will mostly ocurr in the case of queues configuration files, where
the variable `foo` is being resolved in a section where it is not
needed. The simplest solution is to provide for a fake value for token
`bar` to allow the interpolation to work. If that is not desired because
it is confusing, or there are too many sections where it happens, then a
good idea is to split the configuration file into two separate files,
each one with the right `[DEFAULT]` section, and list both of them,
split by comma, in the factory configuration file.

3.18 what do I do when I do not want to use an X509 proxy?
---------------------------------------------------------

To avoid AutoPyFactory from trying to get X509 credentials, set the
variable `batchsumit.<your_submit_plugin>.proxy` to `None`

4 Troubleshooting
=================

This section contains reported tracebacks and other misbehavior, and
their possible explanation. They mean there is a bug in the code,
otherwise, no traceback should appear in the log files. But bugs happen.
And, even if they are fixed in the next release, the problem persist for
those instances running old versions.

4.1 qcl\_dir
------------

``` {.file}
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
```

A traceback related variable `qcl_dir` usually means the configuration
variable `queuesDirConf` has been set in file `autopyfactory.con`, but
the directory does not actually exists, or it is empty. \
 Most probably, after a fresh deployment, it is set as
`queuesDirConf = /etc/autopyfactory/queues.d/`, but that directory does
not exists. \
 Note: fixed in version 2.4.10

4.2 condor\_q against remote pool
---------------------------------

``` {.file}
2017-03-10 17:47:20,581 (UTC) [ WARNING ] main.condor condor.py:356 querycondor(): Leaving with bad return code. rc=1 err=Error: Collector has no record of schedd/submitter
```

This happens due a bug in the code that creates the condor\_q command
line incorrectly, with a missing white space between arguments. \
 Note: fixed in version 2.4.10


