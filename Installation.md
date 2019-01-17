## Installation and Setup

### Linux Setup Instructions

Pre-Install
  *  # subscription-manager repos --enable rhel-7-server-devtools-rpms
  * # subscription-manager repos --enable rhel-server-rhscl-7-rpms
  * # cd /etc/pki/rpm-gpg
  * # wget -O RPM-GPG-KEY-redhat-devel https://www.redhat.com/security/data/a5787476.txt
  * # rpm --import RPM-GPG-KEY-redhat-devel
  * # yum -y groupinstall "Virtualization Host"

Install
  * # yum install cdk-minishift docker-machine-kvm
  * $ minishift setup-cdk --force --default-vm-driver="kvm"

Link to online doc

### Windows Setup Instructions
1. Sign up for an account at developers.redhat.com
2. Enable Hyper-V or install VirtualBox
3. Download Container Development Kit (CDK)
4. Config hypervisor
    * For Hyper-V: C:\> setx HYPERV_VIRTUAL_SWITCH “External (Wireless)”
    * For VirtualBox: C:\> minishift config set vm-driver virtualbox
5. Run C:\> minishift setup-cdk
6. Start MiniShift via C:\> minishift start
7. Add dir with oc.exe to PATH
    * C:\> @FOR /f "tokens=*" %i IN ('minishift oc-env') DO @call %i

Link to online doc
