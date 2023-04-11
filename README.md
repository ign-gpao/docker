# Docker

Module permettant le déploiement de l'ensemble des briques de la GPAO via Docker

## Procédure

### Installation de Docker Engine sous Ubuntu

Suivre ces deux tutos :

1. https://docs.docker.com/engine/install/ubuntu/
2. https://docs.docker.com/engine/install/linux-postinstall/

/!\ Pour gérer le proxy de l'IGN et les erreurs du type `Error response from daemon` :

* Créer un fichier `/home/"$USER"/.docker/config.json` pour le compte admin et le compte normal et écrire dedans :
``` shell
{
    "proxies": {
        "default": {
            "httpProxy": "http://proxy.ign.fr:port",
            "httpsProxy": "http://proxy.ign.fr:port",
            "noProxy": "api-gpao,localhost,127.0.0.1,.ign.fr"
        }
    }
}
```

* Créer un fichier   `/etc/systemd/system/docker.service.d/proxy.conf` et écrire dedans :
``` shell
[Service]
Environment="HTTP_PROXY=http://proxy.ign.fr:port"
Environment="HTTPS_PROXY=http://proxy.ign.fr:port"
Environment="NO_PROXY="localhost,127.0.0.1,::1
```

Un `systemctl restart docker` voire  un reboot la machine sera peut-être nécessaire pour les prendre en compte.

Vérifier avec un `docker info` que les variables proxy sont enregistrées.

### Déploiement de la GPAO

Cloner le dépôt ign-gpao/docker et dans un terminal lancer la commande suivante :
``` shell
SERVER_HOSTNAME=localhost docker compose up --scale client-gpao=0
```

Les images Dockers sont téléchargées sur le [DockerHub de la GPAO](https://hub.docker.com/u/gpao) et les conteneurs database-gpao, api-gpao et monitor-gpao sont lancés automatiquement via le fichier `docker-compose.yml`. Le monitor est alors accessible à l'adresse `localhost:8000/` et le client est à lancer à part.

## Licence

Ce projet est sous licence CECILL-B (voir [LICENSE.md](https://github.com/ign-gpao/.github/blob/main/LICENSE.md)).

[![IGN](https://github.com/ign-gpao/.github/blob/main/images/logo_ign.png)](https://www.ign.fr)
