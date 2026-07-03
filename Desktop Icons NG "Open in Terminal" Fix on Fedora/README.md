# Desktop Icons NG "Open in Terminal" Fix Fedora (44)

This guide fixes the **"No Terminal"** error when using **Desktop Icons NG (DING)** on Fedora Workstation.

It also fixes the issue where **Ptyxis always opens in your Home directory** instead of the selected folder.

## Install `xdg-terminal-exec`

```bash
sudo dnf install xdg-terminal-exec
```

## Configure the default terminal

Create the configuration file:

```bash
mkdir -p ~/.config
printf '%s\n' org.gnome.Ptyxis.desktop > ~/.config/GNOME-xdg-terminals.list
```

> Note: If you use another terminal, replace `org.gnome.Ptyxis.desktop` with its desktop ID.

## Fix Ptyxis opening in the Home directory

Open **Ptyxis → Preferences → Behavior** and:

- Set **Preserve Working Directory** to **Safe** or **Always**
- Click **Set as Default Terminal**

This ensures **Open in Terminal** starts in the selected folder instead of your Home directory.

## Tested on

- Fedora Workstation
- Desktop Icons NG (DING)
- Ptyxis

## Video

Watch the [full tutorial](https://youtube.com/shorts/nuJwljTmRdM) on my [YouTube channel](https://www.youtube.com/@BahRamTech).

## License

This project is licensed under the [MIT License](../LICENSE).