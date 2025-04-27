### Comment la faille a été trouvée:

1. Depuis la page d'acceuil il y a un bouton qui propose de chercher des images (cf. img 1).
Quand on clique dessus on tombe sur un champs de recherche (cf. img 2)

2. Si on tente une injection classique de ce type *"' OR 1=1 --"* : rien ne ce passe.

### Comment la faille a été exploitée:

2. Si dans le champs on tente une  tentative d'injection SQL sanas caractère d'échapatoire : *"1 OR 1=1"* (cf. img 3)
On obtient une liste d'URLs, avec ID et title associé.

3. Parmis les éléments, l'un d'entre eux attire l'oeil : 
*"Title: Hack me ?"*
*"Url : borntosec.ddns.net/images.png"*

4. Les IDs sont tous remplacés par : ID = 1 OR 1=1
Dans MySQL, il existe une base interne spéciale nommée: *"information_schema"*
On teste si si information_schema est accessible:

=> 1 UNION SELECT table_name, NULL FROM information_schema.tables
On obtient une liste détaillé de noms de tables.
On retrouve un table nommé *"list_images"* qui pourrait donc représenter la liste que l'on a affiché precedement.

5. One regarde ensuite les colonne qui compose la table *"list_images"*, pour ce faire on injecte: UNION SELECT table_name, column_name FROM information_schema.columns
On recherche *"list_images"* et on voit une colonne jusqu'a présent non connue: "*comment"*

6. Essayons de voir ce qu'il est mit dans cet colonne pour nos différents éléments. 
Pour cela : 1 UNION SELECT comment, NULL FROM list_images

7. Parmis la liste on a une ligne qui retient notre attention:
Url : If you read this just use this md5 decode lowercase then sha256 to win this flag ! : 1928e8083cf461a51303633093573c46

8. Pour obtenir le flag on doit donc:
- décoder ce hash MD5 => https://crackstation.net/
- mettre le résultat en minuscules
- le hacher avec SHA256 => https://codebeautify.org/sha256-hash-generator

9. 
Resultat:
- albatroz
- pas besoin de changer quoi que ce soit du coup.
- f2a29020ef3132e01dd61df97fd33ec8d7fcd1388cc9601e7db691d17d4d6188
Une fois les étapes realisées on obtient le flag !

### Comment la faille peut être corrigée:

- utiliser des fonctions qui pemette de parser la requete recu afin de la "nettoyé" avant traitement.
- parser les valeurs recues, si differentes du format attendu : tu refuse et ne traite pas la demande.