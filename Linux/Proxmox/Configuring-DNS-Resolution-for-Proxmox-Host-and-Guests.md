Configuring DNS Resolution for Proxmox Host and Guests

## Introduction

This documentation provides a step-by-step guide to resolve DNS issues in Proxmox environments. It includes configuring the Proxmox host and guest machines to ensure proper name resolution for both internal and external networks.  

---

## Problem

The Proxmox host could resolve domain names after configuration, but guest machines were unable to resolve DNS queries due to an incomplete `/etc/resolv.conf` setup.

---

## Solution

Follow the steps below to resolve DNS issues for both the Proxmox host and its guests.

---

## Steps to Configure DNS Resolution

### **1. Configure DNS on Proxmox Host**

1. **Edit the Host's `/etc/resolv.conf` File**
   Open the file:
   ```bash
   sudo nano /etc/resolv.conf
   ```
   Add or replace the following entries:
   ```plaintext
   nameserver 8.8.8.8
   nameserver 1.1.1.1
   ```
   Save and exit.

2. **Make the File Immutable**
   Prevent automatic overwriting of the DNS configuration by making the file immutable:
   ```bash
   sudo chattr +i /etc/resolv.conf
   ```

3. **Verify DNS Resolution**
   Test the configuration by pinging a domain:
   ```bash
   ping www.google.com
   ```

4. **Set Persistent DNS in Proxmox Web Interface**
   - Navigate to **Datacenter → DNS** in the Proxmox web interface.
   - Add valid DNS servers, such as `8.8.8.8` and `1.1.1.1`.

### **2. Configure DNS for Guest Machines**

1. **Edit the Guest's `/etc/resolv.conf` File**
   On the guest machine:
   ```bash
   sudo nano /etc/resolv.conf
   ```
   Add the following lines:
   ```plaintext
   nameserver 10.0.0.1
   nameserver 8.8.8.8
   nameserver 1.1.1.1
   ```
   Save and exit.

2. **Prevent Overwriting of `/etc/resolv.conf`**
   Make the file immutable to persist changes:
   ```bash
   sudo chattr +i /etc/resolv.conf
   ```

3. **Restart Networking**
   Restart the networking service on the guest:
   ```bash
   sudo systemctl restart networking
   ```

4. **Verify DNS Resolution**
   Test the guest machine's DNS resolution:
   ```bash
   ping www.google.com
   nslookup www.google.com
   ```

### **3. Persistent Network DNS Configuration**

For a permanent and scalable solution:
1. **Update the Proxmox Host's Network Configuration**
   Edit `/etc/network/interfaces`:
   ```bash
   sudo nano /etc/network/interfaces
   ```
   Add a `dns-nameservers` directive under the appropriate network interface:
   ```plaintext
   iface vmbr0 inet static
       address 10.0.0.1
       netmask 255.255.255.0
       gateway 10.0.0.254
       dns-nameservers 8.8.8.8 1.1.1.1
   ```
   Save and restart networking:
   ```bash
   sudo systemctl restart networking
   ```

2. **Test Host and Guest DNS Configuration**
   Validate the changes on both host and guest machines:
   ```bash
   ping www.google.com
   nslookup www.google.com
   ```

---

## Conclusion

By configuring both the Proxmox host and its guest machines, the DNS resolution issues were successfully resolved. This documentation ensures a robust and persistent setup for DNS resolution in a Proxmox environment.

