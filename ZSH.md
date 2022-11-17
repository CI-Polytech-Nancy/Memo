## Installer zsh et le configurer

```bash
sudo apt install zsh
chsh -s /bin/zsh
```

## Installer `Oh My Zsh` et le configurer

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

```bash
ZSH_THEME="bira"

plugins=(
  git
  zsh-syntax-highlighting
  zsh-autosuggestions
)
```

