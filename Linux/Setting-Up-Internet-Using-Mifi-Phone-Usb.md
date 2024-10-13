
---

### **Step 1: Connect MiFi USB to Provide Temporary Internet**

#### **1.1: Connect MiFi USB and Identify the Interface**

1. **Plug in the MiFi USB device**:
   - Once you connected your MiFi via USB, it was automatically recognized by the system as a network interface.
   
2. **List all network interfaces** to identify the newly added MiFi interface:
   ```bash
   ip link
   ```
   - You saw the MiFi connection appear as a new interface (e.g., `usb0` or `enp0s29u1u2`, depending on your system). 
   - At this point, your Proxmox system wasn’t yet using it for internet connectivity.

#### **1.2: Check the IP Address of the MiFi Interface**

3. **Check the IP address assigned to the MiFi interface**:
   - To confirm that the MiFi interface received an IP address from the router, run:
   ```bash
   ip addr show <interface_name>
   ```
   Example:
   ```bash
   ip addr show usb0
   ```
   - You should see an IP address like `10.0.0.x/24` under the interface.

#### **1.3: Set the MiFi Interface as the Default Gateway**

4. **Check the current routing table**:
   - The routing table determines which network interface is used to access the internet. At this stage, there may have been conflicting routes from other interfaces like `vmbr0`.
   - To check the routing table, run:
   ```bash
   ip route
   ```
   - You likely saw routes that pointed to both `vmbr0` (e.g., 10.0.0.149) and the MiFi interface (e.g., 10.0.0.178).

5. **Delete the conflicting route** (if necessary):
   - If `vmbr0` was listed as the default route (with `default via 10.0.0.1 dev vmbr0`), you needed to remove that default route so that the system could use the MiFi interface for internet access.
   - Remove the route by running:
   ```bash
   sudo ip route del default via 10.0.0.1 dev vmbr0
   ```

6. **Add the MiFi interface as the new default gateway**:
   - Once you removed the conflicting route, you set the MiFi interface as the new default gateway for internet access.
   - Add the new default route using the IP address of the MiFi connection’s gateway (e.g., `10.0.0.1`):
   ```bash
   sudo ip route add default via <gateway_ip> dev <interface_name>
   ```
   Example:
   ```bash
   sudo ip route add default via 10.0.0.1 dev usb0
   ```

7. **Verify that the MiFi interface is now the default route**:
   - Run `ip route` again to ensure that the MiFi interface is now listed as the default route. The output should look like:
   ```plaintext
   default via 10.0.0.1 dev usb0
   ```

8. **Test the internet connection**:
   - After setting the MiFi interface as the default gateway, test the connection by pinging a public IP like Google DNS or Cloudflare:
   ```bash
   ping 1.1.1.1
   ```
   - If successful, you now have internet access through the MiFi USB interface.

#### **1.4: Install NetworkManager**

9. **Install NetworkManager**:
   - After establishing temporary internet access through the MiFi USB, you proceeded to install NetworkManager to manage Wi-Fi connections more easily:
   ```bash
   sudo apt update
   sudo apt install network-manager
   ```

10. **Enable and start NetworkManager**:
   - You made sure that NetworkManager was enabled and running:
   ```bash
   sudo systemctl enable NetworkManager
   sudo systemctl start NetworkManager
   ```

---

### **Summary of Step 1**

- **Connected the MiFi USB device** to the Proxmox server and identified the new interface (e.g., `usb0`).
- **Checked the IP address** assigned to the MiFi interface.
- **Adjusted the routing table** to make the MiFi interface the default gateway by deleting the old default route and adding the MiFi interface as the new default.
- **Tested the connection** to confirm internet access through the MiFi.
- **Installed NetworkManager** to simplify network management, particularly for the Wi-Fi setup later.

---