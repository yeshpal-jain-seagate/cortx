#  Build Installation - FAQs

    This document lists the errors and the procedures that must be followed to resolve them.
Error
In cortx – s3, while installing init.sh, the below mentioned error occurs.
	 
http://ssc-satellite1.colo.seagate.com/pulp/repos/EOS/Production/CentOS-7_7_1908/custom/EPEL-7/EPEL-7/repodata/repomd.xml: [Errno 14] curl#6 - "Could not resolve host: ssc-satellite1.colo.seagate.com; Unknown error"
Resolution
To address the above mentioned error, perform the following:
1.	Navigate to the cloned repository. 
cd /root/cortx- s3server/ansible/files/yum.repos.d/centos7.7.1908
2.	Ensure that the following is entered:
[root@localhost centos7.7.1908]# ls -lrt total 12
-rw-r--r-- 1 root root 178 Jul  8 21:25 epel7.repo
-rw-r--r-- 1 root root 216 Jul  8 21:30 eos_s3_deps.repo
-rw-r--r-- 1 root root 206 Jul  8 21:30 eos.repo
3.	Add relevant comments against entries made in the previous step.
[root@localhost centos7.7.1908]# cat *.repo
#[releases_eos]
#baseurl = http://ci-storage.mero.colo.seagate.com/releases/eos/github/master/rhel-7.7.1908/last_successful/
#gpgcheck = 0
#name = Yum repo for eos builds - OS Centos 7.7.1908
#priority = 1
#[releases_eos_s3deps]
#baseurl = http://ci-storage.mero.colo.seagate.com/releases/eos/s3server_uploads/centos7/
#gpgcheck = 0
#name = Yum repo for s3 dependencies rpms built for eos - OS Centos/rhel 7
#priority = 1
#[epel7]
#baseurl = http://ssc-satellite1.colo.seagate.com/pulp/repos/EOS/Production/CentOS-7_7_1908/custom/EPEL-7/EPEL-7/
#gpgcheck = 0
#name = Yum repo for epel7
#priority = 1
 
Error
The lctl list_nids command does not show any output.
Resolution
To address the above mentioned error, perform the following:
1.	Check the following entry:
•	cat /etc/modprobe.d/lnet.conf
            options lnet networks=tcp(ens160) config_on_load=1
2.	If any change is made in the previous step, the following is applicable:
•	systemctl restart lnet
•	systemctl status lnet
3.	lctl list_nids (the output must be <ip address of VM>@tcp )
Error
No package found for log4cxx_cortx.
Resolution
To resolve the above error, navigate to https://github.com/Seagate/cortx/blob/master/doc/CortxS3ServerQuickStart.md. Then, refer the following section.
•	Create a local repository
Error
rmmod: ERROR: Module m0tr is in use or rmmod: ERROR: Module galois is in use by: m0tr or
insmod: ERROR: could not insert module /root/cortx-motr/extra-libs/galois/src/linux_kernel/galois.ko: File exists Error unloading /root/cortx-motr/m0tr.ko
Resolution
This error occurs when the previous runs are ended abruptly or forcefully (ctrl+c). Reboot the system or perform the following:    
          
1.	List the modules as follows:
[root@localhost ~]# lsmod
                       Module                  Size  Used by
         m0ctl                  48916  0
         m0tr                13901353  1 m0ctl
         galois                 22944  1 m0tr
         ksocklnd              183608  0
         lnet                  586401  3 m0tr,ksocklnd
         libcfs                415252  2 lnet,ksocklnd
         loop                   28072  12
Note: The output displays the list of modules along with m0ctl, m0tr, and galios.
2.	Remove the modules in the following order:
o	[root@localhost ~]# rmmod m0ctl
o	[root@localhost ~]# rmmod m0tr
o	[root@localhost ~]# rmmod galois
