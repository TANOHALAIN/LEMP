Ce guide vous fournira toutes les instructions nécessaires pour déployer et configurer un projet LEMP (Linux, Nginx, MariaDB, PHP), utilisant une architecture de containers modernes. Chaque étape, du déploiement initial à la configuration finale des serveurs web et de la base de données, est clairement détaillée pour assurer une mise en œuvre réussie et sécurisée de votre environnement de serveurs.

Pour bien structurer et présenter votre documentation pour le déploiement de votre architecture de conteneurs avec Nginx, WordPress, et MariaDB dans un format Markdown adapté à un README de projet, voici un exemple détaillé :

markdown
Copy code
# Déploiement d'architecture de containers avec WordPress et MariaDB

Ce guide explique comment déployer une architecture de conteneurs comprenant un load balancer Nginx, deux serveurs web Nginx + WordPress, et une base de données MariaDB. Seul le conteneur du load balancer est exposé sur les ports 80 et 443 et agit comme un proxy inversé pour distribuer le trafic vers les serveurs web.

## Architecture

- **Load Balancer (Nginx)**: Répartit le trafic entre les serveurs web.
- **Serveurs Web (Nginx + WordPress)**: Deux conteneurs exécutant Nginx et WordPress.
- **Base de Données (MariaDB)**: Gère les données pour les sites WordPress.

## Déploiement

### Prérequis

- Avoir Vagrant installé sur votre machine.
- Cloner le dépôt de configuration.

### Configuration

1. **Modifier les noms de domaine**:
   Remplacez `votre-nom-de-domaine` par le nom de domaine approprié dans les fichiers suivants :
   - `Load-balancer/lb.conf`
   - `WP/wp.conf`

2. **Lancer le déploiement**:
   Utilisez la commande suivante pour créer et configurer les conteneurs :
   ```bash
   vagrant up
Configuration Post-Déploiement
Base de Données
Sécurisation de MariaDB:
bash
Copy code
vagrant ssh db
sudo mysql_secure_installation
Création de la base de données et des utilisateurs:
bash
Copy code
sudo mariadb -u root
CREATE DATABASE wordpress;
CREATE USER 'wordpress'@'192.168.10.11' IDENTIFIED BY '0000';
CREATE USER 'wordpress'@'192.168.10.12' IDENTIFIED BY '0000';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'192.168.10.11';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'192.168.10.12';
FLUSH PRIVILEGES;
EXIT;
Permettre l'accès distant:
Modifier le fichier de configuration de MariaDB pour permettre l'accès distant.
bash
Copy code
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
# Commenter la ligne: bind-address = 127.0.0.1
sudo systemctl restart mariadb
exit
Installation de WordPress
Accédez à votre site via un navigateur et suivez les instructions d'installation de WordPress.
Modification des fichiers home.html pour chaque instance WordPress
WordPress 1:
bash
Copy code
vagrant ssh w1
sudo nano /var/www/html/wordpress/wp-content/themes/twentytwentyfour/templates/home.html
# Ajouter: <h1 style="text-align:center;">Server 1</h1>
exit
WordPress 2:
bash
Copy code
vagrant ssh w2
sudo nano /var/www/html/wordpress/wp-content/themes/twentytwentyfour/templates/home.html
# Ajouter: <h1 style="text-align:center;">Server 2</h1>
exit
Passage en HTTPS
Configurer HTTPS avec Certbot:
bash
Copy code
vagrant ssh lb
sudo certbot --nginx -d <votre-nom-de-domaine>
sudo systemctl restart nginx
Configuration de SSL Offloading
Modifier wp-config.php pour chaque serveur WordPress pour gérer le SSL offloading.
WordPress 1 et 2:
bash
Copy code
vagrant ssh w1 # ou w2 pour WordPress 2
sudo nano /var/www/html/wordpress/wp-config.php
# Décommenter la section pour activer HTTPS derrière un load balancer
exit
Conclusion
Après avoir suivi ces étapes, votre architecture devrait être pleinement fonctionnelle avec le load balancing, le SSL offloading, et WordPress configuré pour utiliser HTTPS.

arduino
Copy code

Ce README est prêt à être intégré dans votre projet pour guider les utilisateurs à travers l'installation et la configuration de




