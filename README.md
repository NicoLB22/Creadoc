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
Afin de toujours préserver la paternité des auteurs du document, ces données *JSON* sont cryptées au format *AES* avec comme clef l'*id* du document.
Utilisé de manière fonctionnelle uniquement dans l'éditeur de documents, sa validité est testée lors de l'import de documents.  
Pour des raisons de paternité de document et de respect des licences attribuées, sa présence et sa validité doivent tout de même être testées dans les lecteurs HTML.
	
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
**Pour le moment stocke uniquement le template HTML contenant les informations sur la licence du document et ses auteurs.**  
Données au format *JSON*, cryptées au format *AES* avec comme clef l'*id* du document.
```
{
	"copyright-template": "..."
}
```
Le template est affichés dans l'export HTML (Menu ***"Crédits"***), PDF (au pied de la dernière page) et PNG d'une page (en pied de page).  
Afin d'éviter la possibilité de suppression manuelle de cette méta par l'utilisateur, ou la suppression de sa valeur, celle-ci doit-être également présente même si le document ne comporte pas de licence, la valeur de ***copyright_template*** sera alors égale à l'***id*** du document.  
La présence de cette méta et sa valeur doivent donc également être testées dans le lecteur de l'export HTML.

## La balise *title*
La balise ***title*** contient le titre du document qui sera utilisé comme nom des fichiers d'exports (en enlevant les caractères spéciaux, accentués et les espaces), et sera affiché dans l'onglet du navigateur web servant à afficher la version HTML.

## La section *navigation*
La section ***navigation*** contient la liste des pages du document, ordonnée de la première à la dernière.  
Chaque item ***page*** contenu dans cette balise représentera une page unique et indiquera son identifiant (***id***), le nom de son fichier source (***file***) ainsi que le nom du fichier image (*PNG*) servant de vignette au document (***tn***).

## Les vignettes
Le document doit présenter une vignette au format PNG à la racine de l'archive GZ et intitulée tn_medium.png.  
Chaque page doit également posséder une vignette au format PNG. Celles-ci doivent-être placées dans le dossier *"thumbnails"*.  
Les vignettes doivent avoir une largeur de 332 pixels pour un document en mode paysage, et de 235 pixels en mode portrait. La hauteur est quand à elle variable et est calculée par l'éditeur afin de conserver l'aspect visuel du document sans modifier ses proportions.

## Les pages
Les différentes pages du document sont stockées dans des fichiers individuels placés dans le dossier *"pages"*.  
Ces fichiers doivent être au format ***".EDU"***, format dérivé du XML et reprenant une structuration de balises compatibles au format HTML mais selon une structuration précise et une norme précise (voir plus bas).

## L'export "print"
Le fichier ***"export_print.html"*** est un export autonome du document compatible HTML.  
Dans Creadoc®, il sert aux fonctions d'impression du document et à son export au format PDF.  
À ce titre, il doit contenir tous les formatages CSS nécessaires à la bonne mise en page et au respect des dimensions du document lors de son impression.

## L'export HTML
Le fichier ***"nom-du-document.html*** est l'export autonome du document au format HTML.  
C'est ce fichier qui sera exporté et partagé par les enseignants afin de consulter les documents dans un navigateur web et dans réaliser les éventuels exercices interactifs.  

###### Ce fichier doit contenir :
- Le *manifest* explicité plus haut.  
- L'ensemble des styles CSS nécessaires à l'affichage du document et de ses éléments sous forme de pages, ainsi que des éléments constitutifs du lecteur HTML
- Le code Javascript nécessaire à l'interprétation du format CREADOC et à la mise en œuvre de ses fonctionnalités, et permettant le fonctionnement du lecteur HTML  
- L'ensemble des polices de caractères nécessaire au bon affichage du document et du lecteur
- Les éléments HTML constitutifs de l'IHM du lecteur
- L'ensemble du code EDU des pages du document à l'intérieur d'une balise HTML conteneur et enchaînées de la première à la dernière.


## Structure de la page CREADOC (format .edu)
Le fichier EDU contient les éléments constitutifs d'une page d'un document CREADOC au format HTML.  
Exemple de page EDU :

```
<section class="page inherit" data-type="page" id="xZ7e71-Vg9Cbt" data-scale="1" data-title="" data-player-color="Inherit" data-scene-color="Inherit" data-transition-type="Inherit" data-slide-pause="Inherit" data-visible="1" data-cascade="Inherit" data-width="794" data-height="1122" style="width: 794px; height: 1122px; background-color: rgba(255, 255, 255, 0); position: relative; left: 0px; top: 0px; display: block;">
	<!--2p3S2hndtNEV4pnb4BDSNZVQhVmRpZlcTNlW2N2aap1TxYzaYNTV0cGSNRlbL9WNWZnZ3gzZLhEOMtSeFZ3LCBXOFFVavxmVkR1UHBld4JzLS5UW3U1UHB3b3cUVjZUYMd3Q1BnWyIFdBpUNTx0ayQWVzM0Q2Z2aONWVyJ3bEhnVaZ2UBR2ciZXdNl3NLREUPxUOapnNBRGWwZFbmtGdqtCVLZjbuRmRyVnc0N2M5F3M0RTZwEFWx9kRuBjR6BHNThDNOdjSpZENClWdoZ1SQFFM5FEehlGa5hTNkljWih0ZnJnavhnRJp1LYVFd1lDZYFXM4l3StdVeXRmbwZTc0IlWwFETJhGZGV1asNEMpB1aVpkNMRmZ1kUezZlcEJDTxxWTTxWZvE1LwIlVBRkSh5WaOhFUoZVd4RWR1N1b2gjdBhWeZpEcSZXR4wmM4M3Tql1U0FHVWJWSld3V1cTOFdUNzEEOWRnR342QpJVRph0MspXMxpUdl1GdzJWa1VWc6FVOstmQnRXTChFTLhXYZdmMvAVYx0WeqVHMvpEZPZ2YzRHUvhnYIlUYUdHZIFje0cHM5pHTsRTWRRWQxw0YtREUvE2VlR0SWBlN3hnUrgDaBBDR5RHal9mcGxWb5k1SwhEMDFzQSZnbCZFMvFFeKBzNwpFT21kc5g3dlVncFJ3QIx0dMp2Y0gmRwwEWypGNlRFcvNTeNRzaI9UeXRTTiJEdIh1b2MUb4gWUDF1L2UkZYlzUDhTMYtmVHR2cGJTV-->
	<div id="9HoG2Y-ke6Zzl-7w8MkA" class="crea-background" data-type="background" data-locked="1" data-zoom="1" data-ratio="0" data-minwidth="20" data-minheight="20" border-id="432" style="opacity:1; width:794px; height:1122px; transform:matrix(1, 0, 0, 1, 0, 0)"><div class="crea-wrapper"><svg preserveAspectRatio="none" class="crea-svg" viewBox="0 0 794 1122"><rect vector-effect="non-scaling-stroke" x="0" y="0" rx="0" ry="0" width="794" height="1122" style="fill:#ffffff; fill-opacity:1; stroke:#000000; stroke-width:0; stroke-linejoin:miter; stroke-linecap:butt; stroke-opacity:0;"></rect></svg><div class="crea-nested-image" style="display:none;"><svg preserveAspectRatio="none" viewBox="0 0 794 1122"><defs><clipPath id="PtPT5-gpBJe"><rect x="0" y="0" rx="0" ry="0" width="794" height="1122" style="fill:#2196f3; stroke:none; stroke-linejoin:miter; stroke-linecap:butt;"></rect></clipPath></defs></svg><img class="nested-image" style="display:none; padding-left:20px; padding-top:20px; padding-right:20px; padding-bottom:20px; object-fit:none; object-position:left top; image-rendering:auto; clip-path:url(#PtPT5-gpBJe); -webkit-clip-path: url(#PtPT5-gpBJe);"><div class="nested-pattern" style="display:none; padding-left:0px; padding-top:0px; padding-right:0px; padding-bottom:0px; background-repeat:repeat; clip-path:url(#PtPT5-gpBJe); -webkit-clip-path: url(#PtPT5-gpBJe);"></div></div><svg preserveAspectRatio="none" class="crea-svg" viewBox="0 0 794 1122"><rect vector-effect="non-scaling-stroke" x="0" y="0" rx="0" ry="0" width="794" height="1122" style="fill:none; stroke:none; stroke-width:0; stroke-opacity:1; stroke-dasharray:0,0; stroke-linejoin:miter; stroke-linecap:butt;"></rect></svg><div class="crea-border">&nbsp;</div></div></div>
	<div id="wt3GDY-eXfVzS-h1PO77" class="crea-element" data-type="text" data-locked="0" data-zoom="1" data-ratio="1" data-minwidth="20" data-minheight="20" border-id="432" style="opacity: 1; width: 300px; height: 49px; transform: matrix(1, 0, 0, 1, 247, 53);" text-style="custom"><div class="crea-wrapper"><div class="crea-text" style="padding: 10px; visibility: visible; border-width: 2px; border-style: solid; border-color: rgb(0, 0, 0); background-color: rgb(221, 221, 221);"><div style="text-align:center;"><strong><span style="font-size: 18px; font-family: Montserrat;">Le corps humain</span></strong></div></div></div></div>
	<div id="X1olxQ-m8zpsq-5DWFhx" class="crea-element cible proposition crea-button" data-type="text" data-locked="0" data-zoom="1" data-ratio="1" data-minwidth="20" data-minheight="20" border-id="432" style="opacity: 1; width: 193px; height: 42px; transform: matrix(1, 0, 0, 1, 39, 837);" text-style="style_6" data-sound="correct"><div class="crea-wrapper"><div class="crea-text" style="padding: 10px; visibility: visible; background-color: rgb(221, 255, 255); border-top: none; border-right: none; border-bottom: none; border-left: 6px solid rgb(33, 150, 243); border-radius: 0px;"><div style="text-align:center;"><strong><span style="font-size:16px; font-family:Montserrat;">DENTS</span></strong></div></div></div></div>
	<div id="JCJALC-75I8fy-fPWiCm" class="crea-element cible proposition crea-button" data-type="text" data-locked="0" data-zoom="1" data-ratio="1" data-minwidth="20" data-minheight="20" border-id="432" style="opacity: 1; width: 193px; height: 42px; transform: matrix(1, 0, 0, 1, 301, 837);" text-style="style_7" data-sound="wrong"><div class="crea-wrapper"><div class="crea-text" style="padding: 10px; visibility: visible; background-color: rgb(251, 235, 235); border-top: none; border-right: none; border-bottom: none; border-left: 6px solid rgb(215, 70, 81); border-radius: 0px;"><div style="text-align:center;"><strong><span style="font-size:16px; font-family:Montserrat;">OREILLE</span></strong></div></div></div></div>
	<div id="0y36Ir-N8RCLl-uNqzrZ" class="crea-element cible proposition crea-button" data-type="text" data-locked="0" data-zoom="1" data-ratio="1" data-minwidth="20" data-minheight="20" border-id="432" style="opacity: 1; width: 193px; height: 42px; transform: matrix(1, 0, 0, 1, 547, 837);" text-style="style_5" data-sound="wrong"><div class="crea-wrapper"><div class="crea-text" style="padding: 10px; visibility: visible; background-color: rgb(255, 247, 235); border-top: none; border-right: none; border-bottom: none; border-left: 6px solid rgb(239, 154, 64); border-radius: 0px;"><div style="text-align:center;"><strong><span style="font-size:16px; font-family:Montserrat;">CHEVEUX</span></strong></div></div></div></div>
	<div id="fCg9bB-CROG85-DJn5O8" class="crea-element" data-type="circle" data-locked="0" data-zoom="1" data-ratio="0" data-minwidth="20" data-minheight="20" border-id="432" style="opacity: 1; width: 378px; height: 378px; transform: matrix(1, 0, 0, 1, 208, 254);"><div class="crea-wrapper"><svg preserveAspectRatio="none" class="crea-svg" viewBox="-1 -1 378 378"><ellipse vector-effect="non-scaling-stroke" cx="188" cy="188" rx="188" ry="188" style="fill:#ffffff; fill-opacity:1; stroke:#000000; stroke-width:2; stroke-opacity:0; stroke-linejoin:miter; stroke-linecap:butt;"></ellipse></svg><div class="crea-nested-image" style="display:none;"><svg preserveAspectRatio="none" viewBox="0 0 378 378"><defs><clipPath id="c5F8D-Zsu8E"><ellipse cx="189" cy="189" rx="188" ry="188" style="fill:#2196f3; stroke:none; stroke-linejoin:miter; stroke-linecap:butt;"></ellipse></clipPath></defs></svg><img class="nested-image" e-padding="0" style="display:none; padding-left:1px; padding-top:1px; padding-right:1px; padding-bottom:1px; object-fit:none; object-position:left top; image-rendering:auto; clip-path:url(#c5F8D-Zsu8E); -webkit-clip-path: url(#c5F8D-Zsu8E);"><div class="nested-pattern" style="display:none; padding-left:1px; padding-top:1px; padding-right:1px; padding-bottom:1px; background-repeat:repeat; clip-path:url(#c5F8D-Zsu8E); -webkit-clip-path: url(#c5F8D-Zsu8E);"></div></div><svg preserveAspectRatio="none" class="crea-svg" viewBox="-1 -1 378 378"><ellipse vector-effect="non-scaling-stroke" cx="188" cy="188" rx="188" ry="188" style="fill:none; stroke:#000000; stroke-width:2; stroke-opacity:1; stroke-dasharray:0,0; stroke-linejoin:miter; stroke-linecap:butt;"></ellipse></svg></div></div>
</section>
```
Une page minimale doit être constituée d'une balise racine *section* de type *"page"* représentant la page en elle-même et d'un élément HTML *DIV* enfant de type *"crea-background"*.  
Le type d'un élément est indiqué dans l'attribut *"data-type"*.  
Les types d'éléments autorisés sont : ***"page", "background", "group",  "rectangle", "circle", "triangle", "line", "text", "image", "primitive", "table", "arrow", "sound", "qrcode", "answer_button", "cadre", "document_background"***.  

**1. L'élément *"section"***  
Une balise ***section*** de type ***"page"*** doit être l'élément de plus bas niveau du fichier EDU et contient tous les autres éléments de la page.  
Il représente le corps de la page et en défini certaines caractéristiques.  
Liste des attributs de la ***"section page"*** :  
- ***class*** : classes CSS appliquées à la page. Doit contenir la classe ***"page"***. Autres valeurs possibles : ***"inherit"*** (utilisé dans l'éditeur afin de représenter les contours de la page lorsque la couleur du fond du document - "sous" la page - est en mode ***"Inherit"***).
- ***data-type*** : obligatoirement de type ***"page"***
- ***id*** : l'identifiant unique de la page, sous la forme *"xxxxxx-xxxxxx"* (ou "x" représente un caractère alphanumérique senseible à la casse *[0-9a-zA-Z]*
- ***data-scale*** : la *scale* actuelle de la page, sert à appliquer un zoom sur celle-ci et les éléments qu'elle contient
- ***data-title*** : le titre de la page. **(non usité actuellement)**
- ***data-player-color*** : la couleur du lecteur HTML. **(non usité actuellement)**
- ***data-scene-color*** : la couleur du fond du document pour cette page, surchargeant la valeur définie de manière globale. Valeurs possibles : ***'default'*** (valeur par défaut de l'éditeur et du player), ***'Inherit'*** (la scène hérite de la couleur de la page en cours), ***'#xxxxxx'*** (code hexadécimal d'une couleur) **(non usité actuellement)**
- ***data-transition-type*** : en mode diaporama, type de transition entre cette page et la suivante. **(non usité actuellement)**
- ***data-slide-pause*** : en mode diaporama automatique, durée de la pause sur cette page avant de passer à la suivante. **(non usité actuellement)**
- ***data-visible*** : en mode diaporama, visibilité de la page lors de la navigation avec les boutons *"page suivante"* et *"page précédente"*. ***1 = visible; 0 = invisible***. **(non usité actuellement)**
- ***data-cascade*** : apparition en cascade des éléments enfants de la page (hormis l'élément **background**). ***1 = apparition en cascade ; 0 = pas d'apparition en cascade***. **(non usité actuellement)**
- ***data-width*** : largeur de la page en pixels
- ***data-height*** : hauteur de la page en pixels
- ***style*** : styles CSS appliqués à la page. Peuvent-être différents en fonction du mode d'affichage (diaporama ou classique), et du lieu d'affichage (éditeur ou lecteur). Les styles de base sont :
```
width: 794px; height: 1122px; background-color: rgba(255, 255, 255, 0); position: relative; left: 0px; top: 0px; display: block;
```
***width*** et ***height*** définissent la taille visible de la page après application du ***data-scale***. Le positionnement est généralement en mode ***"absolute"***, mais pourra être autre pour les besoin d'affichage de l'éditeur ou du lecteur. ***left*** et ***top***. L'attribut ***"display"*** peut servir aux jeux de masquage des pages dans l'éditeur ou le lecteur. La couleur de fond, ici d'opacité nulle n'est en général pas usité car un élément de type ***"background"***, premier enfant de la *section*, tient lieu et place du fond de page.

**2. Le commentaire de début de page**  
Un commentaire HTML peut se trouver comme premier élément de la page (*section*).
Ce commentaire sert, pour les pages contenant des exercices interactifs, à stocker les associations ***propositions/cibles*** définissant les bonnes et mauvaises réponses de la page.  
Chaque exercice de la page est représenté sous forme d'une chaine JSON formant une liste de représentation d'objets ***"Answer"*** définissant toutes les associations possibles de propositions et cibles avec le statut associé (1 = bonne réponse ; 0 = mauvaise réponse).  
Un objet ***"Answer"*** est une association propocition/cible/status.

#### Exemple d'objet *Answer*
```
{
	proposition_id: "Ey0aQJ-9KvHyS-2KolAO",
	cible_id: "k42GLi-8OMRVa-9UFFDk",
	status: 1
}
```
Afin d'être stockée pour le lecteur HTML, cet objet ***"Answer"*** sera stockée sous la forme :  
```
id_proposition-id_cible: status
```
La liste des objets ***"Answer"*** représentant un exercice sera donc une chaine JSON de cette forme :
```
{
	id_proposition1-id_cible1: status,
	id_proposition1-id_cible2: status
	id_proposition2-id_cible1: status
	id_proposition2-id_cible2: status
	...
}
```

#### Exemple de chaîne obtenue pour un exercice

```
{"Pkcr6l-kuGJY3-secPer-LhGiXm-03K0qZ-id85MX":1,"Pkcr6l-kuGJY3-secPer-BrKIe6-nOyVnd-xlA0Q4":0,"ECFZOD-YwEu8e-us4T8f-LhGiXm-03K0qZ-id85MX":0,"ECFZOD-YwEu8e-us4T8f-BrKIe6-nOyVnd-xlA0Q4":1,"Pkcr6l-kuGJY3-secPer-0iDL9URN0-xQG0RrNxQ-Ww5KAgZZq":0,"ECFZOD-YwEu8e-us4T8f-0iDL9URN0-xQG0RrNxQ-Ww5KAgZZq":0,"0iDL9URN0-xQG0RrNxQ-Ww5KAgZZq-LhGiXm-03K0qZ-id85MX":0,"0iDL9URN0-xQG0RrNxQ-Ww5KAgZZq-BrKIe6-nOyVnd-xlA0Q4":0,"0iDL9URN0-xQG0RrNxQ-Ww5KAgZZq-0iDL9URN0-xQG0RrNxQ-Ww5KAgZZq":1,"Pkcr6l-kuGJY3-secPer-OxlgMT-xAy5Lc-UIN9S3":0,"ECFZOD-YwEu8e-us4T8f-OxlgMT-xAy5Lc-UIN9S3":0,"0iDL9URN0-xQG0RrNxQ-Ww5KAgZZq-OxlgMT-xAy5Lc-UIN9S3":0,"OxlgMT-xAy5Lc-UIN9S3-LhGiXm-03K0qZ-id85MX":0,"OxlgMT-xAy5Lc-UIN9S3-BrKIe6-nOyVnd-xlA0Q4":0,"OxlgMT-xAy5Lc-UIN9S3-0iDL9URN0-xQG0RrNxQ-Ww5KAgZZq":0,"OxlgMT-xAy5Lc-UIN9S3-OxlgMT-xAy5Lc-UIN9S3":0}
```

Afin d'être stockée dans le commentaire, chacun de ces objets JSON représentant un exercice sera crypté au format *AES*, avec comme clef l'*id* de la page.  
C'est l'ensemble de ces objets représentant potentiellement les différents exercices de la page qui sera concaténé par des ***","*** (puis encodé en Base64 et finalement renversée) qui sera stocké dans le commentaire de la page.  
**Il faut noter que dans cette version du format CREADOC, chaque page ne supporte qu'un seul exercice. Toutes les associations propositions / cibles d'une page seront considérées comme faisant partie du même exercice (ex: un drag and drop et un bouton réponse présents sur la même page seront considérés comme faisant parti d'un seul exercice).**

## Structure des objets CREADOC
**1. Les éléments de base**  
Chaque élément CREADOC présent sur une page doit-être un élément HTML *DIV* contenant la classe CSS correspondante à un des trois types de base :
- *"crea-element"*, pour les objets classiques (rectangles, traits, images, boutons, etc...).
- *"crea-background"*, pour l'objet représentant le fond de la page.
- *"crea-group"*, pour les conteneurs d'objets (objets groupés)

**2. Le type IElementObject**
Les différentes propriétés d'un objet CREADOC sont représentés par un type d'objet Javascript particulier, appelé ***"ElementObject"***.  
Voici l'interface *TypeScript* de ces objets :
```
export interface IElementObject {
  id: string;
  dataType: ElementType;
  dataLocked: number;
  dataZoom: number;
  dataRatio: number;
  dataHidden: number;
  dataMinwidth?: number;
  dataMinheight?: number;
  dataGroup?: string;
  dataSize?: number;
  dataPremium?: number;
  dataCibles?: string;
  dataPropAnimate?: number;
  dataPropZoom?: number;
  dataMarkCibles?: number;
  dataAllowWrong?: number;
  dataAudioFeedback?: number;
  dataSrc?: string;
  svgDatas?: any;
  svgEditable?: number;
  svgBorderEditable?: number;
  textDatas?: string;
  transformCSS: ICSSTransform;
  borderCSS?: ICSSBorder;
  backgroundCSS?: ICSSBackground;
  svgCSS?: ICSSSvg;
  tableCSS?: ICSSTable;
  arrowCSS?: ICSSArrow;
  soundDatas?: ISoundDatas;
  textCSS?: ICSSText;
}
```
Cet objet défini, pour chaque élément, les différentes propriétés qui affecteront sont rendu visuel et/ou son comportement dans le cas d'éléments interactifs.  
Voici la description de la structure des différents objets utilisés par un ***ElementObject*** pour définir certaines de ses propriétés :
```
export enum ElementType {
  RECTANGLE = 'rectangle',
  CIRCLE = 'circle',
  TRIANGLE = 'triangle',
  LINE = 'line',
  TEXT = 'text',
  IMAGE = 'image',
  PRIMITIVE = 'primitive',
  BACKGROUND = 'background',
  DOCUMENT_BACKGROUND = 'document_background',
  GROUP = 'group',
  TABLE = 'table',
  CADRE = 'cadre',
  ARROW = 'arrow',
  SOUND = 'sound',
  QRCODE = 'qrcode',
  ANSWER_BUTTON = 'answer_button',
}

export interface ICSSTransform {
 width: number;
 height: number;
 matrix: Matrix;
 opacity: number;
}

export interface ICSSBorder {
 dataId: string;
 borderImageRepeat?: string;
 borderImageSource?: string;
 borderImageSlice?: string;
 borderTopWidth?: number;
 borderRightWidth?: number;
 borderBottomWidth?: number;
 borderLeftWidth?: number;
 visibility?: string; // hidden or visible => border pattern div
}

export interface ICSSBackground {
 dataId?: string;
 width?: number;
 height?: number;
 backgroundDatas?: string;
 clipPath?: string;
 objectFit: string;
 objectFitEditable: number;
 objectPosition?: string;
 imageRendering?: string;
 paddingTop?: number;
 paddingLeft?: number;
 paddingRight?: number;
 paddingBottom?: number;
 extraPadding?: number;
}

export interface ICSSSvg {
 strokeWidth: number;
 stroke: string;
 strokeOpacity: number;
 strokeDasharray: string;
 strokeLinecap?: string;
 strokeLinejoin?: string;
 strokeMiterlimit?: number;
 fill?: string;
 fillOpacity?: number;
 borderRadius?: number;
}

export interface ICSSTable {
 backgroundColor?: string;
 borderColor?: string;
 vPadding: number;
 hPadding: number;
 tab_class: string;
}

export interface ICSSArrow {
 dataId: string;
}

export interface ISoundDatas {
 autoplay: boolean;
 loop: boolean;
 controls: boolean;
 volume: number;
 src: string;
}

export interface ICSSText {
  backgroundColor: string;
  borderTopWidth: string;
  borderTopStyle: string;
  borderTopColor: string;
  borderRightWidth: string;
  borderRightStyle: string;
  borderRightColor: string;
  borderBottomWidth: string;
  borderBottomStyle: string;
  borderBottomColor: string;
  borderLeftWidth: string;
  borderLeftStyle: string;
  borderLeftColor: string;
  paddingTop: string;
  paddingRight: string;
  paddingBottom: string;
  paddingLeft: string;
  borderRadius: number;
  borderWidth: string;
  borderStyle: string;
  borderColor: string;
  textStyle?: string;
}
```
L'ensemble de ses propriétés doivent-être stockées sous forme d'attributs ou de styles CSS inline sur l'élément CREADOC ou les éléments HTML enfants qu'il contient.

**3. Le fond de page** 

**4. Le rectangle**  
**5. Le cercle**  
**6. Le triangle**  
**7. Le trait**  
**8. Les images**  
**9. Les primitives**  
**10. Les flèches**  
**11. Le champ de texte**  
**12. Le tableau**  
**13. Le bouton son**  
**14. Le QRCode**  
**15. Le bouton réponse**  
 
**16. Les groupes**  
**17. Les objets déplaçables**  
**18. Les Blocs**  

## Format de stockage des exercices interactifs
**1. La notion de proposition**  
**2. La notion de cible**  
**3. L'association proposition(s)/cible(s)**  
**4. Les réponses**  
**5. Exemple de stockage JSON**  
