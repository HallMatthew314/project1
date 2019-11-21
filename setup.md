# Setup

## Virtual Networks

 1. Open the VMWare workstation network editor. (Edit -> Virtual Network Editor)
 
 2. Check the settings for VMnet1 and VMnet8.
 
 2.1. VMnet1 should be Host-only and have a subnet address of `192.168.107.0/24`
 2.2. VMnet8 should be NAT and have a subnet address of `192.168.58.0/24`
 
    If any of the setting don't match, click the `Change Settings` button and correct them.

## Target Downloads

 1. Download one or more of the target images from vulnhub, with any other targets you like:

   - https://www.vulnhub.com/entry/symfonos-1,322/
   - https://www.vulnhub.com/entry/tempus-fugit-2,364/
   - https://www.vulnhub.com/entry/jigsaw-2,337/
   - https://www.vulnhub.com/entry/metasploitable-2,29/
   
 2. Install each image in VMware Workstation
 
 3. Set the network adapter to NAT for all target machines.

## Kali

  1. Install the Kali image in VMWare Workstation.
     The credentials are `root`, `toor`
  
  2. Set the network adapter to `Custom: VMnet1 (Host-only)`

## Router
  
  1. Install the router image in VMWare Workstation.
     The credentials are `router`, `router`.

  2. Set the network adapter to NAT.
  
  3. Create another network adapter for the router and assign it to VMnet1.
    3.1. Virtual Machine Settings
	3.2. Hardware->Add
	3.3. Choose 'Network Adapter' and then 'Finish'
	3.4. Set the new adapter to `Custom: VMnet1 (Host-only)`

## Testing

  1. Start the router and login to the Kali machine.
  
  2. Run the following ping commands on the Kali machine:
  
  2.1. `ping -c 4 192.168.107.10` - Ping the Kali machine's gateway.
  2.2. `ping -c 4 192.168.58.10` - Ping the gateway's other interface.
  2.3. `ping -c 4 192.168.58.2` - Ping an external host (VMware NAT gateway).
  
  3. Possible problems / solutions:
  
  3.1. If the first ping failed:

  3.1.1. Check `/etc/network/interfaces` on the Kali machine and the router.
  3.1.2. Make sure the Kali machine is in the `192.168.107.0/24` network.
  3.1.3. Make sure the router's `ens37` interface is in the `192.168.107.0/24` network.
  3.1.4. Make sure the router's `ens37` interface has an IP address of `192.168.107.10`.
  3.1.5. Double-check the VMware virtual network editor has the settings described earlier.
  3.1.6. Make sure the machine's networking interfaces are assigned to the correct networks in VMware.
  
  3.2. If the second ping failed:
  
  3.2.1. Make sure the Kali machine's gateway is `192.168.107.10`.
  3.2.2. Make sure the router's `ens33` interface is in the `192.168.58.0/24` network.
  3.2.3. Check the custom service on the router with `systemctl status routing`.
  3.2.4. Manually run the `iptables.sh` script on the router.
  
  3.3. If the third ping failed, as above.

# Using the Environment

 1. Start the router, the Kali machine and any targets you wish to be active.
 
 2. To begin monitoring traffic with Snort, login to the router and run:
  ```
  $ sno
  ```
  # OR
  ```
  $ ./snort.sh
  ```
 
 3. To stop the monitoring, press Ctrl+C in the router.
 
 4. Happy hacking!