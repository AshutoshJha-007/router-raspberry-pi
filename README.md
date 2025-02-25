# Router Setup Using Raspberry Pi

**Dec 2024 - Jan 2025**

## Project Overview
This project involved configuring a **Raspberry Pi** as a fully functional router to provide secure and efficient internet access. The goal was to enable seamless connectivity, implement network security measures, and optimize bandwidth usage using advanced configurations.

## Key Features
- **Wi-Fi Hotspot Creation**: Installed and configured **hostapd** to turn the Raspberry Pi into a Wi-Fi access point, allowing multiple devices to connect wirelessly.
- **DHCP Server Setup**: Configured **dnsmasq** to dynamically assign IP addresses to connected devices, ensuring efficient network management.
- **NAT (Network Address Translation)**: Enabled NAT using **iptables** to allow internet sharing from an Ethernet or cellular connection.
- **Firewall and Security**: Implemented firewall rules using **iptables** to filter incoming and outgoing traffic, enhancing network security.
- **Traffic Management & QoS**: Configured **tc (traffic control)** to manage bandwidth allocation and prioritize critical network traffic.
- **Network Monitoring & Debugging**: Used **Wireshark** and **iftop** to analyze network traffic, troubleshoot connectivity issues, and optimize performance.

## Step-by-Step Procedure
1. **Prepare the Raspberry Pi**
   - Install **Raspberry Pi OS**.
   - Update system dependencies using:
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

2. **Install Required Packages**
   - Install hostapd and dnsmasq:
     ```bash
     sudo apt install hostapd dnsmasq -y
     ```
   - Enable and start the services:
     ```bash
     sudo systemctl unmask hostapd
     sudo systemctl enable hostapd
     ```

3. **Configure DHCP Server (dnsmasq)**
   - Edit the configuration file:
     ```bash
     sudo nano /etc/dnsmasq.conf
     ```
   - Add the following lines:
     ```ini
     interface=wlan0
     dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h
     ```

4. **Configure the Wi-Fi Hotspot (hostapd)**
   - Edit the configuration file:
     ```bash
     sudo nano /etc/hostapd/hostapd.conf
     ```
   - Add the following:
     ```ini
     interface=wlan0
     driver=nl80211
     ssid=MyRouter
     hw_mode=g
     channel=7
     wmm_enabled=0
     macaddr_acl=0
     auth_algs=1
     ignore_broadcast_ssid=0
     wpa=2
     wpa_passphrase=StrongPassword
     wpa_key_mgmt=WPA-PSK
     rsn_pairwise=CCMP
     ```

5. **Configure NAT (Network Address Translation)**
   - Enable IPv4 forwarding:
     ```bash
     sudo nano /etc/sysctl.conf
     ```
   - Uncomment the following line:
     ```ini
     net.ipv4.ip_forward=1
     ```
   - Apply the changes:
     ```bash
     sudo sysctl -p
     ```

6. **Set Up iptables Firewall Rules**
   - Configure NAT rules:
     ```bash
     sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
     ```
   - Save the rules:
     ```bash
     sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
     ```

7. **Enable Services and Reboot**
   - Restart the services:
     ```bash
     sudo systemctl restart hostapd
     sudo systemctl restart dnsmasq
     ```
   - Reboot the system:
     ```bash
     sudo reboot
     ```

## Tools & Technologies Used
- **Hardware**: Raspberry Pi, Ethernet adapter, Wi-Fi dongle
- **Software**: Raspberry Pi OS, hostapd, dnsmasq, iptables, tc, Wireshark, iftop
- **Protocols**: TCP/IP, DHCP, NAT, QoS

## Outcomes & Learnings
- Successfully created a stable and secure wireless network using Raspberry Pi.
- Gained experience in **network administration, security configurations, and troubleshooting**.
- Developed skills in **Linux networking, iptables firewall management, and traffic optimization**.

## Repository
[Project Repository on GitHub](https://github.com/AshutoshJha-007/router-raspberry-pi)
