
# recompile vmware vmmon and vmnet kernel modules; worked up to vmware 16.0.0

cd /root/vmware-modules/vmmon-only                                                                                           
make                                                                                                                         
cp -f vmmon.ko ./../vmmon.o                                                                                                  
cd /root/vmware-modules/vmnet-only                                                                                           
make                                                                                                                         
cp -f vmnet.ko ./../vmnet.o                                                                                                  
mkdir /lib/modules/`uname -r`/misc                                                                                           
                                                                                                                             
cp vmmon.o /lib/modules/`uname -r`/misc/vmmon.ko                                                                             
                                                                                                                             
cp vmnet.o /lib/modules/`uname -r`/misc/vmnet.ko                                                                             
                                                                                                                             
depmod -a                                                                                                                    
                                                                                                                             
/etc/init.d/vmware restart    

#
#  rhel 8.4/vmware 16.0.0
#
#!/bin/bash
VMWARE_VERSION=workstation-`vmware -v|grep [0-9.]* -o|head -n 1`
#VMWARE_VERSION=workstation-15.5.1
TMP_FOLDER=/tmp/patch-vmware
rm -fdr $TMP_FOLDER
mkdir -p $TMP_FOLDER
cd $TMP_FOLDER
#git clone https://github.com/mkubecek/vmware-host-modules.git
git clone -b ${VMWARE_VERSION} https://github.com/mkubecek/vmware-host-modules.git
cd $TMP_FOLDER/vmware-host-modules
#git checkout $VMWARE_VERSION
git fetch
make
sudo make install
sudo mv /usr/lib/vmware/lib/libz.so.1/libz.so.1 /usr/lib/vmware/lib/libz.so.1/libz.so.1.000
sudo ln -s /lib/x86_64-linux-gnu/libz.so.1 /usr/lib/vmware/lib/libz.so.1/libz.so.1
#sudo /etc/init.d/vmware restart 
/etc/init.d/vmware restart 
