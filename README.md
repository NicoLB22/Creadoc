# Spécifications du format de document CREADOC

Le format CREADOC est un format de documents pouvant être utilisé par des applications tierces afin de générer et sauvegarder des documents statiques ou interactifs destinés principalement au monde de l'éducation.

Un document CREADOC peut-être multi-pages et de quelque format que se soit (A4, US Letter ou personnalisé).
Il peut-être créé, ouvert, modifié et enregistré avec l'application Creadoc®, ou toute autre application compatible avec les présentes spécifications.

Un document CREADOC, qu'il soit statique ou interactif, est destiné à être visualisé depuis n'importe quel navigateur web (ordinateur, mobile/tablette ou TBI). Le format d'export HTML généré par l'application Creadoc® présente un lecteur intégré proposant des boutons de navigation et de zoom, une fonction d'impression, la présentation de la licence attribué au document, ainsi qu'une palette d'outils TBI afin de pouvoir travailler sur le document (tableau blanc et noir, crayon, craies, couleurs...).

Un document CREADOC peut également être imprimé ou exporté au formats PDF et image directement depuis son éditeur.

Enfin, un document Creadoc a pour vocation d'être partagé, lu et réalisé (dans le cas de documents interactifs) sur des outils collaboratifs entre professeurs et élèves telle la classe en ligne Creadoc Classroom®
___


## Le fichier CREADOC
Les fichiers CREADOC sont des archives *GZIP* dont les extensions reconnues sont *.gz* et *.creadoc*

## Contenu des fichiers CREADOC
 - Un fichier *"manifest.crea"*
 - Un fichier *"tn_medium.png"* correspondant à la vignette du document
 - Un fichier *"export_print.html"* servant à l'impression du document ainsi qu'à son export en PDF
 - Un fichier *"titre-du-document.html"* correspondant à la version lisible et standalone du document au format HTML
 - Un dossier *"pages"* contenant les fichiers ".edu" correspondants à chacune des pages (un fichier par page)
 - Un dossier *"thumbnails"* contenant les vignettes de chacune des pages
 
## Le manifest
 Le manifest est un fichier au format XML qui doit-être formé comme suit :
 ```
 <creadoc>
	<meta name="params" id="HZpBlY-do10uq-dMqvo3" size="2618" width="794" height="1122" scene-color="default">
	<meta name="generator" content="Creadoc, https://creadocforschool.com">
	<meta name="grades" content="">
	<meta name="description" content="">
	<meta name="owner" content="Nicolas LB">
	<meta name="author" content="encrypted datas">
	<meta name="license" content="CC BY-NC-ND (3.0)">
	<meta name="license_url" content="">
	<meta name="identifier-url" content="https://creadocforschool.com">
	<meta name="date-creation" content="2022-03-14 10:27:35">
	<meta name="date-modification" content="2022-03-14 10:27:35">
	<meta name="creadoc-version" content="1.4.0">
	<meta name="slideshow" content="0">
	<meta name="paging" content="1">
	<meta name="display-score" content="0">
	<meta name="apply-penalty" content="0">
	<meta name="autoplay" content="0">
	<meta name="success-screen" content="0">
	<meta name="creadoc-datas" content="encrypted datas">
	<title>Nouveau document - Import</title>
	<navigation>
		<page id="BMhV78-6PrPWl" file="BMhV78-6PrPWl.edu" tn="BMhV78-6PrPWl.png"></page>
	</navigation>
</creadoc>
```
###### Description des différents paramètres :
**1. Meta *"params"***
- *id* : Identifiant unique du document
- *size* : Taille approximatif du document en octets
- *width* : Largeur d'une page du document en pixels
- *height* : Hauteur d'une page du document en pixels
- *scene-color* : Couleur de la scène (fond en arrière plan des pages) pour ce document. Valeurs possibles : ***'default'*** (valeur par défaut de l'éditeur et du player), ***'Inherit'*** (la scène hérite de la couleur de la page en cours), ***'#xxxxxx'*** (code hexadécimal d'une couleur)

**2. Meta *"generator"***  
Application qui a servie à générer ce document CREADOC.

**3. Meta *"grades"***  
Niveaux scolaires à qui est destiné le document.  
Slugs reconnus: *'early_childhood', 'prek1', 'prek2', 'kindergarten', '1st_grade', '2nd_grade', '3rd_grade', '4th_grade', '5th_grade', 'ULIS', 'SEGPA', 'ITEP', 'IME', 'UPE2A', '6th_grade', '7th_grade', '8th_grade', '9th_grade', '10th_grade', '11th_grade', '12th_grade', 'higher_education', 'adult_education'.*

**4. Meta *"description"***  
Description / objectifs du document.  
Défini par l'auteur.

**5. Meta *"owner"***  
Le propriétaire du document (auteur en cours), sous la forme: ***Prénom Nom(, url)***  
Si l'auteur a renseigné une url de blog ou de site web dans son profil, elle devra être ajoutée à la suite de du prénom et du nom, séparé pr une virgule et un espace.

**6. Meta *"author"***  
Liste des différents auteurs successifs, sous forme de tableau *JSON*, ordonné de l'auteur en cours à l'auteur original.  
Chaque auteur doit être un objet *JSON* sous la forme :
```
{
	"name": "Prénom et Nom de l'auteur",
	"url": "url du site de l'auteur",
	"title": "Titre du document",
	"email": "E-mail de l'auteur",
	"licence": "Licence attribuée au document",
	"licence_url": "url d'informations sur la licence"
}
```
La liste des auteurs se présente donc comme suit :
```
[
	json_auteur2,
	json_auteur1,
	json_auteur0
]
```
Afin de toujours préservé la paternité des auteurs du document, ces données *JSON* sont cryptées au format *AES* avec comme clef l'*id* du document.
Utilisé uniquement dans l'éditeur de documents.
	
**7. Meta *"licence"***  
Licence en cours du document.  
Peut-être différente de la licence originale en cas d'auteurs multiples et sous condition que la licence précédente autorise le changement de celle-ci.  
***Valeurs autorisées :***
*"CC BY (3.0)", "CC BY-SA (3.0)", "CC BY-ND (3.0)", "CC BY-NC (3.0)", "CC BY-NC-SA (3.0)", "CC BY-NC-ND (3.0)", "COMMERCIAL"*

**8. Meta *"licence_url"***  
Lien vers la page de description de la licence. 
Fixé par l'auteur (et obligatoirement présent) en cas de licence commerciale.

**9. Meta *"date-creation"***  
Date de création du document au format ***`"yyyy-mm-dd H:m:s"`***.
	
**10. Meta *"date-modification"***  
Dernière date de modification du document au format ***`"yyyy-mm-dd H:m:s"`***.

**11. Meta *"creadoc-version"***  
Numéro de version du logiciel ayant servi à la dernière édition du document au format ***X.x.x***
	
**12. Meta *"slideshow"*** 
Mode de visualisation du document au format d'export HTML : ***1 = diaporama, 0 = lecture verticale par défilement (type PDF).***

**13. Meta *"paging"***  
Affichage de la pagination en mode export HTML (applicabe qu'en export diaporama) : ***1 = oui, 0 = non.***

**14. Meta *"display-score"***  
Pour les documents interactifs (contenant des exercices interactifs), affichage du score en direct pour le mode d'export HTML : ***1 = oui, 0 = non.***

**15. Meta *"apply-penalty"***  
Pour les documents interactifs, en mode d'export HTML, application (***content="1"***) ou non (***content="0"***) de pénalités en cas de mauvaise réponse.  
Valeur de la pénalité : ***-1 point par mauvaise réponse***, le score ne pouvant cependant être négatif.

**16. Meta *"autoplay"***  
Pour les documents interactifs, en mode d'export HTML, passage automatique à la page suivante si toutes les bonnes réponses de la page ont été trouvées : ***1 = oui, 0 = non.***  
Désactivé automatiquement si la proposition déclenchant le test de validation de l'exercice contient un attribut de navigation renseigné.

**17. Meta *"success-screen"***  
Pour les documents interactifs, en mode d'export HTML, affichage automatique de l'écran standard de félicitations du lecteur en cas de succès à tous les exercices du document.

**18. Meta *"creadoc-datas"***  
Meta pouvant servir à stocker diverses informations et données concernant le document.  
*Pour le moment stocke uniquement le template HTML contenant les informations sur la licence du document et ses auteurs.*  
Données au format *JSON*, cryptées au format *AES* avec comme clef l'*id* du document.
```
{
	"copyright-template": "..."
}
```
Le template est affichés dans l'export HTML (Menu ***"Crédits"***), PDF (au pied de la dernière page) et PNG d'une page (en pied de page).
 
## Les vignettes
 
## Les pages
 
## L'export "print"
 
## L'export HTML
 
## Structure de la page CREADOC (format .edu)
 
## Structure des objets CREADOC
 
**1. Le rectangle**  
**2. Le cercle**  
**3. Le triangle**  
**4. Le trait**  
**5. Les images**  
**6. Les primitives**  
**7. Les flèches**  
**8. Le champ de texte**  
**9. Le tableau**  
**10. Le bouton son**  
**11. Le QRCode**  
**12. Le bouton réponse**  
**13. Le fond de page**  
**14. Les groupes**  
**15. Les objets déplaçables**  
**16. Les Blocs**  
