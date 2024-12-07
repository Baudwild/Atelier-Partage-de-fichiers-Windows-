### 1.Installe le rôle Serveur de fichiers :

- Ouvre le Gestionnaire de serveur
- Clique sur "Gérer" > "Ajouter des rôles et fonctionnalités"
- Choisis "Installation basée sur un rôle ou une fonctionnalité"
- Sélectionne ton serveur
- Dans la liste des rôles, coche "Services de fichiers et de stockage" > "Serveur de fichiers"
- Suis l'assistant pour terminer l'installation

### 2.Crée un dossier "Documents_Entreprise" à la racine du disque C:

Clique droit > nouveau > dossier > renommé "Documents_Entreprise".

### 3.Configure un partage nommé "Docs" pour ce dossier :

- Dans le Gestionnaire de serveur, va dans "Services de fichiers et de stockage" > "Partages"
- Clique droit > "Nouveau partage"
- Choisis "Partage SMB - Rapide"
- Sélectionne l'emplacement du dossier Documents_Entreprise
- Nomme ton partage Docs
- Suivant x3
- Créé 

### 4.Crée trois sous-dossiers : "RH", "Comptabilité" et "Direction" :

- Dans le dossier Documents_Entreprise > clique droit > dossier > renommé (RH, Comptabilité et Direction).

### 5.Configure les permissions NTFS et de partage :

Dans l'AD, création de 3 utilisateurs et de 3 groups + ajout des utilisateurs :
- UserCompta ---> Comptabilité 
- UserDirection ---> Direction 
- UserRH ---> RH

- Sur les dossiers RH, Comptabilité et Direction. Clique droit propriété > sécurité > avancée > désactiver l'héritage > « Convertir les autorisations héritées en autorisations explicites sur cet objet » > Appliquer > Ok.
- Dans sécurité > Modifier > j'ajoute le groupe concerné et les droits.

- Le groupe "RH"---> lecture/écriture sur le dossier "RH"
- Le groupe "Comptabilité"---> lecture/écriture sur le dossier "Comptabilité"
- Le groupe "Direction"---> lecture/écriture sur tous les dossiers

- Modifications des permissions sur le dossier Documents_Entreprise sur:
- les utilisateurs ---> lecture seule au dossier 
- Ajout des 3 groupes an plus.

### 6.Utilise PowerShell pour lister tous les partages sur le serveur :

- Pour voir les partages existants ---> Get-SmbShare
- Pour voir les lecteurs mappés ---> Get-PSDrive -PSProvider FileSystem

### 7.Sur un poste client Windows 10, configure un lecteur réseau pointant vers ce partage via PowerShell

- Pour mapper un lecteur réseau ---> New-PSDrive -Name "E" -PSProvider FileSystem -Root "\\WINSERV1\Docs" -Persist
- Je fais la commande à chaque fois quand je me connecte à un compte.

### 8.Teste l'accès depuis le poste client avec différents comptes utilisateurs :

Sure le compte UserCompta :
- Dossier Comptabilité = OK
- Dossier RH = Pas autorisé 
- Dossier Direction = Pas autorisé 

Sure le compte UserDirection :
- Dossier Comptabilité = OK
- Dossier RH = OK
- Dossier Direction = OK

Sure le compte UserRH :
- Dossier Comptabilité = Pas autorisé
- Dossier RH = OK
- Dossier Direction = Pas autorisé

OK tout fonctionne.
