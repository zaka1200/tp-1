# [Primitives Cryptographiques](https://pageperso.lis-lab.fr/emmanuel.godard/enseignement/tp-de-securite-des-donnees-et-protection-de-la-vie-privee/02_primitives/)
## Nom & Prenom : ZAOUI Zakariae
# Objective
Le but de ce TP TPest de manipuler les primitives cryptographiques classiques au travers des outilsd’OpenSSL sans avoir à reprogrammer les algorithmes cryptographiques correspondants.
# Algorithmes de base
## Encodage
Encodage du prénom et nom de famille en :
- base64
  - code :
  - ```cmd
    echo "zakariae zaoui" > p_n.txt
    openssl enc -base64 -in p_n.txt
    ```
  - resultat :
  - ![image](https://github.com/user-attachments/assets/cf6693de-9cd4-4247-8342-f9c39444923e)
- binaire
  - code :
  - ```cmd
    cat p_n.txt | xxd -b > b.bin
    ```
  - resultat :
  - ![image](https://github.com/user-attachments/assets/3790d11a-37db-48b0-935c-e76130488e5d)
- hexadécimal
  - code :
  - ```cmd
    xxd -p -in p_n.txt
    ```
  - resultat :
  - ![image](https://github.com/user-attachments/assets/11e23ffa-ae97-4880-800a-7b6ad15f7b12)

