
---

### **Setting Up Internet and Wi-Fi on Proxmox: A Step-by-Step Guide**

#### **Step 1: Connect MiFi USB to Provide Temporary Internet**

1. **List Network Interfaces**:
   - Used the command to list all network interfaces and identify the USB MiFi connection:
     ```bash
     ip link
     ```
   - You identified a new network interface (e.g., `usb0` or similar) for the MiFi connection.

2. **Check the IP Address**:
   - Verified the IP address assigned to the MiFi interface:
     ```bash
     ip addr show <interface_name>
     ```

3. **Fix Routing Issues**:
   - Initially, there was a conflict in the routing table, where multiple interfaces (e.g., `vmbr0` and the MiFi interface) were present.
   - You checked the current routes using:
     ```bash
     ip route
     ```
   - Adjusted routing by ensuring the correct gateway and interface were prioritized. You temporarily used the MiFi USB interface for internet access, and this allowed the server to connect.

---

#### **Step 2: Configure Proxmox to Connect to Wi-Fi Using NetworkManager**

1. **Comment Out Manual Configurations in `/etc/network/interfaces`**:
   - Edited `/etc/network/interfaces` to remove manual configurations that conflicted with NetworkManager.
     ```bash
     sudo nano /etc/network/interfaces
     ```
   - Commented out the `vmbr0` bridge and `wlp7s0` Wi-Fi interface configurations:
     ```plaintext
     # auto vmbr0
     # iface vmbr0 inet static
     #    address 10.0.0.149/24
     #    gateway 10.0.0.1
     #    bridge-ports wlp7s0
     #    bridge-stp off
     #    bridge-fd 0
     
     # iface wlp7s0 inet manual
     ```

2. **Restart NetworkManager**:
   - Ensured that NetworkManager would take control of network interfaces:
     ```bash
     sudo systemctl restart NetworkManager
     ```

3. **Verify NetworkManager Controls Wi-Fi**:
   - Checked the status of the Wi-Fi adapter to ensure it was managed by NetworkManager:
     ```bash
     nmcli device status
     ```
   - Ensured that `wlp7s0` was **managed** and not blocked:
     ```bash
     sudo rfkill unblock wifi
     ```

4. **Manually Connect to Wi-Fi Using NetworkManager**:
   - Connected to your Wi-Fi network via `nmcli`:
     ```bash
     sudo nmcli device wifi connect "YourWiFiSSID" password "YourWiFiPassword"
     ```
   - Verified that the Wi-Fi adapter received an IP address:
     ```bash
     ip addr show wlp7s0
     ```

---

#### **Step 3: Set Wi-Fi to Auto-Connect on Boot**

1. **Enable Auto-Connect**:
   - Set the Wi-Fi connection to automatically connect on boot:
     ```bash
     sudo nmcli connection modify "YourWiFiSSID" connection.autoconnect yes
     ```

2. **Verify Auto-Connect**:
   - Checked the configuration to ensure that auto-connect is enabled:
     ```bash
     nmcli connection show "YourWiFiSSID"
     ```

---

#### **Step 4: Final Checks**

1. **Reboot and Verify Connection**:
   - Rebooted the Proxmox server to ensure the Wi-Fi connection works after a reboot:
     ```bash
     sudo reboot
     ```

2. **Check Networking**:
   - Verified that the system automatically connected to the Wi-Fi on boot and that Proxmox had access to the network.

3. **Access Proxmox Web Panel**:
   - Successfully accessed the Proxmox web interface using the server’s IP address assigned by the Wi-Fi network.

---

### **Final Outcome**:
- The Proxmox system is now configured to connect automatically to Wi-Fi on boot, using NetworkManager to handle all network connections.
- You no longer have to rely on the USB MiFi connection for internet access, and you can manage Proxmox through the web panel remotely.

---
