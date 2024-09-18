# [Primitives Cryptographiques](https://pageperso.lis-lab.fr/emmanuel.godard/enseignement/tp-de-securite-des-donnees-et-protection-de-la-vie-privee/02_primitives/)
## Nom & Prenom : ZAOUI Zakariae
# Objective
Le but de ce TP TPest de manipuler les primitives cryptographiques classiques au travers des outilsd’OpenSSL sans avoir à reprogrammer les algorithmes cryptographiques correspondants.
# Algorithmes de base
## Encodage
Encodage du prénom et nom de famille en :
- base64
  - commande :
  - ```cmd
    echo "zakariae zaoui" > p_n.txt
    openssl enc -base64 -in p_n.txt
    ```
  - resultat :
  - ![image](https://github.com/user-attachments/assets/cf6693de-9cd4-4247-8342-f9c39444923e)
- binaire
  - commande :
  - ```cmd
    cat p_n.txt | xxd -b > b.bin
    ```
  - resultat :
  - ![image](https://github.com/user-attachments/assets/3790d11a-37db-48b0-935c-e76130488e5d)
- hexadécimal
  - commande :
  - ```cmd
    xxd -p -in p_n.txt
    ```
  - resultat :
  - ![image](https://github.com/user-attachments/assets/11e23ffa-ae97-4880-800a-7b6ad15f7b12)

Ces méthodes ne sont pas des moyens de sécuriser ou chiffrer des données, elles ne font que les représenter sous une autre forme.
- **Hexadécimal** est utilisé pour afficher des données binaires dans un format lisible par les humains exemple des clés cryptographiques.
- **Binaire** est utilisé au niveau des systèmes bas pour représenter toutes sortes de données, fichiers, etc. Toutes les données sont représentées en binaire au plus bas niveau des systèmes informatiques.
- **Base64** Permet d’encoder des données dans des tokens d'authentification JSON Web Tokens (JWT), ou dans les certificats SSL, où la base64 permet d'inclure des données binaires dans des formats texte.
  
## Hachage

Hachage du prénom et nom de famille en :

- MD5
  - commande :
  - ```cmd
    cat cat p_n.txt | openssl dgst -md5 
    ```
  - resultat :
  - ![image](https://github.com/user-attachments/assets/9e11bd33-ff88-4362-8885-a94f91c5944f)
- SHA-1
  - commande :
  - ```cmd
    cat cat p_n.txt | openssl dgst -sha1
    ```
  - resultat :
  - ![image](https://github.com/user-attachments/assets/31af847d-138c-4e08-ae51-c0e2b2242861)
- SHA-256
  - commande :
  - ```cmd
    cat cat p_n.txt | openssl dgst -sha256
    ```
  - resultat :
  - ![image](https://github.com/user-attachments/assets/c2c09c1d-549c-41f2-a58a-43cfb28a9b57)
- RIPEMD-160
  - commande :
  - ```cmd
    cat cat p_n.txt | openssl dgst -ripemd160
    ```
  - resultat :
  - ![image](https://github.com/user-attachments/assets/4f309830-3b3d-46a4-bfd4-783d1575ee4b)

Les fonctions de hachage sont conçues pour transformer un message de taille arbitraire en une empreinte de taille fixe. Elles sont largement utilisées en cryptographie, notamment pour garantir l'intégrité des données.
- **MD5** est utilisé pour vérifier l'intégrité des fichiers, mais déconseillé aujourd’hui pour toute application nécessitant de la sécurité.
- **SHA-1** est utilisé autrefois pour la vérification des signatures numériques et dans les certificats SSL. Cependant, il est désormais déconseillé et a été remplacé par des algorithmes plus sécurisés comme SHA-256.
- **SHA-256** est utilisé dans les certificats SSL, les signatures numériques, la blockchain ...
- **RIPEMD-160** : Bitcoin utilise RIPEMD-160 en combinaison avec SHA-256 pour générer des adresses.

## Chiffrement Symétrique

Chiffrement du prénom et nom de famille, accompagnés d’un texte “secret”, avec chacun des algorithmes ci-dessous. On affichera les résultat en héxadécimale ainsi qu’encodé en base64. On prendra, selon les cas, les paramètres suivants :

Clef:baba,
Vecteur d’Initialisation: ebaba
Phrase de passe: TP Primitives Crypto
Les algorithmes:

- IDEA avec phrase de passe
- AES 192 bits CBC par clé/vecteur
- AES 256 en mode ECB par phrase de passe.
- AES 256 en mode GCM

### IDEA avec phrase de passe

- commande :

  ```cmd
  cat p_n_s.txt | openssl enc -idea -salt -pass pass:"TP Primitives Crypto" -out ciphertext_idea.bin
  ```
- erreur :
  - L'algorithme IDEA (International Data Encryption Algorithm) n'est plus directement supporté par OpenSSL dans ses versions récentes en raison de son brevet initial, maintenant expiré, et de l'adoption croissante d'algorithmes plus performants comme AES. L'utilisation d'IDEA a progressivement diminué, et OpenSSL a choisi de limiter son support pour se concentrer sur des standards modernes.

### AES 192 bits CBC par clé/vecteur

- commande :

  ```cmd
  cat p_n_s.txt  | openssl enc -aes-192-cbc -k baba -iv ebaba | xxd -p
  cat p_n_s.txt  | openssl enc -aes-192-cbc -k baba -iv ebaba | base64
  ```
- resultats :

  ![image](https://github.com/user-attachments/assets/ccc98c18-2edd-41da-914b-63702d44990f)

- Dechiffrement :

# Chiffrement asymétriques

En commençant par la recherche de la clé utilisée, et sachant que celle-ci est dissimulée dans la page, nous avons décidé d'examiner le code source. En explorant le code source de la page, nous avons rapidement identifié un indice
  
```html
<DIV CLASS="body" ID="body">
<!--
<P>
INDICE: le texte est chiffré en AES 256 CFB
     la phrase de passe est
la steganographie est l'art de la dissimulation...
</P>
-->
<P>
Déchiffrer ce qui suit pour obtenir la suite de l&#8217;énoncé.
</P>

<PRE>
U2FsdGVkX18d93nIbpP7GUR1gP8wKwsWvwRzuFjR5Sj6Gq+q65okHROJZYcgU904
BquXTwrCXEq+rlJADU9+lwltGqclq1i9tPc41G02hGSzQ5B6FqpVfZpBgOlm5sUb
TIRA0gyzQPEtptFBOdlO+uQcfKXbWeMM5G8uFUpjspjyyQs8y08LDeGRblUS78Bg
4OuugslCW7kBUz+oQJORKrHrf7aEI0LhsvzpKiN2m+95I+fJMdVcSu8vG/afhUlS
```

maintenant on procede vers decoder et dechiffrer le fichier :

```cmd
base64 -d open_suite.txt > decoded.txt
openssl enc -d -pbkdf2 -aes-256-cfb -in decoded -out out
cat out
```

![image](https://github.com/user-attachments/assets/2df68223-68e0-4ad5-9d21-b1db6c9aa14e)

Après avoir déchiffré le contenu, il est évident qu'il s'agit d'une page HTML. Nous allons maintenant essayer d'accéder à cette page en l'ouvrant directement dans un navigateur

![image](https://github.com/user-attachments/assets/7a9e433d-4007-40a0-8b10-2f4ce5a67d02)

Après avoir réussi à localiser la continuité du TP, nous allons maintenant nous concentrer sur la résolution des prochaines étapes.

## Clefs Publiques/Clefs Privées

on cree notre propre couple clef publique / clef privee :

```cmd
openssl genrsa -aes256 -out p_key.pem
```

La phrase de passe est demandée pour protéger la clé privée. La sécurité de la clé privée dépend de la force de la phrase de passe. Une phrase de passe faible ou facilement devinable réduit considérablement la sécurité, car un attaquant pourrait la retrouver par des attaques par force brute ou dictionnaire.

![image](https://github.com/user-attachments/assets/a2378ff5-6e50-49a7-ad6f-864de760a416)

chiffrement du message suivant avec notre clef publique :

```text
En 2005, la cryptanalyste Xiaoyun Wang et ses co-auteurs Yiqun Lisa
Yin et Hongbo Yu annoncent une attaque théorique pour la recherche de
collisions pour SHA-1 qui est plus efficace que l'attaque des
anniversaires (l'attaque générique de référence)11. L'attaque complète
sera publiée à Crypto 201512. Le NIST pour sa part recommande de
passer sur un algorithme SHA-2 ou SHA-3 quand c'est possible. Depuis
2013, Microsoft a déclaré l’obsolescence du SHA-113 et en
2014. L'ANSSI déconseille son utilisation dans la seconde version de
son Référentiel Général de Sécurité (RGS)14 d'application au 1er
juillet 2014.
```

```cmd
openssl pkeyutl -encrypt -pubin -inkey pub_key.pem -in text.txt -out ciphered.txt
```

Nous avons rencontré une erreur en tentant de déchiffrer le message, car la taille du message dépassait la limite que la clé publique générée pouvait déchiffrer en une seule fois. Pour contourner cette limitation, nous avons décidé de diviser le message en plusieurs blocs

![image](https://github.com/user-attachments/assets/f5e83a64-eaaa-4365-a33b-08ce6e10bf0e)

apres on dechiffre :

```cmd
openssl pkeyutl -decrypt -inkey p_key.pem -in cyphered.txt -out decrypted.txt
```

-------

```cmd
openssl pkeyutl -encrypt -pubin -inkey public.pem -in p_n.txt -out ciphered1.txt
```

![image](https://github.com/user-attachments/assets/1a9b3318-3a95-4882-9b11-635b4b34f8c6)

Pour valider le chiffrement, il est nécessaire de disposer de la clé privée correspondant à la clé publique utilisée lors du processus de chiffrement. Toutefois, cette clé privée n'est pas disponible ici. En général, la vérification du chiffrement est effectuée par la personne qui possède la paire de clés (publique/privée), en déchiffrant les données chiffrées.
