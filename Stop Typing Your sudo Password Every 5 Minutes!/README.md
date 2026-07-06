# Stop Typing Your sudo Password Every 5 Minutes!

By default, `sudo` asks for your password again after a few minutes. If you're using your own Linux machine, you can configure `sudo` to remember your authentication for your entire login session.

This is especially useful for long-running installations such as Ollama, NVIDIA drivers, Docker, and large package downloads.

## Open the sudoers file

```sh
sudo visudo
```

Or use your preferred editor:

```sh
sudo EDITOR="gnome-text-editor" visudo
# sudo EDITOR="gedit" visudo
```

## Add this line

```text
Defaults    timestamp_timeout=-1
```

Place it below: `Defaults    env_reset`

## Apply the change

Close the current terminal and open a new one. If it doesn't work, log out and log back in to start a new login session.

## Notes

* `timestamp_timeout=-1` keeps your sudo authentication valid until your login session ends.
* This is recommended only for personal computers that you trust.
* If you prefer a timeout instead of unlimited authentication, replace `-1` with the number of minutes you want.

⭐ If this repository helped you, please consider giving it a Star.

## Video

Watch the [full tutorial](https://youtube.com/shorts/Hd53nbsp7XI) on my [YouTube channel](https://www.youtube.com/@BahRamTech).

## License

This project is licensed under the [MIT License](../LICENSE).
