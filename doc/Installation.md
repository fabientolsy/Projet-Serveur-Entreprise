# Fichier d'istallation du service et des ameliorations

## Installation du service

### Procedure principale

#### Installer Plex

`curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add - `

`echo deb https://downloads.plex.tv/repo/deb public main | sudo tee /etc/apt/sources.list.d/plexmediaserver.list`

`sudo apt install apt-transport-https`

`sudo apt update`

`sudo apt install plexmediaserver`

Verifier que Plex est bien en cours d'execution avec: `sudo systemctl status plexmediaserver`

Plein de lignes apparaissent, regarder que celle-ci y soit: `Active: active (running) since Mon 2018-06-25 10:42:28 PDT;`

#### Configurer Plex

`sudo mkdir -p /opt/plexmedia/{movies,series}`

`sudo chown -R plex: /opt/plexmedia`

##### Source

https://linuxize.com/post/how-to-install-plex-media-server-on-ubuntu-18-04/

### Activer le SSH Tunneling pour devenir admin

#### Pour un client Linux

`/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/Preferences.xml`

`ssh -L 8888:localhost:32400 username@hostname`

#### Pour un client Windows

`/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/Preferences.xml`

Installer le plugin (https://chrome.google.com/webstore/detail/deprecated-secure-shell-a/pnhechapfaindjhompbnflcldabbghjo?hl=en) sur Chrome

Renseigner le nom d'utilisateur et d'hote

Pour SSH Arguments, mettre `-L 8888:localhost:32400`

Laisser le reste par default et faire Entree

##### Source

https://docs.google.com/presentation/d/1wDTeY58-5IFmbO1ffMIZ9vr_AxsJd2XYKwXY2hwsCdU/edit?usp=sharing

## Installation des ameliorations

### Creation de deux librairies dont une de photo avec du contenu

- Aller dans le dossier `opt/plexmedia`
- Y ajouter le nombre de dossier necessaire (un par librairie) avec des noms sementiques (penser a y ajouter du contenut)
- Sur Plex (au niveau du nom du serveur, sur la gauche), cliquer sur le +
- Suivre les indications de Plex

Ps: a la deuxieme etape, choisir le chemin `opt/plexmedia/la_librairie_a_ajouter`

### Augmenter le debit permit

- Se rendre dans les parametres de Plex
- Aller dans `Qualite`
- A `Qualite video`, selectionner maximale
- Descendre en bas de page et sauvegarder

### Installer Tautulli

- `sudo apt-get install git python3.7 python3-setuptools`
- `cd /opt`
- `sudo git clone https://github.com/Tautulli/Tautulli.git`
- `sudo addgroup tautulli && sudo adduser --system --no-create-home tautulli --ingroup tautulli`
- `sudo chown -R tautulli:tautulli /opt/Tautulli`
- `sudo cp /opt/Tautulli/init-scripts/init.systemd /lib/systemd/system/tautulli.service`
- `sudo systemctl daemon-reload && sudo systemctl enable tautulli.service`
- `sudo ufw allow 8181`
- `sudo ufw allow 22`
- `sudo ufw enable`
- `sudo systemctl start tautulli.service`

### Activer et configurer les logs

- `sudo mv '/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/Logs' '/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/Logs.bak'`
- `sudo mkdir '/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/Logs'`
- `sudo chown plex:plex '/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/Logs'`
- `sudo ln -s '/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/Logs' /var/log/plex`
- `sudo chown -R plex:plex /var/log/plex`
- `sudo chmod -R 0755 '/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/Logs'`
- `sudo service plexmediaserver restart`



