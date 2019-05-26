# Configuring pam_slurm_adopt.so

This note presents how to configure pam_slurm_adopt to prevent a user access to a compute node
of Heracles. The instructions below will only allow users in wheel to have access to compute nodes (2->17).


```
[test1@heracles ~]$ ssh node17
Access denied by pam_slurm_adopt: you have no active jobs on this node
Connection closed by 10.0.0.17
```

**1. Add two following lines into file /etc/pam.d/sshd**
```
account    sufficient   pam_access.so 
account    required     pam_slurm_adopt.so  
```
The resulted file is: 
```
#%PAM-1.0
#auth       sufficient   pam_ldap.so
auth       required     pam_sepermit.so
auth       substack     password-auth
auth       include      postlogin
# Used with polkit to reauthorize users in remote sessions
-auth      optional     pam_reauthorize.so prepare
account    required     pam_nologin.so
account    include      password-auth
account    sufficient   pam_access.so #added by Manh
account    required     pam_slurm_adopt.so  #added by Manh
password   include      password-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open env_params
session    required     pam_namespace.so
session    optional     pam_keyinit.so force revoke
session    include      password-auth
session    include      postlogin
# Used with polkit to reauthorize users in remote sessions
-session   optional     pam_reauthorize.so prepare
```

**2. Comment out following two lines in /etc/pam.d/password-auth**
```
account     sufficient    pam_localuser.so
-session     optional      pam_systemd.so
```
The result file is: 
```
#%PAM-1.0
# This file is auto-generated.
# User changes will be destroyed the next time authconfig is run.
auth        required      pam_env.so
auth        sufficient    pam_unix.so nullok try_first_pass
auth        requisite     pam_succeed_if.so uid >= 1000 quiet_success
auth        sufficient    pam_ldap.so use_first_pass
auth        required      pam_deny.so

account     required      pam_unix.so
#account     sufficient    pam_localuser.so
account     sufficient    pam_succeed_if.so uid < 1000 quiet
account     [default=bad success=ok user_unknown=ignore] pam_ldap.so
account     required      pam_permit.so

password    requisite     pam_pwquality.so try_first_pass local_users_only retry=3 authtok_type=
password    sufficient    pam_unix.so sha512 shadow nullok try_first_pass use_authtok
password    sufficient    pam_ldap.so use_authtok
password    required      pam_deny.so

session     optional      pam_keyinit.so revoke
session     required      pam_limits.so
#-session     optional      pam_systemd.so
session     [success=1 default=ignore] pam_succeed_if.so service in crond quiet use_uid
session     required      pam_unix.so
```

**3. Add following two lines into /etc/securities/access.conf**
```
+:wheel:ALL
-:ALL:ALL
```

## Auto scripts. 



<h2> References:  <h2> 
