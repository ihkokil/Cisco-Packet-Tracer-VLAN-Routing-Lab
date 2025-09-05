# Corporate Network Infrastructure Lab with VLANs and Inter-VLAN Routing

![CI/CD](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Cisco%20Packet%20Tracer-blue?style=for-the-badge)
![Technologies](https://img.shields.io/badge/Technologies-VLAN%2C%20DHCP%2C%20DTP%2C%20Port%20Security-orange?style=for-the-badge)

---

## 📖 Project Overview

This repository showcases a meticulously configured Cisco Packet Tracer lab environment simulating a scalable and secure network infrastructure for a hypothetical small-to-medium business, **XYZ Corp.** The project demonstrates fundamental network design and security principles, including network segmentation using VLANs, dynamic IP address management via DHCP, and Layer 2 security with Port Security.

The core of this simulation is a **"Router-on-a-Stick"** topology, an efficient method for enabling communication between multiple Virtual LANs (VLANs) using a single router interface. This design is both cost-effective and a common real-world solution for segmenting networks in corporate environments.

## Project Scenario & Problem Statement 📝

This section details the "why" behind the network design presented in this lab.

**Scenario:** A growing startup, **XYZ Corp.**, needs to design a scalable and secure network for its new office. The office houses four distinct user groups: Sales (VLAN 10), Marketing (VLAN 20), Human Resources (VLAN 30), and a dedicated Management segment (VLAN 99). A central server is also required for shared resources.

**Problem Statement:**
Design and implement a corporate network infrastructure that meets the following critical requirements:

1.  **Network Segmentation:** Traffic from different departments must be logically separated into distinct broadcast domains to enhance security and reduce overall network broadcast overhead.
2.  **Inter-Departmental Communication:** All departments must be able to communicate with each other and with the central server for essential business operations.
3.  **Dynamic IP Allocation:** The network must automatically assign IP addresses, subnet masks, and default gateway information to all client devices to minimize administrative effort and prevent configuration errors.
4.  **Security:** Implement basic port-level security on access switches to prevent unauthorized devices from connecting to the network.
5.  **Scalability:** The design should be modular and easily expandable, allowing for the future addition of new departments (VLANs) or services without a complete redesign.

This Packet Tracer file provides a practical solution by implementing VLANs for segmentation, a router-on-a-stick configuration for inter-VLAN routing, DHCP for dynamic addressing, and port security for Layer 2 access control.

## ✍️ Author

*   **Md. Iqbal Haider Khan**
    *   GitHub: [@ihkokil](https://github.com/ihkokil)
    *   LinkedIn: [linkedin.com/in/ihkokil](https://www.linkedin.com/in/ihkokil/)

---

## 📋 Table of Contents
1.  [Core Objectives](#-core-objectives)
2.  [Network Topology](#-network-topology)
3.  [Technologies Demonstrated](#-technologies-demonstrated)
4.  [IP Addressing Architecture](#-ip-addressing-architecture)
5.  [Configuration Highlights](#-configuration-highlights)
6.  [Getting Started](#-getting-started)
7.  [Validation & Testing](#-validation--testing)

---

## 🎯 Core Objectives

The design and configuration of this network were guided by the following technical objectives:

*   **Implement Robust Network Segmentation:** Utilize VLANs to logically separate the network into distinct broadcast domains for different corporate departments (Sales, Marketing, HR, Management), thereby enhancing security and network performance.
*   **Establish Seamless Inter-VLAN Communication:** Configure a router to act as a central gateway, enabling controlled and efficient traffic routing between all VLANs using the Router-on-a-Stick method.
*   **Automate IP Address Management:** Deploy a centralized DHCP server on the router to dynamically assign IP addresses, subnet masks, default gateways, and DNS server information to endpoints, significantly reducing administrative overhead and preventing IP conflicts.
*   **Enforce Layer 2 Security:** Implement switch port security measures to mitigate unauthorized access by restricting port access to specific MAC addresses, thus preventing rogue devices from connecting to the network.

---

## 🌐 Network Topology

The network architecture features a central router serving as the default gateway for all VLANs. This router connects to distribution switches, which in turn connect to access switches serving end devices like PCs and printers across different departments.

![Network Topology Diagram](topology.png)

*The diagram visually represents the hierarchical structure and segmentation of the network, illustrating how devices are distributed and logically grouped into different VLANs.*

---

## 💻 Technologies Demonstrated

This project showcases hands-on proficiency with the following networking technologies and concepts:

| Technology              | Description                                                                                                         |
| :---------------------- | :------------------------------------------------------------------------------------------------------------------ |
| **VLANs (Virtual LANs)**| Logically segmenting the physical LAN into multiple broadcast domains to isolate traffic and improve security.        |
| **802.1Q Encapsulation**| The IEEE standard for VLAN tagging. Used on trunk links between routers and switches to identify VLAN membership of frames. |
| **Inter-VLAN Routing**  | Employing a router with sub-interfaces (Router-on-a-Stick) to route packets between different VLANs.                   |
| **DHCP**                | Automating the assignment of IP addresses, subnet masks, default gateways, and DNS server information to client machines. |
| **Port Security**       | A Layer 2 security feature that restricts input to an interface by limiting the MAC addresses allowed to send traffic. |
| **Trunking (DTP)**      | Configuring switch ports to carry traffic for multiple VLANs between switches and routers, typically using 802.1Q.   |
| **Cisco IOS CLI**       | The command-line interface used for configuring, managing, and troubleshooting Cisco networking devices.               |

---

## 🏛️ IP Addressing Architecture

A structured IP addressing scheme was implemented to support the VLAN architecture. Each department/VLAN operates on its own unique subnet, with the router providing the default gateway for each through its sub-interfaces.

| VLAN ID | Department / Purpose | Network Address   | Gateway IP (Router Sub-interface) | DHCP Pool Range           | Notes                                       |
| :------ | :------------------- | :---------------- | :-------------------------------- | :------------------------ | :------------------------------------------ |
| 10      | Sales                | `192.168.10.0/24` | `192.168.10.1`                    | `192.168.10.10 - .254`    |                                             |
| 20      | Marketing            | `192.168.20.0/24` | `192.168.20.1`                    | `192.168.20.10 - .254`    |                                             |
| 30      | HR                   | `192.168.30.0/24` | `192.168.30.1`                    | `192.168.30.10 - .254`    |                                             |
| 99      | Management           | `192.168.99.0/24` | `192.168.99.1`                    | `192.168.99.10 - .254`    |                                             |
| N/A     | Server               | `192.168.10.0/24` | `192.168.10.100` (Static)         | N/A                       | Example: Server0 is statically assigned within the Sales VLAN subnet. |

---

## ⚙️ Configuration Highlights

Below are key configuration snippets demonstrating the implementation of core features within the Cisco IOS.

#### 1. Router-on-a-Stick Sub-interfaces (Example: Router0)

Each sub-interface on the router's physical interface is configured with an IP address to serve as the default gateway for its respective VLAN. The `encapsulation dot1Q` command enables the router to process tagged frames received from the trunk link.

```cisco
! Interface for VLAN 10 (Sales)
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

! Interface for VLAN 20 (Marketing)
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

! Interface for VLAN 30 (HR)
interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0

! Interface for VLAN 99 (Management)
interface GigabitEthernet0/0.99
 encapsulation dot1Q 99
 ip address 192.168.99.1 255.255.255.0
```

#### 2. DHCP Pool Configuration (Example: Router0)

DHCP pools are defined for each departmental VLAN to automate IP address allocation. Specific IP addresses or ranges are marked as excluded to reserve them for static assignments (e.g., servers, printers, or management devices).

```cisco
! DHCP Excluded Addresses for VLAN 10 (Sales)
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.10.100 ! Reserved for Server0

! DHCP Pool Definition for VLAN 10 (Sales)
ip dhcp pool VLAN10_SALES
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8 ! Example DNS server address

! Example DHCP Pool for VLAN 20 (Marketing)
ip dhcp pool VLAN20_MARKETING
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1

! ... (Similar DHCP pool configurations would exist for VLAN 30 and VLAN 99) ...
```

#### 3. Switch Trunk Port Configuration (Example: Switch0)

The switch interface that connects to the central router is configured as a trunk port. This is essential for allowing traffic from multiple VLANs to traverse this link using 802.1Q tagging.

```cisco
interface GigabitEthernet0/1
 description Trunk link to Router0
 switchport mode trunk
```

#### 4. Switch Access Port & Security (Example: Switch0)

Access ports are configured to belong to a specific VLAN for end devices. `Port Security` is applied to restrict the MAC addresses allowed on a port, enhancing security against unauthorized device connections. `mac-address sticky` automatically learns and stores the MAC address of the first connected device, while `violation shutdown` immediately disables the port if a security violation occurs.

```cisco
interface FastEthernet0/2
 description Access port for Sales PC
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security mac-address sticky
 switchport port-security maximum 1 ! Configure to allow only one MAC address
 switchport port-security violation shutdown
```

---

## 🚀 Getting Started

To explore and test this lab environment:

1.  Ensure you have **Cisco Packet Tracer** (version 7.3 or higher recommended) installed on your system.
2.  Clone this repository to your local machine:
    ```bash
    git clone https://github.com/ihkokil/Cisco-Packet-Tracer-VLAN-Routing-Lab.git
    ```
3.  Navigate to the project directory. Open the Packet Tracer simulation file (e.g., `CISCO-Network.pkt`). You might need to rename this file if it has a different name within the cloned repository (e.g., `XYZ-Network.pkt`).

---

## ✅ Validation & Testing

You can confirm the network's functionality by executing the following commands from the Command Prompt of any PC within the simulation:

1.  **Verify DHCP Allocation:** Check if the PC has successfully received a valid IP address, subnet mask, and default gateway from its VLAN's assigned DHCP pool.
    ```bash
    ipconfig
    ```
2.  **Test Intra-VLAN Connectivity (Same Department):** Ensure devices within the same VLAN can communicate directly. For example, ping a PC in VLAN 10 from another PC in VLAN 10.
    ```bash
    ping 192.168.10.11  ! Example: Ping from PC in VLAN 10 to another PC in VLAN 10
    ```
3.  **Test Inter-VLAN Connectivity (Cross-Department):** Verify that devices in different VLANs can communicate with each other via the router. For example, ping a device in VLAN 20 from a PC in VLAN 10.
    ```bash
    ping 192.168.20.10   ! Example: Ping from PC in VLAN 10 to PC in VLAN 20
    ```
    *(Note: The first ping attempt to a new destination may fail due to ARP resolution delays. Subsequent pings should succeed).*
4.  **Test Connectivity to Server:** Ping the central server from any department's PC to ensure network-wide accessibility to shared resources.
    ```bash
    ping 192.168.10.100 ! Example: Ping Server0 from any PC in any VLAN
    ```

Successful responses to all these tests confirm that the network segmentation, inter-VLAN routing, dynamic IP address management, and basic Layer 2 security measures are functioning correctly as per the design objectives.
