# OpenStack-On-CentOs7

 Single Node OpenStack (Stein) Installation Steps on CentOS 7

In this article we will install latest version of OpenStack ‘Stein’ on CentOS 7 using rdo repositories. As per my setup i am using followings on my CentOS 7 Machine

    Hostname : openstack
    Ip Address : 192.168.1.3


Step 1: Set the Hostname Using hostnamectl command.

# hostnamectl set-hostname openstack
# hostname


 Step 2: Set the Selinux in Permissive Mode

# setenforce 0
# vi /etc/sysconfig/selinux
   SELINUX=permissive 


Step 3: Disable firewalld & NetworkingManager Service
  
# systemctl disable firewalld NetworkManager
# systemctl stop firewalld NetworkManager
# systemctl enable network
# systemctl start network


Step 4: Enable rdo repository & Install OpenStack-Packstack

#  yum install -y https://www.rdoproject.org/repos/rdo-release.rpm
#  yum install -y centos-release-openstack-stein
#  yum update -y

#  yum install -y openstack-packstack  

Step 5: Generate the answer file 

#  packstack --gen-answer-file=answerfile.txt

Step 6:  Edit the answer file, set the following parameters and leave rest of parameter as it is.


# vi answerfile.txt

Edit (i)


CONFIG_PROVISION_DEMO=n

CONFIG_NEUTRON_ML2_TYPE_DRIVERS=flat,vxlan 

CONFIG_NEUTRON_ML2_TENANT_NETWORK_TYPES=vxlan 

CONFIG_NEUTRON_ML2_MECHANISM_DRIVERS=openvswitch 

 CONFIG_NEUTRON_L2_AGENT=openvswitch 

CONFIG_NEUTRON_OVS_EXTERNAL_PHYSNET=extnet 

CONFIG_NEUTRON_OVS_BRIDGE_MAPPINGS=extnet:br-ex 

CONFIG_NEUTRON_OVS_BRIDGE_IFACES=br-ex:ens160  (your interface)
​
CONFIG_KEYSTONE_ADMIN_PW=shivam    (Anything)

CONFIG_HORIZON_SSL=y

Save & Exit  (wq!)



Note : We can configure bridge itself in answerfile. 


 Step 7: Start OpenStack Installation Using answerfile.txt


 # pacstack --answer-file=answerfile.txt




 
After Successfully Installation shown the above output. 

Note : Installation take more time depend on your internet speed. (Max time for Installation is 60 Min)


Step 8: Now you can access the OpenStack Dashboard

When Installation is completed a dashboard link was provided on the screen, in My case it is "https://192.168.1.3/dashboard"






