# Projet cybersecurite - Application de commentaires

Présentation

Dans le cadre de ma première année de BTS SIO, j’ai réalisé un projet autour de la cybersécurité et du développement web. L’objectif était d’analyser les vulnérabilités possibles dans une application web simple et de comprendre comment elles peuvent être exploitées puis corrigées.

L’application utilisée est un petit site permettant aux utilisateurs de poster et consulter des commentaires. À partir de cette base, j’ai pu tester plusieurs attaques web classiques dans un environnement contrôlé.

Fonctionnement de l’application

L’application est volontairement simple afin de se concentrer sur les aspects de sécurité.

La page index.html contient un formulaire permettant d’écrire un commentaire. Les données sont envoyées vers submit.php, qui enregistre le message dans la base de données.

La page comments.php récupère ensuite les commentaires enregistrés et les affiche dans la page.

Ce fonctionnement permet d’illustrer le cycle classique d’une application web :

Formulaire → traitement PHP → base de données → affichage
Base de données

Le projet utilise MySQL / MariaDB.

Une base appelée tp_comments contient une table comments avec :

un identifiant

le contenu du commentaire

la date de création

Un script SQL (init.sql) permet de créer automatiquement la base et la table.

Déploiement

L’application a été déployée sur une machine virtuelle Debian 12 utilisant VMware afin de simuler un environnement serveur.

Un environnement LAMP a été installé :

Apache

PHP

MySQL / MariaDB

Les fichiers du projet ont été placés dans le dossier du serveur web (/var/www/html).
Après la création de la base de données et l’exécution du script SQL, l’application était accessible via Apache dans le navigateur.

Cette étape m’a permis de comprendre le déploiement d’une application web sur un serveur Linux et la configuration des services nécessaires.

Vulnérabilités étudiées

Plusieurs failles de sécurité ont été identifiées dans l’application.

Injection SQL

La requête SQL est construite directement à partir des données utilisateur. Il est donc possible d’injecter du code SQL dans le formulaire et de modifier la requête exécutée par la base de données.

XSS (Cross-Site Scripting)

Les commentaires sont affichés sans filtrage. Un utilisateur peut donc injecter du code JavaScript qui sera exécuté dans le navigateur.

CSRF

Le formulaire ne vérifie pas l’origine des requêtes. Un site externe pourrait donc envoyer une requête POST vers l’application.

MITM

L’application fonctionne en HTTP, ce qui signifie que les données circulent sans chiffrement sur le réseau.

Sécurisation de l’application

Plusieurs corrections ont été mises en place afin de sécuriser l’application.

utilisation de requêtes préparées PDO pour éviter les injections SQL

utilisation de htmlspecialchars() pour empêcher les attaques XSS

ajout d’un jeton CSRF dans le formulaire

limitation de l’affichage des erreurs SQL pour éviter la divulgation d’informations sensibles

Ces modifications permettent de réduire fortement les risques d’exploitation des vulnérabilités présentes dans la version initiale.
