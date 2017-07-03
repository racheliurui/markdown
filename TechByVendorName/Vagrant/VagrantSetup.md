install Vagrant
install virtual box
Config vagrant behind the proxy,
http://www.netinstructions.com/running-vagrant-1-8-behind-a-proxy/
After proxy being setted, run to add a box
vagrant box add hashicorp/precise64
Vagrant up to start the vm
================Exception of,
303c.768: Error (rc=-5673):
303c.768: NtAllocateVirtualMemory (0000000000400000 LB 0x1000) failed with rcNt=0xc0000018 allocating replacement memory for working around buggy protection software. See VBoxStartup.log for more details
303c.768: Error (rc=-5645):
303c.768: Too many virtual memory regions.
=================
Trying to use other boxes and changed the default location for .vagrant.d to avoid security limitation (by default under current domain user) by,
set VAGRANT_HOME=somewhereelse
Then, run command to add other boxes (32bit)
http://www.vagrantbox.es/


Vagrant

Workarount the proxy

https://runefs.com/2014/11/28/setting-up-vagrant-behind-a-corporate-proxy/

VERR_VMX_NO_VMX

Vagrant conflict with Hyper-V on windows 10, disable Hyper-V before vagrant up
