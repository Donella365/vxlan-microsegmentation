# VXLAN-Micro-segmentation
VXLAN-based microsegmentation lab demonstrating Zero Trust principles using free tools.
# VXLAN-Based Microsegmentation Lab

This project demonstrates how to implement microsegmentation using VXLAN overlays in a virtual lab environment. The lab simulates a network where workloads are segmented and isolated using VXLAN tunnels, applying Zero Trust security principles through firewall rules.

## Objective

Create isolated Layer 2 overlays (VXLANs) for different workloads (e.g., web server, database) over a shared Layer 3 infrastructure. Enforce microsegmentation and Zero Trust policies by restricting network traffic between workloads.

## Tools Used

- VirtualBox for virtualization
- Ubuntu Server as VM OS
- Linux commands: iproute2, bridge-utils, iptables
- draw.io for network topology diagrams

## Project Topology

- Web VM (192.168.56.101) and DB VM (192.168.56.102)
- VXLAN Tunnel between VMs with VXLAN ID 100
- Dummy interfaces simulate workload network interfaces
- Firewall rules enforce least privilege access

## ⚙️ Step-by-Step Setup

### 1. Launch 2 Ubuntu Server VMs

Use VirtualBox to spin up:
- `web-vm`: 192.168.56.101  
- `db-vm`: 192.168.56.102  

Ensure both VMs are on the same host-only network and can ping each other.

---

### 2. Configure VXLAN Tunnel (on **each** VM)

Replace `<peer_IP>` with the *other* VM's IP address:

```bash
# Create VXLAN interface
ip link add vxlan100 type vxlan id 100 dev eth0 remote <peer_IP> dstport 4789
ip link set vxlan100 up

# Create bridge and bind to VXLAN
brctl addbr br0
brctl addif br0 vxlan100
ip link set br0 up
```

---

### 3. Add Dummy Interfaces to Simulate Workloads

```bash
ip link add dummy0 type dummy
ip addr add 10.0.0.1/24 dev dummy0  # Use .2 for the other VM
ip link set dummy0 up
brctl addif br0 dummy0
```

Use different dummy IPs for web and db workloads (e.g., `10.0.0.1` and `10.0.0.2`).

---

### 4. Set Up Firewall Rules with iptables

Allow only specific traffic (e.g., HTTP from web to DB) and block everything else:

```bash
# Default deny
iptables -P FORWARD DROP

# Allow web to DB on port 3306 (MySQL)
iptables -A FORWARD -s 10.0.0.1 -d 10.0.0.2 -p tcp --dport 3306 -j ACCEPT
```

---

## ✅ Testing

- Use `ping` and `curl` between dummy interfaces to validate isolation.
- Verify blocked traffic doesn’t go through unless explicitly allowed.

---

## 🚀 Next Steps

- Expand lab to include more workloads and VXLAN segments  
- Switch to `nftables` for modern firewall control  
- Integrate with automation tools like Ansible or Terraform  
- Visualize traffic flow using Wireshark  

---

## 🗒️ Notes

This project is for educational and documentation purposes only. No setup automation or repo cloning is required—just manual configuration to demonstrate microsegmentation with VXLAN.
