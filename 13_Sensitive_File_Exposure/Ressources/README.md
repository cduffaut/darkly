### Comment la faille a été trouvée:

1. Dans le fichier robots.txt que nous avions trouver précédement, il y a un autre dossier sur lequel on ne s'est pas encore penché: http://127.0.0.1:8080//whatever/

2. On y trouve à l'intérieur l'élément *"htpasswd"* (cf. img 1)

3. À l'intérieur du fichier on y trouve un identifiant root >>> *"root:437394baff5aa33daa618be47b75cb49"*
Pour le éot de passe qui serait associé au compte root, il semble qu'il soit chiffré en md5 (https://www.dcode.fr/cipher-identifier)

4. En décodant donc cet élément sur : https://www.md5online.org/md5-decrypt.html
On obtient : qwerty123@ (cf. img 3)

### Comment la faille a été exploitée:

5. Depuis la homepage : http://127.0.0.1:8080/index.php
On clique sur le bouton sing in.
Je rentre ensuite les identifiants: user >>> root / passsword >>> qwerty123@ (cf. img 4)
Mais cela ne fonctionne pas.

6. On va donc essayer de trouver un autre point d'entrée.
Pour des utilisateur root on va se concentrer sur des chemins comme /admin ou /private

7. Bingo, on a bien la page de connection : http://127.0.0.1:8080/admin/ (cf. img 5)
Et le flag apparait !

### Comment la faille peut être corrigée:

- Restreindre l'accès aux fichiers sensibles via .htaccess (fichier de configuration de règles de sécurité, de gestion des accès)

- Si un accès non autorisé est reconnu : le bloquer ou le re-diriger : error 403

- Et de manière générale: stocker les fichiers et données sensibles en dehors du répertoire web.