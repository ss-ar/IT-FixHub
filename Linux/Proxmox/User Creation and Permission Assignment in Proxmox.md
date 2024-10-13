
---

# **Documenting User Creation and Permission Assignment in Proxmox**

## **Step 1: Access Proxmox Terminal**
- Log in to your Proxmox server terminal as the root user.

## **Step 2: Create a New User**
To create a new user, use the following command:

```bash
pveum useradd USERNAME@pve --comment "User Full Name" --password YOUR_PASSWORD
```

**Example:**
```bash
pveum useradd rahim@pve --comment "Rahim User" --password MySecurePassword
```

## **Step 3: Create a User Group**
Before assigning permissions, you can create a user group. This will help manage permissions for multiple users efficiently.

```bash
pveum groupadd admin
```

## **Step 4: Add the User to the Group**
Add the newly created user to the group:

```bash
pveum usermod rahim@pve --group admin
```

## **Step 5: Assign Group Permissions**
Now you can assign permissions to the group. Use the following command to modify the ACL for the group:

```bash
pveum aclmod <path> --add <group@realm> --role <role>
```

**Example:**
Assign the `Administrator` role to the `admin` group at the root level:

```bash
pveum aclmod / --add admin@pve --role PVEAdministrator
```

## **Step 6: Verify the New User and Permissions**
You can verify that the user was created successfully and has the correct permissions:

- **List Users:**
```bash
pveum user list
```

- **List ACLs:**
```bash
pveum acl list /
```

## **Example Commands Recap**
```bash
# Create a new user
pveum useradd rahim@pve --comment "Rahim User" --password MySecurePassword

# Create a new group
pveum groupadd admin

# Add the user to the group
pveum usermod rahim@pve --group admin

# Assign the group permissions
pveum aclmod / --add admin@pve --role PVEAdministrator

# Verify the user and permissions
pveum user list
pveum acl list /
```

## **Conclusion**
You have now successfully created a new user, assigned that user to a group, and set group permissions in Proxmox. This structured approach will help you manage user permissions more effectively and enhance the security of your Proxmox environment.

--- 
