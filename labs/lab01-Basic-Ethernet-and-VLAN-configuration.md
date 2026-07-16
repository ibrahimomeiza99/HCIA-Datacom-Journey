> **Author:** Ibrahim Omeiza
**Date:** 14/07/2026
**Duration:** 100 Minutes
**Difficulty:** ⭐⭐
**Status:** ✅ Completed
> 

---

## Objective

> This lab demonstrates inter-VLAN communication, port-based VLAN, MAC-address-based VLAN configuration, and traffic analysis.
> 

---

## Topology Diagram

!image.png

### IP Address Table

| Device | Interface | IP Address | Subnet Mask | Gateway |
| --- | --- | --- | --- | --- |
| S1 | GE0/0/0/1, GE0/0/0/10, GE0/0/0/13,  | N/A | N/A | N/A |
| S2 | GE0/0/1, GE0/0/0/10, GE0/0/0/14, | N/A | N/A | N/A |
| S3 | GE0/0/0/1 | 10.1.3.1 | 255.255.255.0 | N/A |
| S4 | GE0/0/0/2 | 10.1.3.2 | 255.255.255.0 | N/A |
| R1 | GE0/0/0/1 | 10.1.2.1 | 255.255.255.0 | N/A |
| R2 | GE0/0/0/2 | 10.1.10.1 | 255.255.255.0 | N/A |

---

## Tools and Environment

| Item | Detail |
| --- | --- |
| Simulator / Hardware | eNSP |
| Devices Used | Switch and Router |
| Operating System | Windows |
| Lab Reference | Textbook, Huawei doc, course module, Google, Claude AI |

---

## Theory and Concepts

**Key concept:**

This lab teaches you how to create VLANs, configure switches so that it understands which frame is to be delivered to a terminal (end device). You will also learn about the 802.1Q standard (VLAN ID), which is a 4-byte ID that is used to identify which VLAN a particular frame belongs to. In additio, you will also learn port-based and MAC address based VLAN configuration.

**Why it matters in cybersecurity / real networks:**

By understanding how networking devices communicate, you will be able to find out any anomaly that occurs during that communication. The concept of creating multiple VLANs is also similar to network separation. Network separation/segregation divides the overall network into multiple subnets so that even if an attack occurs, it doesn’t affect the whole network. It also used WireShark to capture packets, which is an essential tool in cybersecurity to capture live traffic from one device to another for further traffic analysis.

---

## Configuration Steps

---

# Step 1

Configure the names for the first switch as S1 and S2

```
# Step 1: Enter system view
<Huawei> system-view
[Huawei] sysname S1

[S1]

# Use similar command to change the name for switch 2
```

The command <sysname> changes the name of the switch to the name you specify.

---

# Step 2: Creating VLANs

I created vlans 2, 3 and 10 on S1 and S2 respectively as shown in the topology diagram above.

```
# Enter system-view
<S1> sys-view
[S1] vlan batch 2 to 3 10
# Use the same command to create vlans in S2
```

The above command creates vlan 2, 3 and 10 respectively

# Step 3: Configuring IP addreses

I configured the device IP addresses for R1 and R2 as 10.1.2.1/24 and 10.1.10.1/24 respectively.

```
# Step 1: Enter system view on R1
<R1> system-view
[R1] 
# Enter the interface to set the IP address
[R1] int g 0/0/1
[R1-GigabitEthernet0/0/1]ip address 10.1.2.1 24

# Use the above command to set the IP address for R2
```

### Set the IP addresses for S3 and S4 to 10.1.3.1/24 and 10.1.3.2/24 respectively

```
# Step 1: Enter system view
<S3> system-view
[S3] 
# Enter the interface to set the IP address
[S3] int g 0/0/1
[S3-GigabitEthernet0/0/1]ip address 10.1.3.1 24

# You might encounter a problem here because the interface does not support switching from layer 2 to layer 3 interfaces. To switch to layer 3 interface, use the **<undo portswitch>** command in the interface and then set the ip address again.

[S3-GigabitEthernet0/0/1] undo portswitch

# If the above process didn't work, then you have to create a Virtual Local Area Network Interface (VLANIF).
# To achieve this, you will have to create a VLAN first

[S3] vlan 3
[S3-vlan3] quit
[S3]
[S3] interface vlanif 3
[S3-vlanif3] ip address 10.1.3.1 24
```

---

# Step 4: Port-based configuration

I configured all the interfaces as shown in the topology diagram above into their respective mode as such as access, trunk, and hybrid mode on S1, S2, S3, and S4

```
# Enter system view on S1
<S1> system-view
[S1] 
[S1] interface GigabitEthernet0/0/1

[S1-GigabitEthernet0/0/1] port link-type access
# Set the Port VLAN ID (PVID) of the interface
[S1-GigabitEthernet0/0/1] port default vlan 2
[S1-GigabitEthernet0/0/1] quit
```

```
# Enter system view on S2
<S2> system-view
[S2] 
[S2] interface GigabitEthernet0/0/1

[S2-GigabitEthernet0/0/1] port link-type access
# Set the Port VLAN ID (PVID) of the interface
[S2-GigabitEthernet0/0/1] port default vlan 3
[S2-GigabitEthernet0/0/1] quit
```

```
# Enter system view on S3
<S3> system-view
[S3] 
[S3] interface GigabitEthernet0/0/1

[S3-GigabitEthernet0/0/1] port link-type access
# Set the Port VLAN ID (PVID) of the interface
[S3-GigabitEthernet0/0/1] port default vlan 3
[S3-GigabitEthernet0/0/1] quit
```

```
# Enter system view on S4
<S4> system-view
[S4] 
[S4] interface GigabitEthernet0/0/2

[S4-GigabitEthernet0/0/2] port link-type access
# Set the Port VLAN ID (PVID) of the interface
[S4-GigabitEthernet0/0/2] port default vlan 3
[S4-GigabitEthernet0/0/2] quit
```

# I configured the ports connecting S1 and S2 as trunk ports and allow only packets from VLAN 2 and VLAN 3 to pass through

```
# Enter system view on S1
<S1> system-view
[S1] 
# Set it to interface view
[S1] interface g 0/0/10
[S1-GigabitEthernet0/0/10] port link-type trunk
[S1-GigabitEthernet0/0/10] port trunk allow-pass 2 3
```

```
# Enter system view on S2
<S2> system-view
[S2] 
# Set it to interface view
[S2] interface g 0/0/10
[S2-GigabitEthernet0/0/10] port link-type trunk
[S2-GigabitEthernet0/0/10] port trunk allow-pass 2 3
```

### VLAN 1 should be disabled, because by default VLAN 1 is in the allowed list so if it is not used for any service, it should be deleted for security purposes.

```
# Enter system view on S1
<S1> system-view
[S1] 
# Set it to interface view
[S1] interface g 0/0/10
[S1-GigabitEthernet0/0/10] undo port trunk allow-pass vlan 1

# The same should be done on S2.
```

# Step 5: Configuring MAC address-based VLANS

MAC address-based VLAN is configured on an interface in a device when you prioritize security, it allows PCs to maintain its VLAN even if it is changed from one interface to another unlike port-based VLANs.

```
# Enter system view on S2
<S2> system-view
[S2]
# To configure mac address-based vlan, you must be in a vlan view
[S2] vlan 10
[S2-vlan10] mac-vlan mac-address 5489-9849-30c8
```

The mac-vlan mac-address command associates a MAC address with a VLAN.

```
```Set an interface on S2 to hybrid mode to allow packets
 from MAC address-based VLANs to pass through```
 # Enter system view on S2
 <S2> system-view
 [S2]
 
 # Enter into an interface
 [S2] interface g 0/0/1
 [S2-GigabitEthernet0/0/1] port link-type hybrid
 [S2-GigabitEthernet0/0/1] port hybrid untagged vlan 10
 [S2-GigabitEthernet0/0/1] quit
 
 # Use the same command on other interfaces.
 # Configure S2 and enable MAC address-based VLAN assignment between MAC addresses and VLANs. You must run the **mac-vlan enable** command
  [S2] interface g 0/0/1
  [S2-GigabitEthernet0/0/1] mac-vlan enable
  [S2-GigabitEthernet0/0/1] quit
  [S2]
  
```

# Display the configuration information

## VLAN configuration information on S1

!S1-VLAN-display.png

## VLAN configuration on S2

!S1-VLAN-display.png

## MAC-VLAN configuration information on S2

!S2-MAC-display-config.png

## Verification

I tested the following to ensure that my lab works just fine.

### Ping S4 from S3

!ping-s4-froms3.png

### Ping other devices from R1 and ensure the ping operation fails

!Screenshot 2026-07-16 122620.png

### Ping R1 from R3, capture packets on the link between S1 and S2, and ensure that the ping operation fails but data frames with VLAN 10 tag can be captured.

!Screenshot 2026-07-16 123852.png

### Run the **display mac-address verbose** command on S1 and S2 to check the MAC address tables on the switches.

mac-address verbos on S1

!Screenshot 2026-07-16 124012.png

mac-address verbos on S2

!Screenshot 2026-07-16 124038.png

Problems Encountered

**Problem 1:  IP address assignment error on S3**

- *What happened: The interface on S3 does not support layer 3 interface  switching mode in order to be assigned an IP address.*
- *Error message:* Unrecognized command found at '^' position.
- *What I tried: I tried changing the interface to a layer 3 interface mode  by using the undo portswitch command in order to support the layer 3 switching mode*
- *What actually fixed it: Creating VLANIF, then assigning an IP address to the interface resolved the issue*

Problem 2: Ping operation unsuccessful from S3 and S4

- *What happened: I encountered an unsuccessful attempt from S3 when I tried to test the connectivity between S3 and S4.*
- *Error message:  request timed out.*
- *What I tried: I tried to update the ip addreses of each Vlan interface on S3 and S4. I also updated the vlan of each switches.*
- *What actually fixed it:  I discovered the IP address on S4 wasn’t assigned, so I assigned the valid IP address, and it worked.*

---

## Key Takeaways

1. This lab taught me about configuring devices to communicate from different switches.
2. I learnt about data capture to identify some certain parameters using a networking tool called WireShark.
3. I also learnt about port and MAC-based interface configuration.

---

Commands Reference Card

| Command | What it does |
| --- | --- |
| `display ip routing-table` | Shows the routing table |
| `display interface brief` | Shows status of all interfaces |
| `mac-vlan mac-address [*mac-address id]*` | Associate a MAC address with a VLAN |
| `undo shutdown` | Brings up an interface |
| `ping [IP]` | Tests connectivity |
| `undo trunk allow-pass vlan [*vlan id*]` | Disable a VLAN that is not currently used  |

---

*Documented by Ibrahim Omeiza | HCIA-Datacom Learning Journey*
