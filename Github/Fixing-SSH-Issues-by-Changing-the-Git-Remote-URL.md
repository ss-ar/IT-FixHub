Here's a detailed document outlining the process we followed to fix the issue by changing the Git remote URL:

---

## **Fixing SSH Issues by Changing the Git Remote URL**

### **Problem Overview:**
You were experiencing an issue pushing to a GitHub repository via SSH, despite having added your SSH public key to your GitHub account. After verifying the SSH setup, we identified that the remote URL for the repository was using HTTPS instead of SSH, which caused the authentication problem.

### **Solution Process:**

### 1. **Test SSH Connection to GitHub**
   - First, we tested the SSH connection to GitHub to ensure that your SSH key was correctly set up using the following command:
     ```bash
     ssh -T git@github.com
     ```
   - **Result**: The connection was successful, confirming that GitHub could authenticate your SSH key. You received a message similar to:
     ```bash
     Hi username! You've successfully authenticated, but GitHub does not provide shell access.
     ```

### 2. **Check Current Git Remote URL**
   - We checked the current Git remote URL to confirm whether the repository was using HTTPS or SSH for remote communication:
     ```bash
     git remote -v
     ```
   - **Result**: The remote URL was using HTTPS, which requires username/password authentication rather than SSH key-based authentication.

### 3. **Change the Remote URL to SSH**
   - To resolve the issue, we changed the repository’s remote URL to the SSH version:
     ```bash
     git remote set-url origin git@github.com:your-username/your-repo.git
     ```
   - In this command:
     - Replace `your-username` with your GitHub username.
     - Replace `your-repo` with the name of your GitHub repository.
   
   - This updated the URL from HTTPS to SSH, allowing authentication via your SSH key.

### 4. **Verify the Updated Remote URL**
   - After changing the URL, we verified that the remote URL was correctly updated to use SSH:
     ```bash
     git remote -v
     ```
   - **Result**: The output now showed the SSH URL:
     ```bash
     origin  git@github.com:your-username/your-repo.git (fetch)
     origin  git@github.com:your-username/your-repo.git (push)
     ```

### 5. **Push Changes to the Repository**
   - With the remote URL updated to SSH, we attempted to push changes to the repository again:
     ```bash
     git push origin main
     ```
   - **Result**: The push was successful, and the changes were uploaded to GitHub without any further issues.

### **Summary of Commands Used:**
1. **Test SSH Connection**:
   ```bash
   ssh -T git@github.com
   ```

2. **Check Git Remote URL**:
   ```bash
   git remote -v
   ```

3. **Update Git Remote URL to SSH**:
   ```bash
   git remote set-url origin git@github.com:your-username/your-repo.git
   ```

4. **Verify Remote URL**:
   ```bash
   git remote -v
   ```

5. **Push Changes to Repository**:
   ```bash
   git push origin main
   ```

### **Conclusion:**
By changing the remote URL from HTTPS to SSH, the authentication issue was resolved. You can now push and pull changes to the repository securely using your SSH key.

---

This document outlines the process we followed to successfully fix the issue and ensure smooth SSH-based interaction with your GitHub repository. Let me know if you'd like to add anything else!
