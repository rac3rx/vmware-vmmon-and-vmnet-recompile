
# recompile vmware vmmon and vmnet kernel modules

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
~                                
