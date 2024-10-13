Here’s a documented summary of the steps you took to install `ntfs-3g` and use `rsync` for copying files with progress tracking:

---

### Documentation: Setting Up and Using NTFS-3G with Rsync

#### Step 1: Install NTFS-3G
To access and manage NTFS partitions on your Proxmox system, you need to install the `ntfs-3g` package. This package allows read and write access to NTFS file systems.

1. **Open the terminal.**
2. **Update the package list:**
   ```bash
   sudo apt-get update
   ```
3. **Install `ntfs-3g`:**
   ```bash
   sudo apt-get install ntfs-3g
   ```

#### Step 2: Mount the NTFS Partition
After installing `ntfs-3g`, you can mount your NTFS partition. Replace `/dev/sdXn` with the actual device identifier for your NTFS partition.

1. **Create a mount point (if not already created):**
   ```bash
   sudo mkdir /mnt/ntfs_storage
   ```

2. **Mount the NTFS partition:**
   ```bash
   sudo mount -t ntfs-3g /dev/sdXn /mnt/ntfs_storage
   ```

#### Step 3: Use Rsync to Copy Files
To copy files from your NTFS partition to the ext4 partition while showing progress, use `rsync`.

1. **Create a mount point for the ext4 partition (if not already created):**
   ```bash
   sudo mkdir /mnt/ext4_storage
   ```

2. **Mount the ext4 partition (replace `/dev/sdXn` with the appropriate identifier):**
   ```bash
   sudo mount /dev/sdXn /mnt/ext4_storage
   ```

3. **Use `rsync` to copy files with progress tracking:**
   ```bash
   sudo rsync -avh --progress /mnt/ntfs_storage/path_to_your_files/ /mnt/ext4_storage/
   ```

   - **Options Explained:**
     - `-a`: Archive mode (preserves permissions, timestamps, etc.)
     - `-v`: Verbose mode (provides detailed information)
     - `-h`: Human-readable output (makes sizes easier to read)
     - `--progress`: Displays progress during the file transfer

#### Step 4: Verify the Copy
After the transfer completes, you can check the contents of the ext4 storage to verify that the files have been copied successfully.

1. **List files in the ext4 storage:**
   ```bash
   ls -l /mnt/ext4_storage
   ```

#### Conclusion
You have successfully installed `ntfs-3g` to manage NTFS partitions and used `rsync` to copy files from an NTFS partition to an ext4 partition while monitoring the progress of the operation.

---

Feel free to modify any part of this documentation as needed!