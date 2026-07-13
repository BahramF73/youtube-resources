# Install IntelliJ IDEA

### Create the installation directory

```bash
sudo mkdir -p /opt/jetbrains
```

---

### Extract IntelliJ IDEA

```bash
sudo tar -xvf idea-*.tar.gz -C /opt/jetbrains
```

---

### Rename the directory

```bash
cd /opt/jetbrains
sudo mv idea-IU-* intellij-idea
```

---

### Verify ownership

```bash
ls -ld /opt/jetbrains
ls -ld /opt/jetbrains/intellij-idea
```

Expected:

```text
drwxr-xr-x  root  root  /opt/jetbrains
drwxr-xr-x  root  root  /opt/jetbrains/intellij-idea
```

---

### Launch IntelliJ IDEA

```bash
/opt/jetbrains/intellij-idea/bin/idea
```
---

### Create a Desktop Entry

After IntelliJ IDEA starts, open **Tools → Create Desktop Entry**.

If you want the launcher to be available for every user on the system, enable **Create entry for all users**.

Otherwise, leave it unchecked to create it only for the current user.

Click **OK**, enter your root password if requested, and you're done.

---

### Try updating IntelliJ IDEA

At this point, the update should fail because the installation directory is owned by `root` and isn't writable by the current user.

---

# Method 1 — Single User

### Make yourself the owner of the installation directory

```bash
sudo chown -R $USER:$USER /opt/jetbrains
```

---

### Verify ownership

```bash
ls -ld /opt/jetbrains
ls -ld /opt/jetbrains/intellij-idea
```

Expected:

```text
drwxr-xr-x  bahram  bahram  /opt/jetbrains
drwxr-xr-x  bahram  bahram  /opt/jetbrains/intellij-idea
```

---

### Try updating IntelliJ IDEA again

This time, the update should complete successfully without using `sudo`.

---

# Method 2 — Shared Linux Group

> This method can be used independently. The installation may remain owned by `root`; members of the `jetbrains` group will receive matching permissions.

### Check regular users

```bash
getent passwd | awk -F: '$3 >= 1000 && $3 <= 60000'
```

### Create the shared group

```bash
sudo groupadd jetbrains
```

### Add users to the group

```bash
sudo usermod -aG jetbrains bahram
sudo usermod -aG jetbrains john
```

> Users must log out and log back in, or reboot, before the new group membership takes effect.

### Assign the installation to the shared group

```bash
sudo chgrp -R jetbrains /opt/jetbrains
```

### Give the group the same permissions as the owner

```bash
sudo chmod -R g=u /opt/jetbrains
```

This gives the group the same permissions as the owner while preserving the original executable bits.

### Enable setgid on all directories

```bash
sudo find /opt/jetbrains -type d -exec chmod g+s {} +
```

This makes newly created files and directories inherit the `jetbrains` group.

### Verify permissions

```bash
ls -ld /opt/jetbrains
```

If the installation is owned by `root`, the expected output is similar to:

```text
drwxrwsr-x  root  jetbrains  /opt/jetbrains
```

If Method 1 was previously applied, the owner may instead be your user:

```text
drwxrwsr-x  bahram  jetbrains  /opt/jetbrains
```

### Verify group membership

```bash
id bahram
```

The output should include:

```text
jetbrains
```
---

### Try updating IntelliJ IDEA again

Members of the `jetbrains` group should now be able to update IntelliJ IDEA without using `sudo`.
---

# Method 3 — ACL

> This method can also be used independently and does not require changing the owner or group of the installation.

### Grant access to specific users on existing files and directories

```bash
sudo setfacl -R -m u:bahram:rwX,u:john:rwX /opt/jetbrains
```

The uppercase `X` adds execute permission only to directories and files that were already executable.

### Set default ACLs for newly created files and directories

```bash
sudo find /opt/jetbrains -type d -exec setfacl -m d:u:bahram:rwx,d:u:john:rwx {} +
```

### Verify ACLs

```bash
getfacl /opt/jetbrains
```

You should see ACL entries for the users you added, along with their corresponding default ACL entries.

---

### Try updating IntelliJ IDEA again

Users with the configured ACL permissions should now be able to update IntelliJ IDEA without using `sudo`.

---

## Optional: Remove ACL for a Specific User

If you want to remove ACL permissions for a specific user, you need to remove both the existing ACL entries and the default ACL entries.

Remove the existing ACL entries from all files and directories:

```bash
sudo setfacl -R -x u:john /opt/jetbrains
```

Then remove the default ACL entries from all directories:

```bash
sudo find /opt/jetbrains -type d -exec setfacl -x d:u:john {} +
```

---

## Optional: Remove All ACL Entries

If you want to completely remove ACLs and return to standard Linux permissions, run:

```bash
sudo setfacl -Rb /opt/jetbrains
```

This recursively removes all access ACLs and default ACLs, leaving only the standard Linux owner, group, and permission bits managed by `chown` and `chmod`.