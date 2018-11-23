Ce document énumère les différentes étapes à suivre pour livrer le hornet-showroom-online (version fullspa qui est sur github).

1. Récupérer la version de hornet-showroom (la version sur le master de gitlab) en s'assurant avoir tiré les bonnes dépendances.
2. Faire un hb install puis un hb compile sur le hornet-showroom.
3. Faire une recherche dans les nodes_modules de hornet-showroom puis supprimer les liens vers les adresses internes (adresses IP, liens forge.dsinet, liens cognitium etc).
4. Faire un hb prepare-package sur le projet hornet-showroom (Pas nécessaire d'avoir les *.js.map)
5. Remplacer le contenu du dossier static de hornet-showroom-online par celui du static de hornet-showroom généré suite au hb prepare-package.
6. Vérifier dans les chunks que les liens des ressources sont corrects, au besoin corriger (dans les chunks, les ressources sont importées via */hornetshowroom/static-x-x-x/*, remplacer-les par /hornet-showroom-online/static - remplacer les hornet-showroom-online/static/-x.x.x par hornet-showroom-online/static/ - remplacer les static-x.x.x par static) : Vérifier que dans le lien des static, le numéro de version de hornet-showroom ne figure pas.
7  Modifier dans index.html le contextPath "hornetshowroom-spa" en "hornet-showroom-online"
8. Faire un merge de messages-fr-FR.json avec hornet-messages-components du hornet-js-core.
