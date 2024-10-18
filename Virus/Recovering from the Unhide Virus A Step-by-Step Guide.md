
---

## Recovering from the Unhide Virus: A Step-by-Step Guide

### Introduction
The **Unhide virus** is a type of malware that typically infects Windows computers and is known for hiding files on infected drives. It can significantly disrupt productivity by making files invisible to the user while retaining them on the system. In this post, I’ll outline the steps I took to recover my computer and external drives from this virus, using **Bitdefender Antivirus** and the **attrib** command.

### Understanding the Unhide Virus
The Unhide virus operates by modifying file attributes to hide them from the user. It commonly spreads via USB drives or downloads, executing scripts that automatically run on system startup. When activated, the virus changes file properties, making them invisible in Windows Explorer, leading users to believe that their files have been deleted or lost. 

### Steps to Recover from the Unhide Virus

#### 1. **Detecting the Virus with Bitdefender**
   - I began by running a full system scan with **Bitdefender Antivirus**, which detected the Unhide virus. 
   - Bitdefender removed the virus automatically, cleaning the affected files and restoring their security.

#### 2. **Restoring Hidden Files Using the `attrib` Command**
   - After the virus removal, I noticed that many files were still hidden. To restore these files, I used the **attrib** command in Command Prompt. Here’s how I did it:

   **Steps to Use the `attrib` Command:**
   1. **Open Command Prompt**:
      - Press **Windows + R**, type `cmd`, and press **Enter**.

   2. **Navigate to the Drive**:
      - Type `X:` (replace `X` with the drive letter where files were hidden) and press **Enter**.

   3. **Run the Attrib Command**:
      - To unhide all files, I executed the following command:
        ```bash
        attrib -h -s /s /d *.*
        ```
      - This command does the following:
        - `-h`: Removes the hidden attribute from files.
        - `-s`: Removes the system attribute from files.
        - `/s`: Applies the command to all files in the current directory and its subdirectories.
        - `/d`: Applies the command to directories as well.

   4. **Verify File Restoration**:
      - After running the command, I checked the drive in Windows Explorer to confirm that my files were now visible.

#### 3. **Preventing Future Infections**
   - To safeguard my system against future threats:
     - I ensured that **Bitdefender** was set to perform regular automatic scans.
     - I avoided plugging in USB drives from untrusted sources and scanned any external media before accessing files.

### Conclusion
Recovering from the Unhide virus required a combination of antivirus detection and manual file restoration using the **attrib** command. By understanding how the virus operates and taking proactive measures, I was able to restore my files and protect my system from future infections.

### Final Note
Always keep your antivirus software updated, and be cautious when handling unknown files or drives. Regular backups are also essential to safeguard important data against potential threats.

---
