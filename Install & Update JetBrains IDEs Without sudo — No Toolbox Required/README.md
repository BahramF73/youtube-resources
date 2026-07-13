# Install & Update JetBrains IDEs Without sudo — No Toolbox Required

This repository contains the commands used in my YouTube tutorial for installing **IntelliJ IDEA** and other **JetBrains IDEs** under **`/opt`** without using **JetBrains Toolbox**.

Instead of installing a separate copy for every user, you'll learn how to keep a single system-wide installation and update it without using `sudo`.

The guide also covers three different permission management methods:

- **Single User**
- **Shared Linux Group**
- **Access Control Lists (ACL)**

These methods work for IntelliJ IDEA as well as other JetBrains IDEs such as **PyCharm**, **WebStorm**, **CLion**, **GoLand**, **DataGrip**, **Rider**, **PhpStorm**, **RubyMine**, and more.

## Install IntelliJ IDEA

### Create the installation directory

```bash
sudo mkdir -p /opt/jetbrains
```

### Extract IntelliJ IDEA

```bash
sudo tar -xvf idea-*.tar.gz -C /opt/jetbrains
```

### Rename the directory

```bash
cd /opt/jetbrains
sudo mv idea-IU-* intellij-idea
```

### Verify ownership

```bash
ls -ld /opt/jetbrains
ls -ld /opt/jetbrains/intellij-idea
```

### Launch IntelliJ IDEA

```bash
/opt/jetbrains/intellij-idea/bin/idea
```

### Create a Desktop Entry

Open:

**Tools → Create Desktop Entry**

Enable **Create entry for all users** if you want the launcher available for every user.

### Try Updating IntelliJ IDEA

At this point, the update should fail because the installation directory is owned by `root`.

---

## Method 1 — Single User

### Make yourself the owner of the installation directory

```bash
sudo chown -R $USER:$USER /opt/jetbrains
```

### Verify ownership

```bash
ls -ld /opt/jetbrains
ls -ld /opt/jetbrains/intellij-idea
```

### Try updating IntelliJ IDEA again

The update should now complete successfully without using `sudo`.

---

## Method 2 — Shared Linux Group

### Check regular users

```bash
getent passwd | awk -F: '$3 >= 1000 && $3 <= 60000'
```

### Create a shared group

```bash
sudo groupadd jetbrains
```

### Add users

> Replace `USER1` and `USER2` with your actual Linux usernames.

```bash
sudo usermod -aG jetbrains USER1
sudo usermod -aG jetbrains USER2
```

> Log out and log back in (or reboot) before continuing.

### Change the group owner

```bash
sudo chgrp -R jetbrains /opt/jetbrains
```

### Give the group the same permissions as the owner

```bash
sudo chmod -R g=u /opt/jetbrains
```

### Enable setgid

```bash
sudo find /opt/jetbrains -type d -exec chmod g+s {} +
```

### Verify permissions

```bash
ls -ld /opt/jetbrains
```

### Verify group membership

> Replace `USER1` with your actual Linux username.

```bash
id USER1
```

Members of the `jetbrains` group should now be able to update IntelliJ IDEA without using `sudo`.

---

## Method 3 — ACL


### Grant access to specific users

> Replace `USER1` and `USER2` with your actual Linux usernames.

```bash
sudo setfacl -R -m u:USER1:rwX,u:USER2:rwX /opt/jetbrains
```

### Configure default ACLs

> Replace `USER1` and `USER2` with your actual Linux usernames.

```bash
sudo find /opt/jetbrains -type d -exec setfacl -m d:u:USER1:rwx,d:u:USER2:rwx {} +
```

### Verify ACLs

```bash
getfacl /opt/jetbrains
```

Users with the configured ACL permissions should now be able to update IntelliJ IDEA without using `sudo`.

---

## Remove ACL for a Specific User (Optional)

Remove existing ACL entries:

> Replace `USER2` with the username you want to remove.

```bash
sudo setfacl -R -x u:USER2 /opt/jetbrains
```

Remove default ACL entries:

> Replace `USER2` with the username you want to remove.

```bash
sudo find /opt/jetbrains -type d -exec setfacl -x d:u:USER2 {} +
```

---

## Remove All ACL Entries (Optional)

```bash
sudo setfacl -Rb /opt/jetbrains
```

This removes all access ACLs and default ACLs, restoring standard Linux permissions.

---

## Notes

- A single installation can be shared by multiple users.
- No JetBrains Toolbox is required.
- Method 1 is recommended for personal computers.
- Method 2 is recommended for shared systems.
- Method 3 is useful when only specific users should have update permissions.
- These methods work for most JetBrains IDEs.

⭐ If this repository helped you, please consider giving it a Star.

## Video

Watch the [full tutorial](https://youtu.be/jtPcKxrHeww) on my [YouTube channel](https://www.youtube.com/@BahRamTech).

## License

This project is licensed under the [MIT License](../LICENSE).