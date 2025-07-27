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

## How to Run

1. Clone the repo.
2. Spin up two Ubuntu Server VMs with the IPs above.
3. Follow step-by-step instructions in the `configs/` and `lab_setup/` folders to configure VXLAN and firewall rules.
4. Test connectivity and verify microsegmentation with ping and curl commands.

## Next Steps

- Expand to multiple workloads and VMs.
- Implement dynamic firewall rules using nftables.
- Integrate with orchestration tools for automation.

## License

MIT License
