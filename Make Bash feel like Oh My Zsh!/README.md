# Make Bash feel like Oh My Zsh!
## Install [Starship](https://github.com/starship/starship)
### 1. Install Starship:  
```sh
curl -sS https://starship.rs/install.sh | sh
```
### 2. Enable Starship:  
```sh
echo 'eval "$(starship init bash)"' >> ~/.bashrc
# or for root user
# echo 'eval "$(starship init bash)"' | sudo tee -a /root/.bashrc
```
### 3. Install [Nerd Font](https://www.nerdfonts.com/):  
for example run this to install [0xProto Font](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/0xProto):
```sh
curl -LO https://github.com/ryanoasis/nerd-fonts/releases/download/v3.4.0/0xProto.zip
unzip 0xProto.zip -d 0xProto
mkdir -p "$HOME/.local/share/fonts" && cp -r 0xProto "$HOME/.local/share/fonts/" && fc-cache -fv && rm -rf 0xProto 0xProto.zip
```
or install [Get Nerd Fonts(getnf)](https://github.com/getnf/getnf) 
## Install [ble.sh](https://github.com/akinomyoga/ble.sh)
### 1. Install ble.sh:
```sh
git clone --recursive --depth 1 --shallow-submodules https://github.com/akinomyoga/ble.sh.git
sudo make -C ble.sh install PREFIX=/usr/local
```
### 2. Enable ble.sh:
```sh
echo 'source -- /usr/local/share/blesh/ble.sh' >> ~/.bashrc
# or for root user
# echo 'source -- /usr/local/share/blesh/ble.sh' | sudo tee -a /root/.bashrc
```
## Video

Watch the [full tutorial](https://youtu.be/0G1MM3gM4HQ) on my [YouTube channel](https://www.youtube.com/@BahRamTech).

## License

This project is licensed under the [MIT License](../LICENSE).