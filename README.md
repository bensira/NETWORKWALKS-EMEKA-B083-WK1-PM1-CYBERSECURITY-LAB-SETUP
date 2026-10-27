# 🛡️ Cybersecurity Lab Setup — Week 1, Task 1: Isolated Environment Configuration

As part of my hands-on cybersecurity training, I built an isolated virtual lab environment using Oracle VM VirtualBox to safely practice network reconnaissance, threat detection, and security tool operations without exposing external systems.

---

## 🖥️ Lab Infrastructure Overview

To simulate a real-world enterprise or penetration-testing layout, the laboratory utilizes a host-only segmented virtual network topology housing both an attacking platform and a target system.

* **Hypervisor:** Oracle VM VirtualBox
* **Host Operating System:** Windows 10
* **Attacking Machine:** Kali Linux
* **Target Machine:** Windows 10 VM
* **Virtual Network Type:** Custom NAT Network (`NatNetwork`)
* **Subnet Range:** `10.0.0.0/24`

---

## 📋 Step-by-Step Implementation Guide

### Phase 1: Virtual Network Configuration
1. Opened **VirtualBox Preferences** > **Network** and created a custom NAT Network named `NatNetwork`.
2. Assigned the network CIDR block to **`10.0.0.0/24`** with DHCP enabled to manage local lease assignments.

### Phase 2: Kali Linux Configuration (Attacking Node)
1. Attached the Kali Linux virtual machine to the `NatNetwork` adapter.
2. Booted the VM and configured a static IP address to ensure persistent identification during testing:
   * **IP Address:** `10.0.0.2`
   * **Subnet Mask:** `255.255.255.0`
   * **Gateway:** `10.0.0.1`
   * **DNS Server:** `8.8.8.8`
3. Verified the interface configuration via terminal (`ifconfig` / `ip a`).
4. Tested baseline gateway connectivity using ICMP:
   ```bash
   ping -c 4 10.0.0.1
   Result: 0% packet loss, confirming proper routing out of the virtual switch.


### Phase 3: Windows VM Configuration (Target Node) 

1. Attached the target Windows 10 virtual machine to the same ⁠NatNetwork⁠ adapter.
2. ⁠Configured the static IP parameters via Network Connections (⁠ncpa.cpl⁠)
. IP Address: ⁠10.0.0.3
. Subnet Mask: ⁠255.255.255.0⁠
. Gateway: ⁠10.0.0.1⁠
. DNS Server: ⁠8.8.8.8⁠
3. Verified bidirectional routing by opening Command Prompt (⁠cmd⁠) on Windows and pinging the Kali node: ping 10.0.0.2

Result: 0% packet loss, confirming successful cross-platform communication inside the isolated subnet.



5. ## ⚙️ Summary Configuration Table

| Component | Configuration Details |
| :--- | :--- |
| **Host OS** | Windows 10 |
| **Hypervisor** | Oracle VM VirtualBox |
| **Security OS** | Kali Linux (`10.0.0.2/24`) |
| **Target OS** | Windows 10 VM (`10.0.0.3/24`) |
| **Virtual Switch** | `NatNetwork` (`10.0.0.0/24`) |
| **Default Gateway** | `10.0.0.1` |
| **Primary DNS** | `8.8.8.8` |


## 🎯 Purpose of the Lab

This environment will serve as my **isolated and controlled cybersecurity laboratory** for authorized security testing and hands-on learning.

Some of the activities I will be practicing include:

* 🔎 Network reconnaissance
* 🌐 Port scanning
* 🛡️ Vulnerability assessment
* 📡 Packet analysis
* 🔐 Web application security testing
* 💥 Exploitation practice
* 🧪 Security-tool experimentation

## 🛠️ Challenges & Troubleshooting

### Network Interface Reinitialization (`nmcli`)
* **Issue:** After modifying network configurations, the interface needed to be refreshed to drop old DHCP leases and cleanly apply the manual static IP settings.
* **Resolution:** 
  * Brought the interface down and back up using NetworkManager CLI commands:
    ```bash
    sudo nmcli connection down "Wired connection 1"
    sudo nmcli connection up "Wired connection 1"
    ```
  * Verified the interface successfully re-activated and updated the active route table.

### Key Takeaways & Lessons Learned
 
 Isolation First: Establishing a dedicated local subnet protects external networks from accidental traffic bleed during security evaluations.
 Static Addressing: Assigning fixed IPs (⁠10.0.0.2⁠ and ⁠10.0.0.3⁠) prevents routing instability when building multi-node scanning labs later.
 Verification Discipline: Utilizing ICMP ⁠ping⁠ validation confirms layer-3 connectivity before deploying complex scanning or monitoring tools.
🚀 Next Steps
With the core routing infrastructure validated and documented, the next phase will involve taking VirtualBox snapshots of both machines and begin introducing practical reconnaissance and security-testing activities into the lab.

Author
Emeka Nnorom
