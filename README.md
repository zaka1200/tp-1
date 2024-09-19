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
  openssl enc -aes-192-cbc -K 62616261 -iv 6562616261 -in p_n_s.txt -out secret_aes192cbc.enc -base64
  ```
  
- resultats :

  ![image](https://github.com/user-attachments/assets/ccc98c18-2edd-41da-914b-63702d44990f)

- Dechiffrement :

```cmd
openssl enc -aes-192-cbc -d -K 62616261 -iv 6562616261 -in secret_aes192cbc.enc -base64
```

![image](https://github.com/user-attachments/assets/f230bf1b-7a4c-408b-bd68-4c0dffecbdb6)

### AES 256 en mode ECB par phrase de passe

- commande :
  
```cmd
openssl enc -aes-256-ecb -pass pass:"TP Primitives Crypto" -in p_n_s.txt -out secret_aes256ecb.enc
cat secret_aes256ecb.enc | base64
cat secret_aes256ecb.enc | xxd -p
```

- resultats :

![image](https://github.com/user-attachments/assets/b95452b6-7f21-4c7f-8b21-20a19840135e)

- Dechiffrement :

```cmd
openssl enc -d -aes-256-ecb -pass pass:"TP Primitives Crypto" -in secret_aes256ecb.enc 
```

![image](https://github.com/user-attachments/assets/91bdcdeb-cc48-42d4-b515-97d36b7b3e1d)

### AES 256 en mode GCM

Après la mise à jour d'OpenSSL, il a été constaté que le déchiffrement en ligne de commande utilisant AES-256-GCM ne fonctionne plus correctement. Ce problème survient en raison de modifications dans la gestion des algorithmes et des options dans les versions récentes d'OpenSSL. Pour résoudre ce problème, il est possible de revenir à une version antérieure d'OpenSSL

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

## Chiffrement

Chiffrement du nom et prénom avec cette clé RSA :

```rsa
-----BEGIN PUBLIC KEY-----
  MIIBojANBgkqhkiG9w0BAQEFAAOCAY8AMIIBigKCAYEA4rhR/xbm5qX1d9lJCKXe
  BEI85gX1oPqhAS2v3jvGjYktx8MhbvZmke7VNgsmZ1PUdcmnWIzcDeZS4OWYgO7i
  PDz82CNDTH98H+ELk/csLXNGJDpj636ZKyJhub+u/YTfdzSkHKiZ5IRM5xbzeXcG
  rBJGV2f0j36UrK49AGvakM/jPGQXAq1BbEZiGzCuy+d05ZG5Edb83G+zAgiNH6k1
  Vnwc2y+Cp8+WxI0Xxj7LodGF3pOI9xWLvIjetg3RpKHbGdqIGOTxtGjMpgtPo30r
  KBJk9aohEhD0RNs2gc8qXpo8fBbScWcYlxV/p38W1/YhymBebtf6EB736DVquFOh
  hOZPmWX8HbmqFO95M1y/HiVeE7Jav46cPMRysZHAX0KqWfTjeIVI3ZNTDLkFzSDj
  is0oH4mhemW/GX3qidMvHZ5/YcCQR+palo+zQGw5JkV7Y2oEM/cwaD/XM0R5yY8O
  fr0UCmVHwp3+wFuHb0Z5h8Fc/2+rB5c9oAzh4F246ffnAgMBAAE=
-----END PUBLIC KEY-----
```

- commande :

```cmd
openssl pkeyutl -encrypt -pubin -inkey public.pem -in p_n.txt -out ciphered1.txt
```

![image](https://github.com/user-attachments/assets/1a9b3318-3a95-4882-9b11-635b4b34f8c6)

Pour valider le chiffrement, il est nécessaire de disposer de la clé privée correspondant à la clé publique utilisée lors du processus de chiffrement. Toutefois, cette clé privée n'est pas disponible ici. En général, la vérification du chiffrement est effectuée par la personne qui possède la paire de clés (publique/privée), en déchiffrant les données chiffrées.

## Signature

hachage du message m avec SHA-256 :

```cmd
cat message.txt| openssl dgst -sha256
```

Le hachage avec SHA-256 produit une empreinte unique de 256 bits. Ce résultat permet de vérifier l'intégrité du message.

![image](https://github.com/user-attachments/assets/7d4d4d05-5d3d-49e9-8d35-7aa32a3b1e0c)

Chiffrement du message avec une clé publique RSA :

- on commence par enregistrer la clé publique dans un fichier :

```cmd
nano public_key.pem
```

![image](https://github.com/user-attachments/assets/87b4d677-91e7-4112-8705-d6b3b178b249)

- apres on decode le msg :

```cm
cat message.txt | base64 -d > messaged.txt
```

- maintenant on chiffre le msg avec la cle publique RSA :

```cmd
openssl pkeyutl -encrypt -pubin -inkey public.pem -in messaged.txt -out ciphered_d.txt
```

L'affirmation du message M est correcte, mais elle doit préciser les limites de l'utilisation de RSA en termes de taille de message.

## Vers l'Authentification

1 - Le problème central du message M concerne la confiance dans l'authenticité de son contenu et de l'émetteur du message

2 -  A quelle clef publique correspond-t-il ?

```cmd
openssl x509 -pubkey -noout -in cert
```

Cette commande affichera la clé publique du certificat cert

![image](https://github.com/user-attachments/assets/2c9ab8ce-d2fb-4866-b34b-6fc1a1601533)

3 - Qui est l'émetteur du certificat ?

L'émetteur du certificat est indiqué dans le champ issuer. Dans ce cas, l'émetteur est :

![image](https://github.com/user-attachments/assets/c33f2e40-e3c4-42ac-bb45-e4b682a624fb)

Cela signifie que l'autorité de certification (CA) responsable de la délivrance de ce certificat est GEANT TCS 

4 - Qu'est-ce que "GEANT Vereniging" ? Vous paraissent-ils de confiance ? Leur certificat vous parait-il de confiance ?

GEANT Vereniging est une organisation européenne à but non lucratif qui fournit des services de réseau et de sécurité à la communauté académique et de recherche en Europe.

![image](https://github.com/user-attachments/assets/2d23c415-5f55-42eb-867e-6652e8f4e5a2)

Pour déterminer si nous pouvons faire confiance à 'GEANT Vereniging', nous devrions vérifier si cette autorité de certification (AC) est incluse dans notre liste des autorités de confiance.


