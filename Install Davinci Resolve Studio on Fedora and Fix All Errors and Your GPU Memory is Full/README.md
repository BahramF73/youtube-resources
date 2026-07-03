# Install Davinci Resolve Studio (21.0.1) on Fedora (44) and Fix All Errors and Your GPU Memory is Full

This repository contains the commands used in my YouTube tutorial for installing **DaVinci Resolve Studio 21.0.1** on **Fedora 44**. It also includes fixes for the most common installation issues, Fedora library conflicts, and the **"Your GPU Memory is Full"** error on NVIDIA GPUs.

## Install Dependencies

```bash
sudo dnf install libxcrypt-compat libcurl libcurl-devel mesa-libGLU fuse-libs
```

## Install DaVinci Resolve Studio

```bash
chmod +x DaVinci_Resolve_Studio_21.0.1_Linux.run && SKIP_PACKAGE_CHECK=1 ./DaVinci_Resolve_Studio_21.0.1_Linux.run
```

## Fix Fedora Library Conflicts

Some libraries bundled with DaVinci Resolve conflict with newer Fedora packages. Move them to a separate folder:

```bash
cd /opt/resolve/libs && sudo mkdir disabled-libraries && sudo mv libglib* libgio* libgmodule* disabled-libraries
```

## Force DaVinci Resolve to Use Your NVIDIA GPU (Optional)

If you get a **"Your GPU Memory is Full"** or similar GPU initialization error, run DaVinci Resolve with the NVIDIA OpenGL driver.

### Run once from the terminal

```bash
__GLX_VENDOR_LIBRARY_NAME=nvidia /opt/resolve/bin/resolve %u
```

## Make It Permanent

To always run DaVinci Resolve with the NVIDIA GPU, you need to change the `Exec` line in the DaVinci Resolve desktop files.

You can do it manually or with the command below.

### Method 1: Edit the desktop files manually

Edit these two files:

```bash
/usr/share/applications/com.blackmagicdesign.resolve.desktop
$HOME/Desktop/com.blackmagicdesign.resolve.desktop
```

Find this line:

```bash
Exec=/opt/resolve/bin/resolve %u
```

Replace it with:

```bash
Exec=env __GLX_VENDOR_LIBRARY_NAME=nvidia /opt/resolve/bin/resolve %u
```

### Method 2: Use this command

```bash
sudo sed -i '/^Exec=/ s|^Exec=.*|Exec=env __GLX_VENDOR_LIBRARY_NAME=nvidia /opt/resolve/bin/resolve %u|' /usr/share/applications/com.blackmagicdesign.resolve.desktop "$HOME/Desktop/com.blackmagicdesign.resolve.desktop"
```

## Notes

* Tested on **Fedora 44**.
* Most of these fixes should also work on other Linux distributions with minor changes.
* If you encounter any installation errors or missing dependencies, feel free to open an issue or leave a comment on the YouTube video.

⭐ If this repository helped you, please consider giving it a Star.

## Video

Watch the [full tutorial](https://youtu.be/VdUraKA9yGI) on my [YouTube channel](https://www.youtube.com/@BahRamTech).

## License

This project is licensed under the [MIT License](../LICENSE).
