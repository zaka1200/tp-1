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

  
