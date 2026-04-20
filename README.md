# DevOps
Exercice 1 — Premier contact avec Docker

1.3 docker ps
1.4 Une page web qui dit que nginx est bien installé
1.6 docker ps -a (le -a permet de voir le conteneur même s'il est arrêté)
1.8 docker run -d --name mon-nginx -p 8080:80 --rm nginx:alpine ( c'est le --rm qui le fait)

Exercice 2 — Construire sa première image avec un Dockerfile

2.5 mon-site:v1 fait 26mb et nginx:alpine fait 26.9mb
2.6 Un seul layer à était ajouté (COPY index.html)
2.7 L'étape FROM (l'image de base) est reprise du cache, l'étape COPY est réexécutée Docker détecte que le contenu de index.html a changé

Exercice 3 — Volumes et persistance des données

3.1 Non le fichier n'existe plus car le conteneur a était supprimer (--rm) et lors de la création d'un nouveau il ne garde pas les fichiers créers
3.2 En modifiant le fichier directement sur mon PC, la modification est instantanée dans le navigateur
3.5 Le fichier existe toujours, cela montre que les données ont une durée de vie indépendante de celle du conteneur. Le volume survit même si le conteneur est supprimé
3.6 Docker stocke physiquement ce volume à l'intérieur d'un disque virtuel
3.7 Il faut s'assurer qu'aucun conteneur n'utilise ce volume. Sinon, Docker refusera la suppression

Exercice 4 — Réseaux Docker

4.1 efe873779a52   bridge    bridge    local / 4a2ca6d65143   host      host      local / 469e579fe52b   none      null      local
4.4 On récupére le code HTML de la page "Welcome to nginx!", nn peut utiliser le nom serveur-web car Docker possède un serveur DNS interne
4.5 Ca échoue car le conteneur client-externe est sur le réseau bridge par défaut. Docker n'active pas la résolution DNS automatique sur le réseau par défaut pour des raisons de compatibilité et d'isolation
4.6 docker network connect mon-reseau client-externe

Exercice 5 — Containeriser un serveur Flask

5.3 Si vous modifiez votre code (app.py) mais pas vos dépendances, Docker réutilisera le cache pour l'étape pip install 
5.6 La valeur affichée est développement, elle vient de la valeur par défaut définie dans le code Python : os.environ.get("APP_ENV", "développement")
5.7 flask-app:v1    9cb35e58d869        197MB         48.1MB, utiliser une image complète pour compiler/installer les dépendances, puis copier uniquement le résultat (les packages installés) dans une image finale très légère
