# 🧠 Computer Network Design Project
<br>


<p>
  This repository contains my first major computer networking project, built using 
  <strong>Cisco Packet Tracer</strong>.
  The proposed design is tailored for a company offering homogeneous services—such as an 
  <em>accounting office</em> or similar business environment.
</p>

<p>
  The network infrastructure leverages technologies and best practices aligned with the 
  <strong>Cisco CCNA (Cisco Certified Network Associate)</strong> certification level. It includes 
  elements such as VLAN segmentation, routing, switching, network security, Access Control Lists and redundancy protocols, 
  providing a robust foundation for small to medium-sized business operations.
</p>

<br>
<br>
<br>

<h1>1. Structured Cabling Description</h1>
<h2>1.1. Network Backbone</h2>
  The foundation of the building's telecommunications infrastructure is multimode fiber optic cable, which guarantees high throughput and reliable data transmission. Each floor has its own     distribution cabinet connected to the central distribution point (CDP) with fiber optic patch cords running through dedicated shafts.

  The CDP is located on the 2nd floor and is protected by a video monitoring system and an RFID access card issued only to authorized personnel. In the CDP distribution cabinet, you can find   the same equipment as in the distribution cabinets from other floors: 2 patch panels, 2 access switches, 1 core switch, 1 APC (Automatic Power Control), while exclusively in this cabinet you will find an additional fiber optic switch, router, NAS (Network Attached Storage) and IDS/IPS (Intrusion Detection System/Intrusion Prevention System).

  <table border="1" cellpadding="5" cellspacing="0">
  <thead>
    <tr>
      <th>Rack Unit (U)</th>
      <th>Device</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1U</td>
      <td>Fiber Optic Patch Panel</td>
      <td>Building uplink</td>
    </tr>
    <tr>
      <td>2U</td>
      <td>Patch Panel 1</td>
      <td>Network cable management</td>
    </tr>
    <tr>
      <td>3U</td>
      <td>Patch Panel 2</td>
      <td>Network cable management</td>
    </tr>
    <tr>
      <td>4U</td>
      <td>Blank Panel</td>
      <td>Improved airflow and cable routing</td>
    </tr>
    <tr>
      <td>5U</td>
      <td>Access Switch (Cisco 2960)</td>
      <td>Close to patch panels for shorter patch cords</td>
    </tr>
    <tr>
      <td>6U</td>
      <td>Access Switch (Cisco 2960)</td>
      <td>Close to patch panels for shorter patch cords</td>
    </tr>
    <tr>
      <td>7U</td>
      <td>Core Switch (Cisco 3650)</td>
      <td>Close to patch panels for shorter patch cords</td>
    </tr>
    <tr>
      <td>8U</td>
      <td>Blank Panel</td>
      <td>Ventilation space</td>
    </tr>
    <tr>
      <td>9U</td>
      <td>Cisco Router 2911</td>
      <td>Service termination, Internet access, NAT</td>
    </tr>
    <tr>
      <td>10U</td>
      <td>Blank Panel</td>
      <td>Ventilation space</td>
    </tr>
    <tr>
      <td>11U</td>
      <td>IDS/IPS Appliance</td>
      <td>Network security</td>
    </tr>
    <tr>
      <td>...</td>
      <td>Blank Panel</td>
      <td>Clearance from UPS for cooling, expansion space</td>
    </tr>
    <tr>
      <td>21U</td>
      <td>APC UPS SMT1500RM2UC</td>
      <td>Power backup</td>
    </tr>
    <tr>
      <td>22U</td>
      <td>[Empty]</td>
      <td>[Empty or reserved]</td>
    </tr>
  </tbody>
</table>


<h2>1.2. Vertical Cabling</h2>
  The vertical cabling has been routed through specially planned telecommunications shafts and installation shafts, located in the immediate vicinity of the elevator shaft. This solution       ensures easy service access while protecting the cables from accidental damage and isolating them from user influence and external conditions. The shafts contain backbones based on           multimode fiber optic terminated with LC/PC connectors to SFP+ 10G modules, which makes the network resistant to fragmentation and transmission queuing, ensures bandwidth capable of          carrying the full load of individual users without affecting the rest of the network, and guarantees adequate throughput.

<h2>1.3. Horizontal Cabling</h2>
  Within the horizontal infrastructure, 3 cable management systems and subscriber outlet establishment systems have been implemented, adapted to the specifics of equipment installation and     workstations:

  Raised Floor and Cable Trays
  Where cables run within workstations, they have been hidden in metal cable trays and conduits, routed under raised floor panels. This solution ensures ease of modernization when there is a   need to adapt the infrastructure to new user requirements.

  Subscriber Outlets and Floor Boxes
  In places where desks are positioned against walls, standard subscriber outlets have been used, mounted at working height. In open space areas, where workstations are distant from walls,     floor boxes have been provided – aesthetic modules built into the floor, equipped with keystone jacks, enabling easy equipment connection without the need to run cables "visibly."

  Cabling in Suspended Ceilings
  Equipment mounted above 1 meter from floor level, such as WiFi access points (AP), has been connected using UTP Cat. 6A cables routed in the suspended ceiling space. This solution ensures    minimal interference with room aesthetics while enabling easy adjustment and potential network expansion without the need for wall demolition.

<h2>1.4. Description Methodology</h2>
  The network features clear and consistent naming conventions that facilitate management and diagnostics.

  Each outlet has a unique name according to the format:
  floor number - port number in patch panel, e.g., p1-1, p3-27.

  The cabling in the cabinet is maintained in a "1 to 1" scheme, meaning that switch ports correspond to patch panel port numbers to the greatest extent possible.

  All patch cords are labeled at both ends to avoid the need for manual cable tracing in the rack cabinet.

  Each switch and router in the network receives designation according to the scheme:
  floor number - s/r (switch/router) - device number

  Computers, printers, WiFi access points, and other client devices are designated according to the pattern:
  device type - floor number - subscriber outlet number, e.g., PC-p2-6.


<h1>2. Logical Structure Description</h1>

<h2>2.1. Separate Address Pools for Each Floor and Device Type</h2>
<table border="1" cellpadding="5" cellspacing="0">
  <thead>
    <tr>
      <th>VLAN</th>
      <th>Network</th>
      <th>Use Case</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>110</td>
      <td>10.1.10.0/24</td>
      <td>PC floor 1</td>
    </tr>
    <tr>
      <td>120</td>
      <td>10.1.20.0/24</td>
      <td>Printers floor 1</td>
    </tr>
    <tr>
      <td>130</td>
      <td>10.1.30.0/24</td>
      <td>Wi-Fi floor 1</td>
    </tr>
    <tr>
      <td>210</td>
      <td>10.2.10.0/24</td>
      <td>PC floor 2</td>
    </tr>
    <tr>
      <td>220</td>
      <td>10.2.20.0/24</td>
      <td>Printers floor 2</td>
    </tr>
    <tr>
      <td>230</td>
      <td>10.2.30.0/24</td>
      <td>Wi-Fi floor 2</td>
    </tr>
    <tr>
      <td>310</td>
      <td>10.3.10.0/24</td>
      <td>PC floor 3</td>
    </tr>
    <tr>
      <td>320</td>
      <td>10.3.20.0/24</td>
      <td>Printers floor 3</td>
    </tr>
    <tr>
      <td>330</td>
      <td>10.3.30.0/24</td>
      <td>Wi-Fi floor 3</td>
    </tr>
  </tbody>
</table>

  
  Justification:
  - Vertical isolation (between floors) – prevents Layer 2 broadcast traffic propagation and increases security.
  - Horizontal isolation (within floors) – separates different device types, facilitating network policy management.
  - Scalability – easy addition of new floors or device types without rebuilding the entire network.

  Each VLAN is a separate broadcast domain, which isolates Layer 2 traffic between floors.

<h2>2.2. Logical Addressing Scheme</h2>
	10.x.y.z/24, where:
  - x – floor number
  - y – device type
  - z – outlet number on the floor
    
<table border="1" cellpadding="5" cellspacing="0">
  <thead>
    <tr>
      <th>Floor (x)</th>
      <th>PC VLANs (y)</th>
      <th>Printer VLANs (y)</th>
      <th>Wi-Fi VLANs (y)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Floor 1</td>
      <td>10.1.10.0/24</td>
      <td>10.1.20.0/24</td>
      <td>10.1.30.0/24</td>
    </tr>
    <tr>
      <td>Floor 2</td>
      <td>10.2.10.0/24</td>
      <td>10.2.20.0/24</td>
      <td>10.2.30.0/24</td>
    </tr>
    <tr>
      <td>Floor 3</td>
      <td>10.3.10.0/24</td>
      <td>10.3.20.0/24</td>
      <td>10.3.30.0/24</td>
    </tr>
  </tbody>
</table>

  
    
<h2>2.3. VLAN 69 – Management (MGMT)</h2>
  Addressing: 172.16.69.0/24
  Assignment: All devices requiring remote management (switches, routers) have addresses in this subnet.
 <table border="1" cellpadding="5" cellspacing="0">
  <thead>
    <tr>
      <th>VLAN</th>
      <th>Address</th>
      <th>Use Case</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>69</td>
      <td>172.16.69.1/24</td>
      <td>Switch p1-s1-core</td>
    </tr>
    <tr>
      <td>69</td>
      <td>172.16.69.2/24</td>
      <td>Switch p2-s1-core</td>
    </tr>
    <tr>
      <td>69</td>
      <td>172.16.69.3/24</td>
      <td>Switch p3-s1-core</td>
    </tr>
    <tr>
      <td>69</td>
      <td>172.16.69.11/24</td>
      <td>Switch p1-s2-access</td>
    </tr>
    <tr>
      <td>69</td>
      <td>172.16.69.12/24</td>
      <td>Switch p1-s3-access</td>
    </tr>
    <tr>
      <td>69</td>
      <td>172.16.69.21/24</td>
      <td>Switch p2-s2-access</td>
    </tr>
    <tr>
      <td>69</td>
      <td>172.16.69.22/24</td>
      <td>Switch p2-s3-access</td>
    </tr>
    <tr>
      <td>69</td>
      <td>172.16.69.31/24</td>
      <td>Switch p3-s2-access</td>
    </tr>
    <tr>
      <td>69</td>
      <td>172.16.69.32/24</td>
      <td>Switch p3-s3-access</td>
    </tr>
    <tr>
      <td>69</td>
      <td>172.16.69.10/24</td>
      <td>Router p2-r1-core</td>
    </tr>
    <tr>
      <td>69</td>
      <td>172.16.69.20/24</td>
      <td>Router p2-r2-core</td>
    </tr>
  </tbody>
</table>


  
<h2>2.4. VLAN 100 – Internet Access</h2>
  Addressing: 10.100.100.0/24
  Assignment: Devices participating in routing to/from external networks (core switch p2-s1-core and routers)
 <table border="1" cellpadding="5" cellspacing="0">
  <thead>
    <tr>
      <th>VLAN</th>
      <th>IP Address</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>100</td>
      <td>10.100.100.1/24</td>
      <td>HSRPv2 virtual interface</td>
    </tr>
    <tr>
      <td>100</td>
      <td>10.100.100.2/24</td>
      <td>Trunk interface p2-s1-core VLAN 100</td>
    </tr>
    <tr>
      <td>100</td>
      <td>10.100.100.10/24</td>
      <td>Subinterface gig0/3/0.100 p2-r1-core</td>
    </tr>
    <tr>
      <td>100</td>
      <td>10.100.100.20/24</td>
      <td>Subinterface gig0/3/0.100 p2-r2-core</td>
    </tr>
  </tbody>
</table>

  
<h1>3. Technologies Used</h1>
   
<h2>3.1. Trunking at Access and Core Layer Interface</h2>
  EtherChannel has been implemented in the network, increasing throughput from 100 to 800Mbps between access-core layers (In physical equipment, I would implement switch-stack master-slave,    but Packet Tracer does not support this solution). This way, each host has 100Mbps available, and in case of infection and DoS attack attempt by link saturation, it can saturate a maximum    of 100Mbps, while the remaining 700Mbps is shared among the rest of the hosts. The solution is also cost-friendly, using cheaper, Fast Ethernet Cisco switches, e.g., Catalyst 2960.
  
 <h2>3.2. Switching & Routing</h2>
  InterVLAN routing occurs on enterprise-class L3 switches, here Catalyst 3650, which also runs the DHCP server distributing addresses within VLANs. Using these switches means creating a       core layer with management access where switches are addressed. Edge devices operate based on HSRP (Hot Standby Router Protocol), offering a single, virtual default gateway interface.        Such a router serves primarily as a gateway for secure remote VPN connections and for routing traffic in the in-out relationship, commonly called a NAT router, allowing the secure,           scalable network to access the Internet.
  
  HSRP operates in version 2 as one address configured on ports of two routers from the LAN side. It's worth mentioning that the "router-on-a-stick" technique was used to maintain interVLAN    routing on core switches and utilize a single router interface for routing traffic from Internet access VLAN 100 and serving as gateway to management VLAN 69. On the other side of the        router on gig0/1/0, a fictitious ISP address was set. For greater clarity, private Class C addresses were used; in worklife, dedicated IP addresses would appear on the edge                   interfaces of both routers in HSRP. In laboratory conditions, I used an Internet simulator in the form of a hop–router on which I performed static routing.
  
  Static routing:
  - 209.165.200.0/24 via 192.168.1.1 for traffic from our network toward the web server, without this there is no external communication.
  - 10.100.100.0/24 via 192.168.1.1 for traffic from the web server side to our network, without this there is no return communication.
  
  Pings to the server went only from routers in HSRP, but on p2-s1-core I set:
  ip route 209.165.200.0/24 via 10.100.100.1 (virtual router address).
  Additionally, default route 0.0.0.0 0.0.0.0 10.100.100.1 ensured traffic routing through the virtual interface, and on HSRP routers, dedicated access-lists were created for VLAN addressing:
  e.g. access-list 1 permit 10.1.30.0 0.0.0.255 and added to NAT:
  ip nat inside source list 1 interface gig0/1/0 overload, pointing to the router's edge interface for outgoing traffic. Dedicated return routes were added for VLAN addressing:
  ip route 10.1.30.0 255.255.255.0 10.100.100.2, pointing to the p2-s1-core switch interface in VLAN 100 (Internet access).

<h2>3.3. Port Security, DHCP Snooping, Dynamic ARP Inspection</h2>
  - Port Security:
  As protection against MAC Flooding and unauthorized hardware connection to the network, port security was introduced on access ports. Due to static address allocation on each wired host,     I decided to limit to 1 address per port with sticky parameter and shutdown reaction mode.
  
  - DHCP Snooping:
  Dynamic address allocation occurs only in VLANs 130, 230, 330, where DHCP Snooping mechanism was introduced to limit the impact of DHCP spoofing/starvation attacks. Trusted ports were       activated only on uplink connections; remaining ports are in untrusted state.
  
  - DAI (Dynamic ARP Inspection):
  Protection against ARP spoofing/poisoning attacks and associated MITM (Man-in-the-Middle) attacks is provided by Dynamic ARP Inspection, utilizing the previously introduced DHCP Snooping     Binding.
