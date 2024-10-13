
---

### **Documenting Proxmox Repository Configuration**

#### **Objective**
To configure Proxmox VE to use the no-subscription repository for updates and package installations, avoiding unauthorized errors from enterprise repositories.

#### **Steps Taken**

1. **Access the Proxmox Terminal**
   - Logged into the Proxmox terminal as the root user.

2. **Check Existing Repository Files**
   - Listed the contents of the sources directory to check for existing repository files:
     ```bash
     ls /etc/apt/sources.list.d/
     ```
   - Found the following files:
     - `ceph.list`
     - `pve-enterprise.list`
     - `pve-no-subscription.list` (added for no-subscription access)

3. **Add the pve-no-subscription.list File**
   - If it wasn't present, created the no-subscription repository file:
     ```bash
     nano /etc/apt/sources.list.d/pve-no-subscription.list
     ```
   - Added the following line to enable the no-subscription repository:
     ```plaintext
     deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
     ```

4. **Modify the pve-enterprise.list File**
   - Opened the enterprise repository file:
     ```bash
     nano /etc/apt/sources.list.d/pve-enterprise.list
     ```
   - Commented out all lines referencing the enterprise repository by adding a `#` at the beginning of each line.

5. **Check and Modify the ceph.list File**
   - Opened the Ceph repository file:
     ```bash
     nano /etc/apt/sources.list.d/ceph.list
     ```
   - Commented out any lines if they existed to avoid unnecessary errors.

6. **Check and Modify the sources.list File**
   - Ensured no enterprise repository entries were present in the main sources list:
     ```bash
     nano /etc/apt/sources.list
     ```

7. **Update Package Lists**
   - Cleared local repository cache to avoid issues:
     ```bash
     apt-get clean
     ```
   - Updated the package lists:
     ```bash
     apt-get update
     ```
   - Verified successful fetching of packages without errors related to unauthorized access.

#### **Expected Outcome**
- Successful output from the `apt-get update` command, showing hits from the Proxmox and Debian repositories, confirming that the no-subscription repository is active.

### **Example Output**
```plaintext
Hit:1 http://security.debian.org bookworm-security InRelease
Hit:2 http://download.proxmox.com/debian/pve bookworm InRelease
Ign:3 http://ftp.debian.org/debian bookworm InRelease
Hit:4 http://ftp.debian.org/debian bookworm-updates InRelease
Hit:3 http://ftp.debian.org/debian bookworm InRelease
Reading package lists... Done
```

### **Conclusion**
By adding the `pve-no-subscription.list` file and commenting out the enterprise repository lines, the Proxmox VE is now properly configured to allow updates without encountering unauthorized access errors.

---
