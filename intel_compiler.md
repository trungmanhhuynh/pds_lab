# Installing Educator Intel® Parallel Studio XE Cluster Edition for Linux* with new licenses.

This year (2019), the licenses were renewed, but in different types: named-user instead of floating license. 

With floating license, lic management server software must be set up (in hydra) and other machines
can link to hydra for the license. The name-use licenses are individual for each node, which does 
not require lic manager.

Following are steps I used to install new Intel Studio 2019 with new named-user licenses.
## 1. Uninstall the current intel software by logging to each node and uninstall 
```
>> ssh node2
>> cd /opt/intel/parallel_studio_xe_2017.1.043/
>> sudo ./uninstall.sh 
>> cd /opt
>> sudo rm -r intel
```
