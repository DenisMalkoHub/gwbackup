# gwbackup version 2.0 (gwbackupNomain)
 
bash script for OS Vyatta.  It is used for testing internet connection and reservation of two ISP. For reservetion we use VRRP group priority.

####Install

***On first router*** 
``` 
mkdir /config/scripts/gwbackupNomain
cd /config/scripts/gwbackupNomain

```

copy files 
gwbackupNomain, chooseGW, rt1up, rt2up, rt1down, rt2down, bothDown to new directory

For autostart make link
```
ln -s /config/scripts/gwbackupNomain/gwbackup /config/scripts/postconfig.d/gwbackupNomain

```


chooseGW -  bash script for OS Vyatta.   It is used for change VRRP Master status using priority.

For single word command in CLI like chooseGW make link to /bin

```
ln -s /config/scripts/gwbackupNomain/chooseGW /bin/chooseGW

```

***On second router***
 copy
rt2vrrpset
and run it 
```
cd /config/scripts
scp name@yourserver:/your/parth/rt2vrrpset .
chmod a+x rt2vrrpset
ln -s /config/scripts/rt2vrrpset /config/scripts/postconfig.d/rt2vrrpset
./rt2vrrpset
```
# How to use

***You can change parameters on your own***

Parameters affecting the notification by mail
```
ISP1="ISP1" # ISP1 name
ISP2="ISP2" # ISP2 name

RECIPIENTS=" testmail1@gmail.com testmail2@gmail.com " # Use a space to separate values
```
IPs monitoring via primary GW
```
GW1IP1="8.8.8.8"
GW1IP2="208.67.222.222"
```
IPs monitored via backup GW (static routes via secondary GW need to be configured for them)
```
GW2IP1="8.8.4.4"
GW2IP2="208.67.222.123"
```
Settings that affect the speed of response to the loss of the connection and counteraction to the flap

```

PingCount="5"       # How much ping on every monitoring ip
GW1OKCOUNT="3"      # ISP1 down if count less then this parametr
GW2OKCOUNT="3"      # ISP2 down if count less then this parametr
SLEEP_TIME="15"     # Wait seconds after every cycle of ping ????
CYCLES="10"         # How much cycles wait when two ISP change state to UP 
                    # to ???? in normal state and start load balancing 
                    # Try not to set a very small value to avoid flapping
```

**For start using script**
```
bash /config/script/gwbackupNomain/gwbackupNomain &
```

**For stoping script**
Need to test!!!!!!!!!!!!!!!!!!
```
ps aux | grep -ie gwbackupNomain | awk '{print "kill -9 " $2}'

```
###For choose VRRP master for vlan use 
```
usr@rt1:/ chooseGW
```
At first
 
Choose master router (1 or 2 or rt1 or rt2) or press Enter to exit
```
1
```

And now choose VLAN(s) for this Master. Press Enter for finish choosing VLANs.
```
11
```
press enter

Choose another VLAN or press "ENTER" to exit.
```
Do you want use gateway RT1 for VLAN (1 11 23)? (y/n)
```
press yes or no

Choose another router or press "ENTER" for exit.

****This setting will affect on next cycle of gwbackup in load balance state!**** 
