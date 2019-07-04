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

## 2. Installing new software. 
To download the software, we need to sign-in to intel accounts. 
Follow the installation instructions to install. Note that, I could not use cluster-type installation (there was error in /tmp not enough space). If i change /tmp to myhome/tmp (using -t flag) then the instlation failed, because the /home is shared across node.

Finally, I using silent mode for each node. We meed to modify silent.cfg to the right activation type.
```
ssh node18 /home/manhh/install/parallel_studio_xe_2019_update4_cluster_edition/install.sh --silent=/home/manhh/install/parallel_studio_xe_2019_update4_cluster_edition/silent.cfg
```
Update PATH enviroment, I placed following command in /etc/profile.d/intel.sh 
```
source /opt/intel/bin/compilervars.sh -arch intel64 -platform linux
```
Because the current license we have is named-user or single-user license, I am currently set all nodes in
Heracles the same license number (one in 18 lics that we regitered.)
We need to make sure it works or if we need unique lic files for each node. 


## 3. Install new intel software for Xeon. 
---> will be installed

