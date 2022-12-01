## Créer un super user

Les commandes suivantes permettent de créer un utilisateur avec hauts privilèges :
```bash
useradd -m nom_utilisateur -p "motdepasse"
usermod -aG sudo user
echo "nom_utilisateur ALL=(ALL)" >> /etc/sudoers.d/nom_utilisateur
```

## Authentification par clé

Pour une connexion plus sécurisée, on peut mettre en place une authentification par clé :
```bash
# Générer la clé
ssh-keygen -b 2048 -t rsa

# Copier la clé vers le VPS
ssh-copy-id -i id_rsa.pub user@ip
```

On modifie `/etc/ssh/sshd_config` pour n'autoriser que l'authentification par clé :
```bash
PermitRootLogin no
PubkeyAuthentication yes
ChallengeResponseAuthentication no

PasswordAuthentication no
PermitEmptyPasswords no

UsePAM yes
```

Enfin, du côté de votre machine, on peut créer le fichier `~/.ssh/config` pour faciliter la connexion au VPS :
```
Host nom
	Hostname ip
	PubKeyAuthentication yes
	IdentityFile ~/.ssh/id_rsa
	User user
```

## Utilitaires pour le terminal

Pour faciliter l'expérience en ssh, on peut installer `Oh My Zsh`, qui permet notamment d'avoir une syntaxe colorée ou encore une meilleure autocomplétion.

Comme son nom l'indique, `Oh My Zsh` nécessite `zsh` :
```bash
sudo apt install zsh
chsh -s /bin/zsh
```

Une fois `zsh` installé, on peut installer `Oh My Zsh` et quelques plugins :
```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

On modifie maintenant `~/.zshrc` pour modifier le thème de base et activer les plugins qu'on vient de télécharger :
```bash
ZSH_THEME="bira"

plugins=(
  git
  zsh-syntax-highlighting
  zsh-autosuggestions
)
```
