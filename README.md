yearbook
========

Website and InDesign document for generating a graduation yearbook

### Introduction

Afin d'obtenir un beau Yearbook bien classe, j'avais décidé d'utiliser InDesign en raison de sa surpuissance intergalactique, et aussi du fait qu'on puisse le scripter avec Javascript. Or, faute de temps, je n'ai pas pu aller jusqu'au scriptage. Il y a donc un certain nombre d'étapes à faire à la main. Voilà jusqu'où je suis allé (si quelqu'un veut prendre la suite).

La génération automatique à partir d'une source dans InDesign s'appelle la **Fusion des données**. Le principe est simple : on donne à InDesign un template à appliquer à chaque donnée, et une liste de données.

> **Remarques :**
>
> - Pour afficher les boutons pour faire de la Fusion de données : Fenêtre > Utilitaires > Fusion des données
> - S'il y a des problèmes d'actualisation dans InDesign quand on a modifié le fichier csv : recharger le fichier csv.

#### Génération des pages avec les photos pour un département

Prenons par exemple les étudiants EII. Une donnée serait donc un étudiant, et la liste des données serait la liste des étudiants. Voici les fichiers nécessaires pour générer les pages avec les photos des étudiants en EII :

 - Yearbook/Données/**eii.csv** : la liste des étudiants par ordre alphabétique, avec leurs différentes propriétés : nom, prenom, adresse de la photo, etc.
 - Yearbook/Données/**prenom.nom** : dans le dossier Données se trouvent toutes les photos des étudiants, classées par nom. Il faut donc qu'il y ait dans ce dossier les chemins des photos écrits dans **eii.csv**
 - Yearbook/**yearbook-page-photos-eii-template.indd** : document InDesign contenant le template à appliquer à un étudiant. Chaque champ du document **eii.csv** est relié à un champ dans le template. Par exemple l'adresse de la photo est reliée à un champ d'image qui ira chercher automatiquement la photo à la bonne adresse. InDesign l'indique en mettant le champ comme ceci : `<<nom_du_champ>>`.

Pour générer les pages avec les photos des étudiants en EII :

 1. Ouvrir Yearbook/**yearbook-page-photos-eii-template.indd** avec InDesign.

 2. Afficher l'onglet **Fusion des données**

 3. Cliquer sur **Créer un document fusionné**
 
 4. Régler avec ces paramètres :
     - Dans l'onglet **Enregistrements** :
        - Enregistrements à fusionner : Tous les enregistrements
        - Enregistrements par page de document : Enregistrements multiples
        - Il est possible d'avoir un aperçu rapide en cochant la checkbox, pour s'assurer que ça va fonctionner
     - Dans l'onglet **Mise en page d'enregistrements multiples** :
        - Marges : dépend de votre mise en page, pour la mienne c'est toutes à 20mm (c'est les marges pour la page)
        - Mises en page des enregistrements : dépend de la forme/taille de votre template. Pour le mien : Lignes en premier, 9.9mm entre colonnes et 5mm entre lignes
     - Dans l'onglet : **Options** :
         - Ajustement : remplir les blocs proportionnellement
         - Centrer dans le bloc : oui
         - Lier les images : oui

 5. Cliquer sur **OK**

 6. Un fichier nommé **yearbook-page-photos-eii-template-1.indd** est créé. On pourrait maintenant l'exporter en PDF. Mais on va maintenant l'intégrer dans le livre.

 7. Ce fichier est à ajouter (s'il n'y est pas déjà) dans le livre **yearbook-LIVRE.indb** à l'endroit désiré. Je l'avais mis juste après la page de titre EII (générée automatiquement et à part). 

 
### Prérequis :

- Adobe InDesign CC (trouver un moyen de se le procurer, ou bien utiliser la version d'évaluation 30 jours) : [site officiel](https://creative.adobe.com/fr/products/indesign)
- Python 2.7 (ne **PAS** prendre 3.x pour des soucis de compatibilité) : [site officiel](https://www.python.org/downloads/)
- Programme pour ouvrir des fichiers CSV
	- En mode geek : vim, nano ...
	- OpenOffice Calc : bof un peu lourd
	- Ron's Editor : pour Windows, léger, gratuit, recommandé ! [site officiel](http://www.ronsplace.eu/Products/RonsEditor/Download)
- Police de caractère Roboto : [site officiel](http://developer.android.com/design/style/typography.html)

### Etapes

> **Remarques :**
>
> - Les chemins des fichiers dans les documents **csv** doivent être en **absolu**, c'est-à-dire : **C:\ ... \Yearbook\ ... \monfichier.truc** !
> - Attention au sens des slash ! Sous Windows, les adresses s'écrivent avec des antislash **\ **, et non des slash **/ **

1. Exécuter le script Python **json2csv.py**. Ce script va parser le fichier **etudiants_info.json** et répartir les étudiants selon leur département, dans des fichiers **DEPARTMENT.csv**, avec DEPARTMENT correspondant à chaque département : eii, gcu, gma, info, sgm, src.

2. Ouvrir les fichiers **csv** pour vérifier qu'il n'y a pas d'erreur, ou pour faire des modifications à la main.

3. Executer le script **rename_group_photos.py**

4. Vous devez à ce moment disposer de l'arborescence suivante dans le dossier **Données/** (en gras ce qui va servir dans InDesign) :
        - **groups/** : contient les photos de groupe, nommées **group_i.jpg** avec **i** de 1 à n
        - **photos/** : contient des dossiers **prenom.nom/** qui contiennent tous un fichier **portrait.jpeg**
        - **eii.csv** : liste des etudiants dans le departement eii
        - **gcu.csv** : liste des etudiants dans le departement gcu
        - **gma.csv** : liste des etudiants dans le departement gma
        - **groups.csv** : liste des photos de groupe
        - **info.csv** : liste des etudiants dans le departement info
        - **sgm.csv** : liste des etudiants dans le departement src
        - etudiants_info.json
        - json2csv.py
        - rename_group_photos.py

5. Allez dans le dossier **Resources/** : c'est ici que sont stockées les images autres que les photos des étudiants.

6. Il est possible de modifier les pages d'introduction des chapitres dans InDesign par le biais du fichier **pages-titres-de-sections.csv**

7. Allez dans le dossier **Textes/** : c'est ici qu'il faut modifier les textes pour les mots des directeurs

8. Ouvrir **yearbook-LIVRE.indb** : c'est le fichier InDesign principal (indB avec B comme Book) qui va regrouper toutes les sous-parties qui seront générées par d'autres fichiers InDesign (indD avec D comme Document). Ces sous-parties sont déjà présentes, il ne reste plus qu'à générer chacun d'elles.

9. C'est là qu'il a moyen d'optimiser. En attendant, il faut tout se farcir à la main. Ouvrir (double-clic sur le fichier dans le livre), puis générer comme expliqué plus haut (dans l'ordre ou non) :
    - yearbook-debut.indd
    - yearbook-page-titre-template.indd : qui donnera 6 fichiers, 1 par département
    - Pour chaque département, il y a un fichier yearbook-page-photos-**DEPARTEMENENT**-template.indd, qu'il faut générer dans un fichier yearbook-page-photos-**DEPARTEMENENT**-template-1.indd
    - yearbook-page-titre-groupes.indd
    - yearbook-page-fin.indd

10. Le livre est maintenant prêt à être exporté en PDF. Il faut sélectionner tous les documents du livre et cliquer sur **Exporter Documents sélectionnés au format PDF...** dans le petit menu déroulant dans le bouton en haut à droite de l'onglet Livre.
    
