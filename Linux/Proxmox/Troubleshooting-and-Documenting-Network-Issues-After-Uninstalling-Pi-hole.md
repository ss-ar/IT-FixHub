### Troubleshooting and Documenting Network Issues After Uninstalling Pi-hole

#### 1. **Background:**
After uninstalling Pi-hole, the system experienced network connectivity issues. The previous network configuration was based on the older `/etc/network/interfaces` method, which was replaced by `systemd-networkd` after the removal of Pi-hole. This led to a broken network setup, causing the system to be offline.

#### 2. **Problem Identified:**
- The main issue stemmed from the fact that Pi-hole was utilizing `networking.service` for network management, which was uninstalled during the Pi-hole removal process.
- The network configuration, which was previously managed using the `/etc/network/interfaces` file, was incompatible with the new `systemd-networkd` system, which had to be manually configured for network interfaces to work correctly.

#### 3. **Solution:**
To restore the network connectivity, the following steps were performed:

1. **Checking Current Network Configuration**:
   Initially, the `networking.service` was not found because it had been removed with Pi-hole. The system was relying on `systemd-networkd` for network management, but it was not yet properly configured.

   The following command was run to check the available network services:
   ```bash
   systemctl list-units | grep network
   ```

2. **Configuring `systemd-networkd`**:
   The correct network configuration was created by using `systemd-networkd`:

   - Create the configuration file in `/etc/systemd/network/`, for example: `/etc/systemd/network/10-static.network`.
   - Define the static IP address, gateway, and DNS servers.
   - Example configuration:
     ```plaintext
     [Match]
     Name=eth0  # Replace with actual network interface name

     [Network]
     Address=10.0.0.104/24
     Gateway=10.0.0.1
     DNS=1.1.1.1
     ```

3. **Restarting the Network Service**:
   After creating the configuration file, the `systemd-networkd` service was restarted to apply the changes:
   ```bash
   systemctl restart systemd-networkd
   ```

4. **Verifying Connectivity**:
   Finally, after restarting the networking service, the network interface was checked with:
   ```bash
   ip a
   ```
   Connectivity was tested with:
   ```bash
   ping 10.0.0.1   # Gateway ping
   ping 1.1.1.1    # DNS ping
   ping google.com # External site ping
   ```

   These steps confirmed that the system was back online with the static IP configuration working as expected.

#### 4. **Additional Considerations**:
- **Switching to `systemd-networkd`**: This method is becoming the default for many Linux distributions as it provides better integration with `systemd` and is more flexible than traditional networking methods.
- **Legacy Configuration**: The `/etc/network/interfaces` file is still supported in many distributions, but `systemd-networkd` is now preferred for newer setups due to its performance and ease of integration.
- **DNS Configuration**: After ensuring that the network was working, it was important to check the DNS resolution. This was done by configuring the `/etc/resolv.conf` file with a DNS server (like `1.1.1.1`).

#### 5. **Useful Resources**:
- [systemd-networkd Documentation](https://www.freedesktop.org/wiki/Software/systemd/network/)
- [Pi-hole Network Configuration](https://docs.pi-hole.net/main/post-install/)
- [systemd Networking Guide](https://www.digitalocean.com/community/tutorials/understanding-and-using-systemd-networkd-on-ubuntu-20-04)

This documentation process explains how to troubleshoot and resolve network issues caused by the removal of Pi-hole and the transition to using `systemd-networkd` for managing network interfaces in a modern Linux environment.
