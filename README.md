# DEVNET-1882: Exploring YANG Data Modeling: A Guided Session using NSO

NSO Playground Blog
https://blogs.cisco.com/developer/nsoplayground01

Playground local install:
https://developer.cisco.com/codeexchange/github/repo/CiscoDevNet/NSO-Playground-Local-Install/


Session Type
DevNet

DevNet Session Type
DevNet

Length*
45 Min

Technical Level*
Introductory

Struggling to wrap your head around YANG? In this session, we’ll simplify the core concepts of YANG and demonstrate its application through Cisco Network Services Orchestrator (NSO). Whether you're preparing for certifications or working with automation, this session will help you navigate YANG with ease and confidence. By attending this session, you'll:

Master the Essentials of YANG: Learn the foundational principles of YANG through a guided demonstration in NSO, designed to reinforce key concepts with practical, real-world examples.

Boost Your Skillset for Network Automation: Gain critical YANG knowledge that will elevate your ability to work with network automation, improving your practical expertise and certification readiness.

Cultivate a Strong Learning Foundation: Discover how experimenting with YANG in the NSO environment sets the stage for mastering more advanced automation techniques.

Join us for a session that offers a clear understanding of YANG, culminating in a live demonstration that will boost your network automation skills.

## NSO YANG Courses

https://developer.cisco.com/learning/labs/yang-nso-101/
https://developer.cisco.com/learning/tracks/get_started_with_nso/nso-yang/yang-nso-201/creating-a-new-model/

## Steps for this lab

steps:

```
source $NCS_DIR/ncsrc
ncs-setup --dest ~/nso-instance 
```

Move over the packages using git clone to the home directory and then soft link them to packages directory:

```
git clone https://github.com/aaymahaj/DEVNET-1882

cd ~/nso-instance/packages

# Move the directories from DEVNET-1882 to the packages directory
mv ~/DEVNET-1882/user-info ~/nso-instance/packages/
mv ~/DEVNET-1882/router-model ~/nso-instance/packages/

```

re-compile the packages 

```
cd ~/nso-instance/packages/user-info/src
make clean all
cd ~/nso-instance/packages/router-model/src
make clean all
```


```
cd ~/nso-instance
ncs
ncs_cli -C -u admin
```


```
packages reload force
exit
```


```
echo $DEVENV_APP_8080_URL
# put that link in your browser
# default credentials for the Web UI are: admin/admin
```

Configuring NSO CLI example
```
source $NCS_DIR/ncsrc
ncs_cli -C -u admin
show running-config router
show running-config router | display json
show running-config router | display xml
```

New Configuration
```
admin@ncs# config
Entering configuration mode terminal
admin@ncs(config)# router name re-demo-CLUS address 4.5.6.7 ?
Possible completions:
  interface  operational-status  <cr>
admin@ncs(config)# router name re-demo-CLUS address 4.5.6.7 operational-status up interface ?
Possible completions:
  GigabitEthernet  TenGigE
admin@ncs(config)# router name re-demo-CLUS address 4.5.6.7 operational-status up interface TenGigE ?
Possible completions:
  disabled
  enabled
  interface-id   X.X or X/X
  <cr>
admin@ncs(config)# router name re-demo-CLUS address 4.5.6.7 operational-status up interface TenGigE interface-id 1.4 ?
Possible completions:
  disabled  enabled  <cr>
admin@ncs(config)# router name re-demo-CLUS address 4.5.6.7 operational-status up interface TenGigE interface-id 1.4 disabled
admin@ncs(config-interface-TenGigE)# commit
Commit complete.
admin@ncs(config)# show full-configuration router
router name        re-demo-CLUS
router address     4.5.6.7
router operational-status up
router interface TenGigE
 disabled
 interface-id 1.4
!
```

changing configuration that is already existing example

```
Entering configuration mode terminal
admin@ncs(config)# router ?
Possible completions:
  address  interface  name  operational-status
admin@ncs(config)# router name ?
Possible completions:
  <string>[re-demo-CLUS]
admin@ncs(config)# router name re-123 ?
Possible completions:
  address  interface  operational-status  <cr>
admin@ncs(config)# router name re-123 address 4.5.6.7
admin@ncs(config)# commit dry-run
cli {
    local-node {
        data  router {
             -    name re-demo-CLUS;
             +    name re-123;
              }
    }
}
admin@ncs(config)# commit
Commit complete.
```