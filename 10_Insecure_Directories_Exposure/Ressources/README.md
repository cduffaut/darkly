### Comment la faille a été trouvée:

1. On démarre de la page d'accueil: http://127.0.0.1:8080/, et on examine de nouveau le code source.

2. Dans le HTML, on voit que le site a plusieurs pages accessibles via un système:

?page=survey

?page=member

?page=signin

etc.

3. Mais il pourrait aussi y avoir des dossiers non visibles / cachés qui pourrait être quand même accessibles pour un utilisateur.
Erreur de Logique >>> Le développeur pense qu’en ne mettant aucun lien vers ces dossiers, personne ne les trouvera

4. Et là on peut penser par exemple au fichier: *"robots.txt"*
Un fichier robots.txt, c’est un petit fichier texte que presque tous les sites ont.
Son usage conventionel: Donner des instructions aux moteurs de recherche sur ce qu’ils doivent ou ne doivent pas indexer dans les résultats. Mais nous, rien ne nous empêche d'aller voir.

### Comment la faille a été exploitée:

5. Et on voit s'afficher : 
User-agent: *
Disallow: /whatever
Disallow: /.hidden

6. *"hidden"* attire notre curiosité.

7. On s'y rend : *"http://127.0.0.1:8080//.hidden"*
On tombre sur une liste de lien avec un README (cf. img 1).

8. On se rend compte au'un lien menne à d'autres lien et ainsi de suite. Un vrai labyrinthe qui serait trop lon a faire a la main.
On cherche une methode efficace pour nous aider.

9. Pour se faire, on va se créer un script Python.
On installe d'abord:
pip install requests
pip install beautifulsoup4

10. On code un script très simplifié en Python qui va venir récupérer chaque liens et s'il trouve un README, il va l'ouvrir, lire le contenu, dès que le flag est trouvé il s'arrête et le stocke dans un fichier *"result_scraping"*.

11. Un par un on declenche le script sur chacunes des URLs (afin d'éviter des boucles infinies...) dans *"http://127.0.0.1:8080//.hidden"*.
On met une profondeur de 3 (le maximum que l'on a pu observer en terme de profondeur des liens).
Et on obtient finalement notre flag lors de l'analyse de l'URL: *"http://127.0.0.1:8080/.hidden/whtccjokayshttvxycsvykxcfm/*" (cf. img 2 & 3).

### Comment la faille peut être corrigée:

- Ne jamais mettre de fichiers/dossiers sensibles dans robots.txt
- Si le serveur permet de naviguer dans les répertoires (auto-indexation) il faut le desactiver.
- Protéger les répertoires sensibles par authentification