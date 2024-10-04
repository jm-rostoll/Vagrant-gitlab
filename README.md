
# Déploiement de GitLab sur une Machine Virtuelle Ubuntu avec Vagrant

Ce projet permet de déployer une machine virtuelle Ubuntu à l'aide de Vagrant, puis d'y installer GitLab Community Edition de manière native avec un certificat SSL auto-signé. Cela vous permet de tester GitLab en HTTPS dans un environnement local.

## Prérequis

Avant de commencer, assurez-vous d'avoir installé :

- [Vagrant](https://www.vagrantup.com/downloads)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## Mise en œuvre

1. **Clonez ce dépôt** ou copiez le fichier `Vagrantfile` dans un nouveau répertoire.

2. **Démarrez la machine virtuelle** en exécutant la commande suivante dans votre terminal :

   ```sh
   vagrant up
   ```

3. Vagrant va automatiquement :

   - Créer une machine virtuelle Ubuntu.
   - Mettre à jour le système.
   - Installer GitLab Community Edition.
   - Générer un certificat SSL auto-signé.
   - Configurer GitLab pour utiliser HTTPS avec le certificat.

4. Une fois le processus terminé, vous pouvez accéder à GitLab en ouvrant votre navigateur et en entrant l'adresse suivante :

   ```
   https://localhost:8080
   ```

   Note : Vous recevrez un avertissement de sécurité dans votre navigateur à cause du certificat auto-signé. Vous pouvez l'ignorer pour continuer vers l'interface GitLab.

## Paramètres du `Vagrantfile`

- **Image Ubuntu** : Utilisation de `"ubuntu/focal64"` (Ubuntu 20.04).
- **Ressources de la VM** :
  - Mémoire : 4096 Mo (modifiable dans `vb.memory`).
  - CPUs : 2 (modifiable dans `vb.cpus`).
- **Certificat SSL** :
  - Le certificat et la clé privée sont créés dans le dossier `/etc/gitlab/ssl`.
  - Le certificat est valable pour un an.
- **Accès à GitLab** :
  - Le serveur GitLab est configuré pour être accessible via HTTPS sur `https://gitlab.example.com`.
  - Redirection automatique de HTTP vers HTTPS.

## Gestion de la Machine Virtuelle

- **Arrêter la machine virtuelle** :

  ```sh
  vagrant halt
  ```

- **Redémarrer la machine virtuelle** :

  ```sh
  vagrant up
  ```

- **Se connecter à la machine virtuelle via SSH** :

  ```sh
  vagrant ssh
  ```

## Gestion de GitLab

GitLab est géré à l'aide de la commande `gitlab-ctl`. Voici les commandes courantes :

- **Reconfigurer GitLab** (utile après modification de la configuration) :

  ```sh
  sudo gitlab-ctl reconfigure
  ```

- **Redémarrer GitLab** :

  ```sh
  sudo gitlab-ctl restart
  ```

- **Vérifier l'état de GitLab** :

  ```sh
  sudo gitlab-ctl status
  ```

## Détails sur le Certificat SSL Auto-signé

Un certificat SSL auto-signé est généré pour sécuriser les communications entre votre navigateur et le serveur GitLab. Voici quelques points à noter :

- **Emplacement des fichiers** :
  - Le certificat est stocké à `/etc/gitlab/ssl/gitlab.example.com.crt`.
  - La clé privée est stockée à `/etc/gitlab/ssl/gitlab.example.com.key`.
- **Configuration** :
  - La configuration de GitLab a été modifiée pour rediriger les requêtes HTTP vers HTTPS.
  - Vous recevrez un avertissement de sécurité en raison du caractère auto-signé du certificat.

## Problèmes Courants

- **Problème de sécurité dans le navigateur** : Comme il s'agit d'un certificat auto-signé, votre navigateur affichera un avertissement de sécurité lorsque vous tenterez d'accéder à GitLab. Vous devrez accepter cet avertissement pour continuer.

- **Ports utilisés** : 
  - GitLab utilise le port 443 pour HTTPS et le port 80 pour rediriger le trafic HTTP.
  - Assurez-vous que les ports ne sont pas déjà utilisés par d'autres applications sur votre système.

## Personnalisation

- **Nom de domaine** : Par défaut, le certificat est généré pour `gitlab.example.com`. Vous pouvez changer cette valeur en éditant le `Vagrantfile` et en mettant à jour le champ `-subj` dans la commande OpenSSL.

- **Ressources VM** : Si votre hôte dispose de plus de mémoire ou de CPU, vous pouvez ajuster `vb.memory` et `vb.cpus` dans le `Vagrantfile` pour une meilleure performance.

## Support

Pour toute question ou assistance, veuillez ouvrir une issue sur ce dépôt.

## Auteurs

Créé par [Votre Nom].
