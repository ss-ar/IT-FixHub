
---

# **Documenting the Creation of a Local User in Proxmox for SSH Access**

## **Objective**
To create a local user in Proxmox and configure SSH access for that user, enabling secure login without using the root account.

## **Step 1: Access the Terminal**
- Log in to your Proxmox server terminal as the root user or a user with sudo privileges.

## **Step 2: Create a New User**
- Use the following command to create a new user. Replace `username` with your desired username:

```bash
sudo adduser username
```

- You will be prompted to set a password and provide optional user information. Fill in the required fields as necessary.

## **Step 3: Add the User to the `sudo` Group**
- To allow the new user to execute administrative commands with `sudo`, add the user to the `sudo` group:

```bash
sudo usermod -aG sudo username
```

## **Step 4: Configure SSH Access**
- **Open the SSH configuration file**:

```bash
sudo nano /etc/ssh/sshd_config
```

- **Ensure the following settings are configured**:
    - **PermitRootLogin**: Set this to `no` to disable root login via SSH:
    
    ```plaintext
    PermitRootLogin no
    ```

    - **AllowUsers**: You can specify the users allowed to log in via SSH. To allow the new user, add:

    ```plaintext
    AllowUsers username
    ```

### **Example Configuration:**
The relevant lines in `sshd_config` should look like this:

```plaintext
PermitRootLogin no
AllowUsers username
```

## **Step 5: Save and Exit**
- If using `nano`, save your changes by pressing `CTRL + O`, then hit `Enter` to confirm.
- Exit the editor by pressing `CTRL + X`.

## **Step 6: Restart the SSH Service**
To apply the changes made to the SSH configuration, restart the SSH service:

```bash
sudo systemctl restart ssh
```

## **Step 7: Test SSH Access**
- Open a new terminal session on a different machine and test SSH access with the new user:

```bash
ssh username@10.0.0.149
```

- Replace `10.0.0.149` with the IP address of your Proxmox server.

## **Example Commands Recap**
```bash
# Create a new user
sudo adduser username

# Add the user to the sudo group
sudo usermod -aG sudo username

# Open the SSH configuration file
sudo nano /etc/ssh/sshd_config

# Configure settings
PermitRootLogin no
AllowUsers username

# Restart the SSH service
sudo systemctl restart ssh
```

## **Conclusion**
You have successfully created a local user and configured SSH access for that user, enabling secure login to your Proxmox server without using the root account.

Feel free to refer to this documentation for future reference or additional configurations!

--- 

