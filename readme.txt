Ce document énumère les différentes étapes à suivre pour livrer le hornet-showroom-online (version fullspa qui est sur github).

1. Récupérer la version de hornet-showroom (la version sur le master de gitlab) en s'assurant avoir tiré les bonnes dépendances.
2. Faire un hb install puis un hb compile et un hb prepare-package sur le hornet-showroom.
3. Remplacer le contenu du dossier static de hornet-showroom-online par celui du static de hornet-showroom généré suite au hb prepare-package.
4. Dans le dossier static de hornet-showroom-online, renommer le dossier hornet-themes-intranet par hornet-themes.
5. Vérifier dans les chunks que les liens des ressources sont corrects, au besoin corriger (dans les chunks, les ressources sont importées via */hornetshowroom/static-x-x-x/*, remplacer-les par /hornet-showroom-online/static - remplacer les hornet-showroom-online/static/-x.x.x par hornet-showroom-online/static/).
6  Modifier dans index.html le contextPath "hornetshowroom-spa" en "hornet-showroom-online"
7. Faire un merge de messages-fr-FR.json avec hornet-messages-components du hornet-js-core.
8. Supprimer les liens vers les adresses IP internes (Page de démonstration).
