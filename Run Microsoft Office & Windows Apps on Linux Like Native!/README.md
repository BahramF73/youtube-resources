# Run Microsoft Office & Windows Apps on Linux Like Native with WinApps

This repository contains the commands and configuration files used in my YouTube tutorial for installing **WinApps** on **Fedora** using **Podman**. It covers creating a Windows 11 virtual machine, configuring WinApps, sharing folders between Linux and Windows, and automatically adding Microsoft Office and other Windows applications to your Linux desktop.

## Install Podman

```bash
sudo dnf install -y podman podman-compose
```

## Create WinApps Directory

```bash
mkdir -p ~/winapps
cd ~/winapps
```

Create a new `compose.yaml` file and copy the contents of [`compose.yaml`](compose.yaml) from this repository.

## Start Windows

```bash
podman compose up -d
```

Monitor the installation in your browser:

```
http://127.0.0.1:8006
```

## Install WinApps Dependencies

```bash
sudo dnf install -y curl dialog freerdp git iproute libnotify nmap-ncat
```

## Connect to Windows

```bash
xfreerdp /u:USERNAME /p:PASSWORD /v:127.0.0.1:3389 /cert:tofu
```

If the command above doesn't work, try:

```bash
podman unshare --rootless-netns xfreerdp /u:USERNAME /p:PASSWORD /v:127.0.0.1:3389 /cert:tofu
```

## Configure WinApps

Create the configuration directory:

```bash
mkdir -p ~/.config/winapps
```

Create `~/.config/winapps/winapps.conf` and copy the contents of [`winapps.conf`](winapps.conf) from this repository.

Secure the configuration file:

```bash
chown $(whoami):$(whoami) ~/.config/winapps/winapps.conf
chmod 600 ~/.config/winapps/winapps.conf
```

## Install WinApps

```bash
bash <(curl https://raw.githubusercontent.com/winapps-org/winapps/main/setup.sh)
```

Choose:

- Install
- Current User
- Manual

Leave the remaining options as their defaults.

## Share a Linux Folder (Optional)

Add this line to `~/.config/winapps/winapps.conf`:

```bash
RDP_FLAGS="/drive:Office,/mnt/Data/Office"
```

Restart the RDP session:

```bash
pkill -f xfreerdp
```

Inside Windows File Explorer, open:

```
\\tsclient\Office
```

## Add Windows Applications to Linux

After installing Microsoft Office or any other Windows applications inside Windows, run:

```bash
winapps-setup --user --add-apps
```

Select:

- **Set up all detected officially supported applications**

or manually choose the applications you want.

Then select:

- **Set up all detected applications**

Your Windows applications will now appear in your Linux application menu.

## Start WinApps Automatically (Optional)

Generate a systemd service:

```bash
cd ~/winapps

podman generate systemd --new --files --name WinApps

mkdir -p ~/.config/systemd/user
mv container-WinApps.service ~/.config/systemd/user/

systemctl --user daemon-reload
systemctl --user enable --now container-WinApps.service
```

WinApps will now start automatically after you log in.

## Notes

* Tested on **Fedora** using **Podman**.
* KVM virtualization must be enabled.
* SELinux should remain enabled. The provided configuration already uses the `:Z` volume label.
* Most of these steps should also work on other Linux distributions with minor changes.
* If you encounter any issues, feel free to open an issue or leave a comment on the YouTube video.

⭐ If this repository helped you, please consider giving it a Star.

## Video

Watch the [full tutorial](YOUR_YOUTUBE_VIDEO_LINK) on my [YouTube channel](https://www.youtube.com/@BahRamTech).

## License

This project is licensed under the [MIT License](../LICENSE).