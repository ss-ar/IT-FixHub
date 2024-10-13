
---

# **Documenting Lid Close Action Configuration in Proxmox**

## **Objective**
To configure Proxmox to ignore the lid close action, preventing the system from suspending when the laptop lid is closed.

## **Step 1: Access the Terminal**
- Log in to your Proxmox server terminal as a root user or a user with sudo privileges.

## **Step 2: Open the Logind Configuration File**
Use a text editor to open the `logind.conf` file. In this example, we will use `nano`:

```bash
sudo nano /etc/systemd/logind.conf
```

## **Step 3: Modify Lid Switch Behavior**
- Find the following lines (or add them if they do not exist):

```plaintext
#HandleLidSwitch=suspend
#HandleLidSwitchDocked=suspend
```

- Change these lines to:

```plaintext
HandleLidSwitch=ignore
HandleLidSwitchDocked=ignore
```

### **Example Configuration:**
Your configuration should look like this:

```plaintext
[Login]
HandleLidSwitch=ignore
HandleLidSwitchDocked=ignore
```

## **Step 4: Save and Exit**
- If using `nano`, save your changes by pressing `CTRL + O`, then hit `Enter` to confirm.
- Exit the editor by pressing `CTRL + X`.

## **Step 5: Restart the Systemd Logind Service**
To apply the changes, restart the `systemd-logind` service with the following command:

```bash
sudo systemctl restart systemd-logind
```

## **Step 6: Verify Changes**
After restarting the service, your Proxmox server should now ignore the lid close action. You can test this by closing the laptop lid and ensuring that the system does not suspend.

---

### **Example Commands Recap**
```bash
# Open the logind configuration file
sudo nano /etc/systemd/logind.conf

# Modify the following lines
HandleLidSwitch=ignore
HandleLidSwitchDocked=ignore

# Restart the systemd logind service
sudo systemctl restart systemd-logind
```

## **Conclusion**
You have successfully configured your Proxmox server to ignore the lid close action. This will prevent the system from suspending when the laptop lid is closed. 

Feel free to refer to this documentation for future configurations or troubleshooting!

--- 
