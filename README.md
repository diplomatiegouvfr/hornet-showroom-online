hornet showroom 5.1.0-rc.3
==========================


# Prérequis

* NodeJS 4.0


    $ npm install -g hornet-js-builder

* checkout du projet `hornet-showroom`

# Initialisation

Se positionner dans le répertoire du projet `hornet-showroom` et lancer la commande:

    $ hb install

# Démarrage de l'application en mode développement

## Commande par défaut

la commande à exécuter en mode développement est la suivante:

    $ hb w

Elle permet de lancer l'application en mode `watcher` afin que les modifications soient prises en compte (ce qui
entrainera un redémarrage du serveur node dans le cas d'une détection de modification).

## Options

Il est également possible d'ajouter à cette commande l'option:

    $ hb w -i

Cette commande indique au builder de ne pas transpiler les fichiers typescript en javascript.
Elle est à utiliser dans le cas où l'IDE a été configuré de telle sorte que la transpilation ts->js
se fasse via ce dernier.


# Vérification

Vous pouvez accéder à l'application depuis l'url [http://localhost:8888/hornet-showroom/](http://localhost:8888/hornet-showroom)

# Fichier de configuration de l'application : default.json

L'ensemble de la configuration applicative du serveur NodeJS se situe dans le fichier default.json contenu dans les sources de l'application.

Ce fichier ne doit pas être modifié, excepté pour le log console. Les modifications sont à apporter dans les fichiers d'infrastructure.

## Partie applicative

| Paramètre | Description | Valeur |
|-----------|-------------|--------|
|contextPath| Contexte de l'application déployée|Par défaut vide|
|welcomePage|Page de démarrage de l'application|Passé en paramètre du ServerConfiguration|
|themeUrl|Url du thème CSS|[Protocol]://[host]:[port]/hornet/themeName|

```javascript
{
  "contextPath": "hornet-showroom",
  "welcomePage": "/accueil",
  ...<
}

```


## Configuration des logs serveur

Niveau de log :

| Paramètre | Description | Valeur |
|-----------|-------------|--------|
|level.[all]|Niveau de log pour toute l'application|INFO|
|level.monappli.view|Niveau de log spécifique pour une partie de l'application |optionnel|

```javascript
 "log": {
    "levels": {
      "[all]": "DEBUG",
      "hornet-js-components.table": "TRACE"
    }
    ...
```

Déclaration des appenders :

| Paramètre | Description | Valeur |
|-----------|-------------|--------|
|type|Type d'appender|*file* pour un fichier simple<br/>*dateFile* pour un fichier contenant la date<br/>*console* ...|
|filename| Chemin absolu ou relatif au lancement du fichier de log | /var/log/nodejs/hornet-showroom/hornet-showroom-1.log|
|pattern| Présent pour les types *dateFile* <br />Permet de donner un pattern de date qui sera ajouté au nom du fichier.|-yyyy-MM-dd|
|layout.type| Type d'affichage des messages|pattern|
|layout.pattern| Schéma d'affichage des messages |"%[%d{ISO8601}|%x{tid}|%x{user}|%p|%c|%x{fn}|%m%]"|


Ex: type console

```javascript
"appenders": [
	{
	    "type": "console",
	    "layout": {
	      "type": "pattern",
	      "pattern": "%[%d{ISO8601}|%x{tid}|%x{user}|%p|%c|%x{fn}|%m%]"
	    }
	}
]
```

ex : type fichier

```javascript
"appenders": [
	{
	    "type": "dateFile",
	    "filename": "log/app.log",
	    "layout": {
	      "type": "pattern",
	      "pattern": "%d{ISO8601}|%x{tid}|%x{user}|%p|%c|%x{fn}|%m"
	    }
	}
]
```

## Configuration des logs client

| Paramètre | Description | Valeur |
|-----------|-------------|--------|
|remote|Activation des remotes log|false|
|level|Niveau de log|INFO|

```javascript
  "logClient": {
    "remote": false,
    "level": "TRACE",
    ...
```

## Déclaration des appenders

Type BrowserConsole :


| Paramètre | Description | Valeur |
|-----------|-------------|--------|
|type|Type d'appender|BrowserConsole|
|layout.type| Type d'affichage des messages|THIN/BASIC/pattern/...|
|layout.pattern| Schéma d'affichage des messages |"%p|%c|%m%"|

```javascript
"appenders": [
{
	"type": "BrowserConsole",
	"layout": {
	  "type": "THIN"
	}
}
```

Type Ajax :

| Paramètre | Description | Valeur |
|-----------|-------------|--------|
|type|Type d'appender|Ajax|
|layout.type| Type d'affichage des messages|THIN/BASIC/pattern/...|
|layout.pattern| Schéma d'affichage des messages |"%p|%c|%m%"|
|threshold|Seuil d'envoi des messages de log|100|
|timeout|Timeout d'envoie des messages|3000|
|url|URL d'envoi des logs|/logs|

```javascript
"appenders": [
	{
	    "type": "Ajax",
	    "layout": {
	      "type": "BASIC"
	    },
	    "threshold": 100,
	    "timeout": 3000,
	    "url": "/log"
	}
]
```

## Configuration Apache pour le full SPA

        Alias /hornetshowroom-spa "/var/www/hornet-showroom/static"
        <Directory "/var/www/hornet-showroom/static">
            #Options Indexes FollowSymLinks
            #Require all granted
            Options +FollowSymLinks
            AllowOverride All
            Order deny,allow
            Allow from all
        </Directory>

        RewriteEngine on
        #Redirection du contexte racine vers la page d'accueil
        RewriteRule ^/hornetshowroom-spa/$ /hornetshowroom-spa/accueil [R]
        # Reécrire les routes applicatives (les URI du type /hornetshowroom-spa/route, sans . dans le chemin) pour les renvoyer vers le client js (index.html)
        RewriteRule ^/hornetshowroom-spa/[^\.]*$ /hornetshowroom-spa/index.html [PT]
        # réécrire les images /static/path/to/my/img /path/to/my/img
        RewriteRule ^/hornetshowroom-spa/static/(.*)\.(gif|jpg|png)$ /hornetshowroom-spa/$1.$2 [L,R=301]
