
# Developer Guide Lines

## Directory Structure

```
tree command output tbd
```

The deployment of this role follows the style guide from Adfinis Sygroup. [Click Here](https://docs.adfinis-sygroup.ch/public/ansible-guide/styling_guide.html) for more info

## Where/How to make change/additions


### Algorithm


### Important Steps from the documention:

1. Check if SAP Hostagent is running
```
[root@vm31 ~]# /usr/sap/hostctrl/exe/saphostctrl -function Ping ; echo $?
SUCCESS (    3697 usec)
0
```
or
```
[root@vm31 ~]# /usr/sap/hostctrl/exe/saphostctrl -function Ping ; echo $?
FAILED (    2525 usec)
1
```

2. Check for running SID or INSTANCE
+
```
[root@hana1-repl ~]# /usr/sap/hostctrl/exe/saphostctrl -function ListInstances
 Inst Info : RH1 - 10 - hana1-repl - 749, patch 418, changelist 1816226
```


### Full help output from saphostctrl:

```
Usage: saphostctrl [generic option]... -function <Webmethod> [argument]...
       saphostctrl -help [<Webmethod>]

Generic options:
  -host  <hostname>
  -user  <username> <password>
  -https|-sso
  -format [flat|tree|cimobject] where supported.

Supported Webmethods:
  Ping
    Check if SAPHostAgent is running and reacheable calling a WebService interface [-debug]
        [-wait <sec>    wait <sec> seconds for a successful ping]
        [-delay <sec>   wait <sec> seconds between 2 ping call]
  StartInstance
    -sid <Sys ID> -nr <Sys Number> -saplocalhost <hostname> [-timeout <timeout in sec>] [-service] [-prehook] [-posthook] [-errorhook]
  StopInstance
    -sid <Sys ID> -nr <Sys Number> -saplocalhost <hostname> [-timeout <timeout in sec>] [-service] [-cleanup] [-prehook] [-posthook] [-errorhook] [-softtimeout <softtimeout in sec -1 infinite 0 no softshutdown>]
  ListInstances
    [-running (list running instances only) | -stopped (list stopped instances only)]
  CallServiceOperation
    -op startdb|stopdb -sid <Sys ID> [-dbname <DB name>] [-dbusage Abap|Java|Doublestack|LiveCache] [-dbhost <DB hostname>] [-dbtype ada|db6|mss...] [-service] [-timeout <timeout in sec>] [-prehook] [-posthook]
  ACOSPrepare
    -op <operation> argument... [-op <operation> argument...]... [-timeout <timeout in sec>]  [-prehook] [-posthook] [-errorhook]
    supported operations:
      mount|umount|ifup|ifdown
    mount/umount arguments:
      -storage_type netfs|dfs|srid -fsname <file system name> -mntpoint <mount point> -storage_vendor <vendor> (only for dfs and srid) -dfstype <distributed file system type> -srid <storage resource ID> -sridtype <type> [-fsoptions <file system mound options> (only for netfs and dfs)] [-createmountpoint] [-deletemountpoint]
    ifup arguments:
      -iface <interface name> -vhost <virtual hostname> [-nmask <netmask>] [-bcast <broadcast address>]
    ifdown arguments:
      -vhost <virtual hostname>
  GetOperationResults
    -id <OperationID> [-timeout <timeout in sec>]
  CancelOperation
    -id <OperationID> [-timeout <timeout in sec>]
  IsOperationFinished
    -id <OperationID> [-timeout <timeout in sec>]
  ExecuteOperation
    -name <Operation> [-timeout <timeout in sec>] [<key>=<argument>]
  GetCIMObject
    [-namespace <namespace relative to root>] -enuminstances <CIM Class Name> | -getinstance <CIM Class Name> | -enumclass <CIM class name> | -associators <ObjectClass> -assocclass <AssocClass>-properties <PropertyName=PropertyValue&...> | -arguments <ArgumentName=ArgumentValue&...>
  GetComputerSystem
    [-wbem  Get default WBEM ComputerSystem | -sapitsam  Get SAP_ITSAMComputerSystem]
        [-swpackages   Additionally, get information on installed software packages (sapitsam only)
        [-swpatches    Additionally, get information on installed patches (sapitsam only)
        [-topprocesses Additionally, get information on top N CPU utilizing OS processes (sapitsam only, default top 40)
  ListDatabases

  ListDatabaseSystems
    List available Database Systems
  ListDatabaseMetrics
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>] [-dbhostcheck <no|all|strict>] [-dberroronval] [-id <';' separated list of metric ID's>]
  ListDatabaseConfiguration
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>] [-dbhostcheck <no|all|strict>] [-dberroronval] [-id <';' separated list of config ID's>]
  GetDatabaseStatus
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>]
  GetDatabaseSystemStatus
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>] [-dbhostcheck <no|all|strict>]
  StartDatabase
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>] [-timeout <timeout in sec>] [-service] [-instance] [-force]
  StopDatabase
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>] [-timeout <timeout in sec>] [-service] [-instance] [-force]
  AttachDatabase
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbconfdir </path/to/config-dir>] [-dbcopy <Auto|Offline|Online...>] [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>] [-timeout <timeout in sec>] [-service] [-instance] [-force]
  DetachDatabase
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbconfdir </path/to/config-dir>] [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>] [-timeout <timeout in sec>] [-service] [-instance] [-force]
  GetDatabaseProperties
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>]
  SetDatabaseProperty
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbhost <hostname>] [-dbuser <DB admin username>] [-dbpass <DB admin password>] [-dboption <Additional parameter name>=<Additional parameter value>] <DB Property Name>=<DB Property Value>
  LiveDatabaseUpdate
    -dbname <DB name> -dbtype <ada|db6|mss...> [-updatemethod <Extract|Check|Prepare|Undo|Execute|Cleanup>] [-updateoption <SourcePath=</a/path> [-updateoption <TargetPath=/a/path>]...] [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>] [-timeout <timeout in sec>]
  PrepareDatabaseCopy
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbconfdir </path/to/config-dir>] [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>] [-copymethod <Offline|Online>] [-dostatechange] [-timeout <timeout in sec>]
  FinalizeDatabaseCopy
    -dbname <DB name> -dbtype <ada|db6|mss...> [-dbconfdir </path/to/config-dir>] [-dbhost <hostname>] [-dbinstance <instance name>] [-dbuser <DB admin username>] [-dbpass <DB admin password>] [-copymethod <Offline|Online>] [-timeout <timeout in sec>]
  RegisterInstanceService
    -sid <Sys ID> -nr <Sys Number> -saplocalhost <hostname> [-profile <path to start profile>] [-srvuser <username> -srvpass <password> (Windows only)] [-timeout <timeout in sec>]
  UnregisterInstanceService
    -sid <Sys ID> -nr <Sys Number> -saplocalhost <hostname> [-timeout <timeout in sec>]
  ExecuteInstallationProcedure
    -src <Source directory of the SAPInst executable> -prodid <Product ID> [-inifile <SAPInst ini file> | -paramfile <SAPInst parameter file>] [-timeout <timeout in sec>] [-optargs <optional sapinst arguments> ]
        [-cleanup <auto|delayNR> remove all temporary created directories, eventually umount the mounted filesystem>]
        [-retry <retry a precedentely failed execution. -home and -src are mandatory>]
        [-mountsrc <The source mount directoy>]
        [-mounttgt <The local mount directoy. If the directory doesn't exists will be created>]
        [-mounttype <The filesystem type to be used by teh mount command>]
        [-mountopt <Additionally option to be passed to mount: e.g: '-o exec,ro'>]
        [-mountusr <The user which will be used to mount the filesystem>]
        [-mountpwd <The password of the user used by -mountusr>]
        [-umount <umount the mounted filesystem mounted by -mountsrc on -mounttgt>]
        [-umountopt <Additionally option to be passed to umount>]
        [-home <The home directory, where the whole process will be executed>]
        [-trace <SAPInst trace value.>]
        [-relpath <Relative path to the source path (-src or -mounttgt) where the executable is searched.>]
        [-instenvhdl <Preserve | LoadViaShell>    preserve saphostexec environment or (default) reload environment via shell before start SAPInst]
        [-instenv <Comma-separated list of additional environment variables to be set before start SAPInst.>]
  ExecuteUpgradeProcedure
    -src <Source directory of the STARTUP executable> [-timeout <timeout in sec>] [-optargs <optional startup arguments> ]
        [-cleanup <auto|delayNR> remove all temporary created directories, eventually umount the mounted filesystem>]
        [-retry <retry a precedentely failed execution. -home and -src are mandatory>]
        [-mountsrc <The source mount directoy>]
        [-mounttgt <The local mount directoy. If the directory doesn't exists will be created>]
        [-mounttype <The filesystem type to be used by teh mount command>]
        [-mountopt <Additionally option to be passed to mount: e.g: '-o exec,ro'>]
        [-mountusr <The user which will be used to mount the filesystem>]
        [-mountpwd <The password of the user used by -mountusr>]
        [-umount <umount the mounted filesystem mounted by -mountsrc on -mounttgt>]
        [-umountopt <Additionally option to be passed to umount>]
        [-home <The home directory, where the whole process will be executed>]
        [-relpath <Relative path to the source path (-src or -mounttgt) where the executable is searched.>]
  DeployConfiguration
    -target <configuration target (e.g. operation)> -file <configuration file name> [-file <file>...] [-target <target> -file <configuration file name> [-file <file>...]]...
  GetCapabilities

  ListOSMetrics
        [-id <metric id (default all), can be specified multiple times>]
        [-iv <interval in seconds (60 (default), 300, 900, 3600)>]
        [-metype <filter by measured element type, "?*" wildcards can be used (e.g. OperatingSystem, Processor, Disk, FileSystem, NetworkPort (default all))>]
        [-me <filter by measured element, "?*" wildcards can be used (default all)>]
        [-start <start time in hh:mm[:ss], YYYY-MM-DD-hh:mm:ss or CIM datetime format, '0' for current metrics only (default), '-1' complete history>]
        [-end <end time in in hh:mm[:ss], YYYY-MM-DD-hh:mm:ss or CIM datetime format>]
        [-count <get metrics count times, '0' for continuously (default 1)>]
        [-noheader]
        Filter arguments can be combined. Per default same filter types are or'ed, different are and'ed.
        Optionally, -and, -or arguments can be used to explicitly specify the combination operator.
        And'ing has higher precedence than or'ing.
  ListOSProcesses
        [-cmd <filter by command line pattern, "?*" wildcards can be used>]
        [-cmduser <filter by username pattern, "?*" wildcards can be used>]
        [-pid <filter by PID>]
        [-top <get top N CPU time consuming processes, -1 for all (default 40)>]
        [-count <get processes count times, '0' for continuously (default 1)>]
        [-delay <delay between updates in seconds (default 60)>]
        [-start <start time in hh:mm[:ss], YYYY-MM-DD-hh:mm:ss or CIM datetime format, '0' for current processes only (default), '-1' complete history>]
        [-end <end time in in hh:mm[:ss], YYYY-MM-DD-hh:mm:ss or CIM datetime format>]
        [-noheader]
        Filter arguments can be combined. Per default same filter types are or'ed, different are and'ed.
        Optionally, -and, -or arguments can be used to explicitly specify the combination operator.
        And'ing has higher precedence than or'ing.
  GetSAPOSColVersion

  GetSAPOSColHWConf
    -format <tree|flat>
  AddIpAddress
        -addr <ip address or host name> [-family <address family>] -ifName <interface name>
        [-netmask <mask>] [-broadcast <address>] [-alias <if_alias>] [-scope <scope>]
  RemoveIpAddress
        -addr <ip address or host name> [-family <address family>] [-ifName <interface name>]
  GetIpAddressProperties
        -addr <ip address or host name> [-family <address family>] [-ifName <interface name>]
  MoveIpAddress
        -addr <ip address or host name> [-family <address family>] [-ifName <source interface name> ]
        -targetHost <target host> -targetIfName <target interface name>
        [-targetUser <user for target system>] [-targetPass <password for target system>]
        [-connectionToTarget <httpOnly | httpsOnly | httpAsFallback>]
  DetectManagedObjects
        [-manifest <manifest path>] Start the managed objects detection with the optional path to the manifest file
  DeployManagedObjectsFromSAR
        -archive <SAR archive's path> -target <path to target dir>
  ExecuteOutsideDiscovery
         [-sldreg Run SLD registration after Outside Discovery]
         [-computersystem Discover only the Host ComputerSystem]
         [-database Discover only the Database(s) on this host]
         [-smdagent Discover the Solution Manager Diagnostic Instance(s) and Host ComputerSystem]
         [-saphostagent Discover the SAPHostAgent]
         [-msiis Discover only the Microsoft Internet Information Service]
         [-xmlfromconfig Register XML(s) configured with the ConfigureOutsideDiscoveryPath webservice]
         [-verbose Append discovered data to the webservice result as CIMObject]
         [-cleanup Delete the generated sldreg XML input files after the discovery]
         Note: The ExecuteOutsideDiscovery call without parameters will run all of the parameters above except the SLD registration.
  ConfigureOutsideDiscovery
         Configure the Outside Discovery Job which runs periodically
         These Options control the Outside Discovery Job.
         If frequency is not provided, it will run every 12 hours.
         If execution options are not provided the default will be used (detect everything and try to register it).
         -enable
                 [-frequency <X> Run frequency in minutes]
                 [-jobtimeout <X> Wait X seconds for the Outside Discovery Job which was started with the enable option and return the results]

                 Outside Discovery Execution Options:
                 [-sldreg Run SLD registration after Outside Discovery]
                 [-computersystem Discover only the Host ComputerSystem]
                 [-database Discover only the Database(s) on this host]
                 [-smdagent Discover the Solution Manager Diagnostic Instance(s) and Host ComputerSystem]
                 [-saphostagent Discover the SAPHostAgent]
                 [-msiis Discover only the Microsoft Internet Information Service]
                 [-xmlfromconfig Register XML(s) configured with the ConfigureOutsideDiscoveryPath webservice]
                 [-verbose Append discovered data to the webservice result as CIMObject]
                 [-cleanup Delete the generated sldreg XML input files after the discovery]
         -disable
         -auto detect Outside Discovery settings automatically

         Outside Discovery Destinations:
         -sldhost
         -sldport
         -sldusername
         -sldpassword
         -sldusessl
  ConfigureOutsideDiscoveryPath
         Configure a file or directory to OutsideDiscovery configuration for sld registration.
         Only XML file will be taken into consideration.
         -register <file/directory> Add a file to OutsideDiscovery configuration for sld registration
         -unregister <file/directory> Remove a file from OutsideDiscovery configuration for sld registration
  ReloadConfiguration

  EnableCORS
         -name <service codebase, e.g. slplugin>
         -url <url[,url]> [-url <url[,url]>...]
  DisableCORS
         -name <service codebase, e.g. slplugin>
         -url <url[,url]> [-url <url[,url]>...]

```
