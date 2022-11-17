## Sudo sans mot de passe

```bash
useradd -m -s /usr/bin/zsh user -p ""
usermod -aG sudo user
echo "mc ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/mc
```

## Authentification par clé

```bash
ssh-keygen -b 2048 -t rsa
ssh-copy-id -i id_rsa.pub user@ip

ssh mc@ip
sudo nano /etc/ssh/sshd_config
```

`sshd_config`
```bash
PermitRootLogin no
PubkeyAuthentication yes
ChallengeResponseAuthentication no

PasswordAuthentication no
PermitEmptyPasswords no

UsePAM yes
```

`~/.ssh/config`
```
Host nom
	Hostname ip
	PubKeyAuthentication yes
	IdentityFile ~/.ssh/id_rsa
	User user
```

## ZSH

Voir [ZSH](ZSH.md) pour installer et configurer `Oh My Zsh`