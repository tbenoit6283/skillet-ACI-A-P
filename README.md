# Automated configuration of a cluster of VM series managed by Panorama for Single Pod PBR insertion

### Pre-Requisite :
- Panorama 8.1.X and 2 VM series 8.1.X connected to panorama but not attached to any template nor device group yet.
- Each of your VM series must have a minimum of 3 vNics for that setup and vNic must be defined as describe bellow: 
        - eth0 (vNic1) connected to the management network - eth1 (vNic2) connected to a dedicated port group of your DVS and it will be used for HA2 
        - eth2 (vNic3) connected to a dedicated port group of your DVS and it will be used for PBR connection to the fabric. Promiscuous Mode MUST be enabled on that port group in your DVS!!!`

- A PBR policy MUST be configured in parallel in the Fabric for your pod with filters on a Service Graph and that Service Graph must be applied to a contract to start redirecting some traffic to our cluster of VM Series.
  


### This skillet will automate these tasks for you on Panorama :
- create a Template Stack and a Template dedicated for each of your 2 VM Series all Network and Device policies configured with webpage variables.
- create a Device Group dedicated for that specific cluster of VM series configured for a Single Pod PBR insertion.
- move and attach your 2 VM series to these respective Template Stack and Device Group and trigger a commit to have a fully functional setup.
- configure ACI plugin to connect into APIC controller in order to collect metadata to be able to create DAGs in your firewall policy.  
 

## Variables
- STACK_DEVICE_A (Template Stack name Device A)
- STACK_DEVICE_B (Template Stack name Device B)
- DEVICEGROUP (Device Group name for your VM Series for ACI)
- HA2_DEVICE_A (Device A IP address for HA2)
- HA2_DEVICE_B (Device B IP address for HA2)
- MGT_DEVICE_A (MGT IP address of Device A)
- MGT_DEVICE_B (MGT IP address of Device B)
- SN_DEVICE_A (Serial Number of Device A)
- SN_DEVICE_B (Serial Number of Device B)
- DNS (DNS Server IP address)
- NTP (NTP Server IP address)
- ZONE (Zone creation for PBR interconnection)
- TARGET_IP_PBR (Target IP of the fabric for PBR without Netmask)
- LOCAL_IP_PBR (Local IP of your VM interface connected to the the fabric for PBR with Netmask)
- TAGCOLOR (Color of the TAG for your Zone)
- APIC_IP (IP address of APIC controller)
- APIC_LOGIN (Login of APIC controller)
- APIC_PASSWORD (Password of APIC controller)


## Caution  
- That skillet will not configure ACI Fabric to redirect traffic on the single Pod. That step must be done in parallel.
- Promiscuous mode MUST be enabled on vNIC3 port group to have a functional cluster A/P on your VM series
- That skillet should work with physical devices instead of VM series but it has not been tested.


## Support Policy

Have been tested with a single instance of panorama 8.1.X but should work as well with 9.0.X release.
Works with APIC 3.2 and higher releases.

The code and templates in the repo are released under an as-is, best effort,
support policy. These scripts should be seen as community supported and
Palo Alto Networks will contribute our expertise as and when possible.
We do not provide technical support or help in using or troubleshooting the
components of the project through our normal support options such as
Palo Alto Networks support teams, or ASC (Authorized Support Centers)
partners and backline support options. The underlying product used
(the VM-Series firewall) by the scripts or templates are still supported,
but the support is only for the product functionality and not for help in
deploying or using the template or script itself. Unless explicitly tagged,
all projects or work posted in our GitHub repository
(at https://github.com/PaloAltoNetworks) or sites other than our official
Downloads page on https://support.paloaltonetworks.com are provided under
the best effort policy.