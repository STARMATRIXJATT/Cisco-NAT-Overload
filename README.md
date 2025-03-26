# Cisco NAT Overload (PAT) Project

## 📌 Overview
This project demonstrates the configuration of **NAT Overload (PAT)** in **Cisco Packet Tracer**. NAT Overload allows multiple internal devices to share a single public IP for internet access.



### 📌 IP Scheme:
| Device      | Interface       | IP Address        |
|------------|---------------|------------------|
| **Router0 (NAT Router)** | Gig0/0 (LAN) | 192.168.1.1/24 |
| **Router0 (NAT Router)** | Gig0/1 (WAN) | 203.0.113.1/24 |
| **Router1 (ISP Router)** | Gig0/0 | 203.0.113.2/24 |
| **PC0** | NIC | 192.168.1.2/24 |
| **PC1** | NIC | 192.168.1.3/24 |

## ⚙️ Configuration Steps

### **1. Configure Router0 (NAT Router)**
```bash
enable
configure terminal
hostname NAT_Router

! Configure WAN Interface (Public IP)
interface GigabitEthernet0/1
 ip address 203.0.113.1 255.255.255.0
 no shutdown
 exit

! Configure LAN Interface (Private Network)
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
 exit

! Define ACL for NAT
access-list 1 permit 192.168.1.0 0.0.0.255

! Configure NAT Overload
ip nat inside source list 1 interface GigabitEthernet0/1 overload

! Define NAT Interfaces
interface GigabitEthernet0/1
 ip nat outside
 exit

interface GigabitEthernet0/0
 ip nat inside
 exit

! Default Route to ISP
ip route 0.0.0.0 0.0.0.0 203.0.113.2
end
write memory
```

### **2. Configure Router1 (ISP Router)**
```bash
enable
configure terminal
hostname ISP_Router

! Configure WAN Interface
interface GigabitEthernet0/0
 ip address 203.0.113.2 255.255.255.0
 no shutdown
 exit

! Default route for internet simulation
ip route 0.0.0.0 0.0.0.0 <next-hop-to-internet>
end
write memory
```

## ✅ Verification
To verify that NAT Overload is working correctly, run the following commands:
```bash
show ip nat translations
show ip nat statistics
```

## 🚀 Getting Started with Cisco Packet Tracer
1. Open **Cisco Packet Tracer**.
2. Create the **network topology** as described.
3. Configure the devices using the provided **commands**.
4. Test **internet connectivity** using `ping` from the PCs.
5. Verify NAT using `show ip nat translations`.

## 📌 Contributing
Feel free to fork this repository, improve configurations, or report issues.

## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
👨‍💻 **Author:** Jatin Choudhary  
📧 **Contact:** jatin.cu14@gmail.com  
🏢 **Company:** HCLTech


