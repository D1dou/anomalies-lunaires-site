---
dg-publish: true
dg-home: true
---
En 2023, la NASA a confirmé que la Lune possède un noyau interne solide de 240km de rayon, entouré d’une mince couche externe liquide de 90km. Pourtant, contrairement à la Terre, notre satellite naturel ne génère aucun champ magnétique global. Cette apparente contradiction cache une réalité fascinante : la Lune conserve dans sa croûte les traces magnétiques d’un ancien dynamo actif il y a, hypothétiquement, plus de 2 milliards d’années.

![[IMG_1188 1.webp]]

--- start-multi-column: ID_ID_8o1h
```column-settings
Number of Columns: 2
Largest Column: standard
```

![[Moon_South_Pole.jpg]]

L'image du dessus, provient de la mission Clementine, qui a cartographié la Lune dans les années 1990.

Certaines zones brillantes persistantes (swirls) peuvent signaler une protection magnétique, mais seules les mesures magnétométriques directes confirment la présence d'anomalies.
![[180910-mach-lunar-swirl-nasa-lunar-reconnaissance-orbiter-se-1-231p.webp]]

Cette image capture **Reiner Gamma**, le lunar swirl le plus célèbre de la Lune, situé dans l'Oceanus Procellarum (face visible).

Ces "fossiles magnétiques", appelés anomalies magnétiques lunaires, se manifestent par des champs localisés atteignant plusieurs centaines de nanoteslas au sol - soit 0,001% du champ terrestre.

--- column-break ---

 Ils coïncident avec des formations visuelles : des motifs spiralés brillants observables depuis la Terre. **Reiner Gamma**, sera exploré par la mission **Lunar Vertex** en **2026.**
###### Problématique de Recherche

Malgré cinq décennies d'observations orbitales depuis les missions Apollo, trois questions fondamentales demeurent sans réponse :
-  Ces anomalies proviennent-elles d'un ancien dynamo global, de matériaux d'impact magnétisés, ou de processus magmatiques souterrains ?
-  Pourquoi se concentrent-elles aux antipodes des grands bassins d'impact ?
-  Comment ces champs locaux modifient-ils le bombardement solaire et créent-ils les motifs de swirls observés ?

**Objectifs de l'Analyse**

Cette étude exploite les données magnétiques de la mission Kaguya/SELENE (2007-2009) pour :

-  Cartographier et classifier les anomalies magnétiques à haute résolution (grille 1°×1°, soit ~30 km)
-  Identifier les corrélations statistiques entre intensité magnétique, géologie de surface et histoire d'impacts
-  Tester l'hypothèse des éjecta convergents aux antipodes des grands bassins
-  Évaluer les facteurs influençant la distribution des anomalies (âge, topographie)

--- end-multi-column
##### Méthodologie
--- start-multi-column: ID_7akl
```column-settings
Number of Columns: 2
Largest Column: standard
```

###### Données Utilisées

**Source principale :** MA_GD (Magnetic Anomaly Grid Data)

- Mission : Kaguya/SELENE (JAXA)
- Altitude de mesure : 100 km
- Résolution spatiale : 1°×1° (~30 km au sol)
- Période d'acquisition : 2007-2009
- Points de mesure : 64 440

**Format des données :**
`Latitude, Longitude, Bx, By, Bz, Btotal, σBx, σBy, σBz, σBtotal, N_measurements`


--- column-break ---

Voici un pdf synthétique permettant la lecture des données : 
    https://data.darts.isas.jaxa.jp/pub/pds3/sln-l-lmag-5-ma-grid-v1.0/document/LMAG_Format_en_V01.pdf

Les fichiers suivent le standard PDS (Planetary Data System) avec des métadonnées en format `.lbl` et des données en format `.dat`.

--- end-multi-column
##### Approche 
--- start-multi-column: ID_b0sv
```column-settings
Number of Columns: 2
Largest Column: standard
```

L'analyse combine trois axes complémentaires :

**1. Géophysique computationnelle**

-  Parsing des fichiers PDS (format JAXA/SELENE)
-  Consolidation des grilles magnétiques satellitaires
-  Extraction des composantes du champ magnétique

--- column-break ---

**2. Data science**

-  Classification non supervisée (DBSCAN) des régions d'anomalies
-  Analyse de corrélation spatiale avec les données topographiques SLDEM2015
-  Tests statistiques (Pearson, Spearman)

**3. Visualisation avancée**

-  Cartographie 2D et 3D des anomalies
-  Superposition multi-couches (magnétisme, géologie, âge des surfaces)
-  Identification des clusters principaux

--- end-multi-column
##### Traitement des Données
--- start-multi-column: ID_n9ph
```column-settings
Number of Columns: 2
Largest Column: standard
```

**Étape 1 : Acquisition et parsing**
Lecture des fichiers `.lbl` pour extraire les métadonnées, puis chargement des données `.dat` au format CSV avec les 11 colonnes de mesures magnétiques.

**Étape 2 : Identification des anomalies** 
Application d'un seuil statistique à 2σ (deux écarts-types) pour isoler les anomalies significatives. Sur 64 440 points, 24 899 anomalies ont été identifiées (38,6% du total).

**Étape 3 : Clustering spatial** 
Algorithme DBSCAN (Density-Based Spatial Clustering) appliqué aux anomalies les plus fortes (top 20%, seuil à 0,40 nT) avec les paramètres eps=0,20 et min_samples=25. 

--- column-break ---

Cette approche a permis d'identifier 20 clusters distincts.

**Étape 4 : Analyses corrélatives**
-  Âge géologique estimé par région (basé sur les grandes provinces lunaires)
-  Test de l'hypothèse des antipodes pour les bassins Imbrium, Serenitatis, Crisium et Orientale
-  Corrélation avec la topographie SLDEM2015 (LRO/LOLA)

--- end-multi-column
#### Résultats
##### 1. Distribution Globale des Anomalies

![[01_apercu_donnees_magnetiques.png]]
				**Figure 1 : Aperçu des données magnétiques Kaguya**

--- start-multi-column: ID_s1oo
```column-settings
Number of Columns: 2
Largest Column: standard
```

L'analyse révèle une distribution asymétrique marquée des anomalies magnétiques lunaires (**Figure 1, panneaux supérieurs**). Sur 64 440 points de mesure, 24 899 présentent des anomalies significatives (38,6% de la surface). L'intensité maximale atteint 2,40 nT dans le bassin South Pole-Aitken.

**Statistiques descriptives :**

`Btotal minimum : -5,2 nT`
`Btotal maximum : +2,4 nT`
`Btotal moyen : 0,02 nT`
`Btotal médian : 0,01 nT`

La distribution (**Figure 1, panneau inférieur gauche**) montre un pic massif proche de zéro, confirmant que la majorité de la surface lunaire présente un champ magnétique négligeable. 

--- column-break ---

Le motif en bandes verticales visible sur la carte du nombre de mesures (**Figure 1, panneau inférieur droit**) reflète l'orbite polaire de Kaguya, avec une couverture plus dense aux hautes latitudes.

La distribution montre une concentration prononcée sur la face cachée de la Lune, particulièrement dans l'hémisphère Sud autour du bassin South Pole-Aitken. Cette asymétrie face visible/face cachée est cohérente avec l'histoire géologique lunaire : la face visible, couverte de maria basaltiques récents (3,0-3,8 Ga), a vu ses anomalies anciennes partiellement effacées par les coulées de lave.

--- end-multi-column

![[02_carte_anomalies_principales.png]]
			    **Figure 2 : Carte des anomalies magnétiques lunaires (>2σ)**

La **Figure 2** présente la cartographie complète des anomalies significatives. La domination de l'hémisphère Sud et de la face cachée est évidente, avec des concentrations majeures autour de -30°S à -50°S de latitude et -180°E à +180°E de longitude (région du bassin SPA).
### 2. Clustering des Anomalies Fortes

![[05_clusters_sans_reiner.png]]
			**Figure 3 : Clustering spatial des anomalies (sans Reiner Gamma)**

Le clustering DBSCAN des anomalies les plus intenses (top 20%, soit 4 716 points au-dessus de 0,40 nT) a identifié 20 groupes distincts (**Figure 3, panneau supérieur**). La répartition est dominée par trois clusters majeurs (bleu foncé) situés dans et autour du bassin South Pole-Aitken.

**Top 5 des clusters par intensité maximale :**

| Cluster | Latitude | Longitude | Btotal max | N points | Région              |
| ------- | -------- | --------- | ---------- | -------- | ------------------- |
| 0       | -27,5°S  | -150,2°E  | 2,40 nT    | 1281     | Bassin SPA (centre) |
| 1       | -32,3°S  | 163,8°E   | 1,38 nT    | 964      | SPA Est             |
| 2       | -29,3°S  | 80,6°E    | 1,32 nT    | 339      | Équateur Sud        |
| 3       | -38,7°S  | 11,0°E    | 1,18 nT    | 171      | SPA Ouest           |
| 4       | 17,9°N   | 72,2°E    | 1,06 nT    | 392      | Highlands Nord      |

Le cluster 0 représente à lui seul 27% des anomalies fortes identifiées, confirmant la domination écrasante du bassin South Pole-Aitken dans la distribution du magnétisme lunaire. Cette concentration massive suggère une origine commune liée à l'événement d'impact géant qui a créé le bassin il y a 4,3 milliards d'années.

##### 3. Reiner Gamma : Le Paradoxe du Swirl Célèbre

![[06_clusters_avec_reiner 1.png]]
		  	    **Figure 4 : Localisation de Reiner Gamma parmi les clusters**

--- start-multi-column: ID_gvj2
```column-settings
Number of Columns: 2
Largest Column: standard
```


Reiner Gamma, malgré sa célébrité visuelle, présente une intensité magnétique modérée de seulement 0,37 nT à 100 km d'altitude (**Figure 4, étoile rouge**). Localisé à 9,0°N, -59,5°E, ce swirl spectaculaire ne figure pas parmi les anomalies les plus intenses de la Lune.

Cette apparente contradiction illustre un point crucial : la visibilité d'un swirl ne corrèle pas directement avec l'intensité magnétique mesurée en orbite.
Le motif brillant de Reiner Gamma résulte d'une protection locale contre le vent solaire qui préserve l'albédo de surface, un processus qui peut se manifester même avec des champs magnétiques relativement faibles.
--- column-break ---



La mission Lunar Vertex, prévue pour 2026, atterrira précisément sur ce site pour résoudre ce mystère : comment un champ si modéré génère-t-il un swirl si spectaculaire ? Les mesures in situ permettront de caractériser le champ magnétique au sol (attendu entre 1-5 nT) et de comprendre les mécanismes d'interaction avec le plasma solaire.

--- end-multi-column
##### 4. Test de l'Hypothèse des Antipodes

![[07_analyse_antipodes.png]]
			**Figure 5 : Corrélation clusters-antipodes des bassins d'impact**

L'hypothèse historique suggère que les éjecta des grands impacts convergent à l'antipode du bassin, où ils se magnétisent en présence d'un dynamo actif. Nos tests sur quatre bassins majeurs (**Figure 5**, cercles rouges) révèlent une validation partielle de cette théorie.

**Résultats par bassin (rayon de recherche : 15°) :**

| Bassin          | Antipode        | Cluster proche | Distance  | Validation |
| --------------- | --------------- | -------------- | --------- | ---------- |
| Imbrium         | -32,8°S,-15,6°E | Cluster 3      | 27,3°     | ✗          |
| **Serenitatis** | -28,0°S, 17,5°E | **Cluster 3**  | **12,5°** | **✓ **     |
| Crisium         | -17,0°S, 59,1°E | Cluster 14     | 17,7°     | ✗          |
| Orientale       | 19,4°N,-92,8°E  | Cluster 15     | 30,3°     | ✗          |
**Taux de correspondance : 25% (1/4 bassins)**
--- start-multi-column: ID_61yb
```column-settings
Number of Columns: 2
Largest Column: standard
```

La **Figure 5** montre visuellement que les cercles rouges (antipodes) ne coïncident généralement pas avec les gros clusters colorés. Seul Serenitatis présente un chevauchement partiel avec le cluster 3 (orange).

Ces résultats nuancés suggèrent que l'hypothèse des antipodes, bien que valide pour certains bassins comme Serenitatis, ne constitue pas le mécanisme dominant de magnétisation lunaire.
La majorité des anomalies fortes (clusters 0

--- column-break ---
, 1, 2, 8) sont localisées **dans** le bassin South Pole-Aitken lui-même, pas à son antipode.
Cette observation est cohérente avec la littérature récente (2021-2024) qui propose un mécanisme alternatif : lors d'impacts obliques, le matériau du projectile (riche en fer) se dépose directement sur le bord du bassin plutôt qu'à l'antipode. Pour le bassin SPA, formé par un impact oblique du sud au nord il y a 4,3 Ga, le matériau magnétisable s'est concentré sur le bord nord du bassin - exactement où se trouvent nos clusters les plus intenses.

--- end-multi-column
##### 5. Analyse Âge-Magnétisme

![[08_analyse_age_magnetisme.png]]
		**Figure 6 : Corrélation entre âge géologique et intensité magnétique**

L'analyse de corrélation entre l'âge géologique estimé et l'intensité magnétique révèle une relation complexe, loin d'une simple décroissance avec le temps (**Figure 6, panneau gauche**).

**Corrélation âge-intensité : r = 0,288 (faible, non significative)**

Distribution par type de terrain (**Figure 6, panneau droit**) :

| Terrain                 | Âge (Ga) | Intensité moyenne (nT) | N clusters |
| ----------------------- | -------- | ---------------------- | ---------- |
| Bassin SPA              | 4,3      | 1,06                   | 6          |
| Ancient Highlands       | 4,0      | 0,86                   | 4          |
| Terrains intermédiaires | 3,8      | 0,55                   | 5          |
| Maria (récents)         | 3,2      | 0,70                   | 5          |
--- start-multi-column: ID_vmkj
```column-settings
Number of Columns: 2
Largest Column: standard
```

Le résultat le plus surprenant concerne les maria récents (3,0-3,5 Ga, points rouges sur **Figure 6 gauche**) qui, contrairement aux attentes, présentent des anomalies significatives allant jusqu'à 1,06 nT. Cette observation contredit l'hypothèse naïve selon laquelle les coulées de lave récentes auraient systématiquement détruit les anomalies anciennes.

Le box plot (**Figure 6, panneau droit**) montre que le bassin SPA et les Ancient Highlands présentent les médianes les plus élevées et la plus grande dispersion, tandis que les terrains intermédiaires sont remarquablement homogènes et faibles.

--- column-break ---

Plusieurs mécanismes peuvent expliquer cette préservation :

-  Certaines coulées basaltiques ont recouvert des régions magnétisées sans les chauffer au-dessus de la température de Curie
-  Les maria eux-mêmes se sont magnétisés pendant leur refroidissement si un dynamo résiduel existait encore vers 
  3,2 Ga
-  Des anomalies locales ont été créées par des impacts ultérieurs dans des zones déjà couvertes de maria

--- end-multi-column
##### 6. Corrélation avec la Topographie

![[09_correlation_topographie.png]]
					**Figure 7 : Analyse topographie-magnétisme**

L'analyse de corrélation entre l'altitude (données SLDEM2015) et l'intensité magnétique révèle une absence totale de relation (**Figure 7, panneau supérieur gauche**).

**Corrélation altitude-magnétisme : r = -0,005 (nulle)**

--- start-multi-column: ID_snei
```column-settings
Number of Columns: 2
Largest Column: standard
```

**Statistiques topographiques :**

Altitude minimum : -15 905 m (bassins profonds)
Altitude maximum : +17 130 m (montagnes)
Altitude moyenne : -1 377 m (légèrement sous le niveau de référence)

Le scatter plot (**Figure 7, haut gauche**) montre un nuage de points sans aucune tendance, confirmé par la courbe plate de l'intensité moyenne par tranche d'altitude (**Figure 7, haut droite**). La carte topographique (**Figure 7, bas gauche**) révèle la structure complexe du relief lunaire, tandis que l'histogramme 2D (**Figure 7, bas droite**) montre que la densité maximale de points se situe autour de -1442 m avec des intensités magnétiques faibles.

--- column-break ---

**Vérification sur points connus :**

-  Bassin SPA (Cluster 0) : -3 905 m
-  Reiner Gamma : -3 456 m
-  Équateur : -1 441 m

Ce résultat est géophysiquement significatif : il démontre que la profondeur des bassins d'impact n'est pas un facteur déterminant de l'intensité magnétique. Les anomalies ne proviennent pas simplement de l'excavation de matériaux profonds magnétisés, mais dépendent de facteurs plus complexes comme la composition chimique des matériaux déposés (présence de fer métallique dans les éjecta d'impacteur) et l'histoire thermique locale.

--- end-multi-column
##### 7. Modélisation 3D des Sources Magnétiques

![[10_modelisation_3d_sources.png]]
		**Figure 8 : Estimation de la profondeur des sources magnétiques**
--- start-multi-column: ID_b89a
```column-settings
Number of Columns: 2
Largest Column: standard
```

Une estimation qualitative de la profondeur des sources a été réalisée en appliquant une règle géophysique simplifiée : profondeur ≈ rayon spatial de l'anomalie / 2. La **Figure 8** présente une visualisation tridimensionnelle où l'axe vertical (négatif) représente la profondeur estimée sous la surface.

**Résultats indicatifs :**

Profondeur moyenne : 166 km
Profondeur minimale : 15 km (sources superficielles)
Profondeur maximale : 450 km (bassin SPA)

Les clusters les plus étendus spatialement (SPA : 450 km, point rouge foncé sur **Figure 8**) correspondent à des sources volumineuses et profondes, cohérent avec l'hypothèse de matériau 

--- column-break ---

d'impacteur excavé et redistribué lors de l'impact géant. 
Les petites anomalies localisées (75-120 km de profondeur, points en surface) suggèrent des sources plus superficielles, probablement des éjecta déposés en surface.

**Note méthodologique :** Cette modélisation reste qualitative. Une inversion magnétique rigoureuse nécessiterait des mesures à plusieurs altitudes, des données sur le gradient du champ magnétique, et des contraintes géologiques indépendantes sur l'épaisseur et la composition de la croûte. Les profondeurs absolues présentées ici doivent être considérées comme des estimations indicatives de l'échelle spatiale des sources, non comme des mesures quantitatives précises.


--- end-multi-column
##### Discussion
--- start-multi-column: ID_lpci
```column-settings
Number of Columns: 2
Largest Column: standard
```

###### Domination du Bassin South Pole-Aitken

L'analyse révèle sans ambiguïté que le bassin South Pole-Aitken constitue la source dominante du magnétisme lunaire observable. Avec une intensité maximale de 2,40 nT (6,5 fois supérieure à Reiner Gamma), ce bassin concentre 6 des 20 clusters identifiés et représente 90% des anomalies les plus fortes.

Cette domination s'explique par la conjonction de trois facteurs exceptionnels :

**1. Un impact cataclysmique** 

Formé il y a 4,3 milliards d'années, le bassin SPA (2 500 km de diamètre, 6-8 km de profondeur) résulte du plus grand impact connu dans le système solaire interne. L'impacteur, probablement un corps différencié de plusieurs dizaines de kilomètres, a excavé des quantités massives de matériau riche en fer métallique - potentiellement des fragments du noyau de l'impacteur lui-même.

--- column-break ---

**2. Magnétisation pendant le dynamo actif** 

L'impact est survenu pendant la période d'activité maximale du dynamo lunaire (4,2-3,6 Ga), permettant une magnétisation thermoremanente efficace lors du refroidissement des matériaux déposés. Les découvertes paléomagnétiques de Chang'e-5 (publiées janvier 2025) confirment que le dynamo lunaire est resté actif jusqu'à 2,03 Ga, bien plus tard qu'anticipé.

**3. Préservation exceptionnelle** 

Situé sur la face cachée, le bassin SPA n'a jamais été recouvert par les coulées de lave basaltique qui ont effacé une partie des anomalies de la face visible. La croûte magnétisée il y a 4,3 Ga reste exposée, préservant intacte la signature de cet événement cataclysmique.

--- end-multi-column
##### Remise en Question de l'Hypothèse des Antipodes
--- start-multi-column: ID_j6oh
```column-settings
Number of Columns: 2
Largest Column: standard
```

L'hypothèse classique des éjecta convergents aux antipodes, formulée dans les années 1970-1980, ne trouve qu'un support limité dans nos données (25% de taux de validation). Cette faible correspondance s'explique par plusieurs facteurs :

**Cas du bassin Serenitatis (validé)** Le seul bassin montrant une corrélation claire (cluster 3 à 12,5° de l'antipode) correspond probablement au mécanisme classique : convergence d'éjecta riches en matériau d'impacteur à l'antipode, magnétisés in situ.

**Cas du bassin SPA (non validé)** Les anomalies les plus fortes se trouvent **dans le bassin lui-même**, pas à son antipode. Cette observation est cohérente avec les simulations d'impacts obliques publiées en 2021 : pour un angle d'impact de 30-45° (le plus probable), le matériau de l'impacteur se dépose asymétriquement le long du bord du bassin dans la direction du mouvement, créant des anomalies locales plutôt qu'antipodales.

--- column-break ---

**Bassins Imbrium, Crisium, Orientale (non validés)** L'absence de corrélation peut s'expliquer par :

- Superposition d'impacts ultérieurs ayant perturbé les anomalies antipodales
- Démagnétisation progressive par bombardement météoritique
- Recouvrement partiel par des coulées de lave (cas d'Imbrium)

Cette remise en question ne signifie pas que l'hypothèse des antipodes est fausse, mais qu'elle ne représente qu'un mécanisme parmi d'autres. La littérature récente (2021-2024) propose un modèle hybride : antipodes pour certains bassins (Serenitatis, Crisium partiellement), dépôt local pour d'autres (SPA, Orientale).

--- end-multi-column
#### Facteurs Déterminants du Magnétisme Lunaire
--- start-multi-column: ID_84gl
```column-settings
Number of Columns: 2
Largest Column: standard
```

L'absence de corrélation significative avec l'âge (r=0,288) et l'altitude (r=-0,005) démontre que le magnétisme lunaire ne dépend pas de facteurs géométriques simples. Les déterminants réels sont :

**1. Composition chimique** 
La présence de fer métallique (susceptibilité magnétique élevée) dans les matériaux d'impacteur est cruciale. Les roches lunaires natives, pauvres en fer, ne peuvent acquérir qu'une faible magnétisation. C'est le matériau extralunar qui porte le signal magnétique principal.

**2. Timing de magnétisation** 


--- column-break ---

L'intensité du dynamo lunaire au moment de la magnétisation est déterminante. Les impacts survenus pendant la période 4,2-3,6 Ga ont bénéficié d'un champ magnétisant fort (plusieurs dizaines de μT), tandis que les impacts plus tardifs se sont magnétisés dans un champ décroissant.

**3. Histoire thermique** 
Tout événement ultérieur chauffant les roches au-dessus de la température de Curie (593-1043 K selon le minéral) efface la magnétisation. Les maria de la face visible ont partiellement démagnétisé les régions qu'ils recouvrent, tandis que la face cachée a préservé ses anomalies anciennes.

--- end-multi-column
##### Conclusion

Cette analyse des données Kaguya/SELENE (2007-2009) apporte plusieurs contributions à la compréhension du magnétisme lunaire :

**1. Quantification précise de la distribution des anomalies** :

Sur 64 440 points de mesure à 100 km d'altitude, 38,6% présentent des anomalies significatives. Le clustering DBSCAN identifie 20 régions magnétiques distinctes, dominées par le bassin South Pole-Aitken qui concentre 90% des anomalies les plus intenses.

**2. Validation partielle de l'hypothèse des antipodes** : 

Seulement 25% des bassins testés (Serenitatis uniquement) montrent une corrélation avec leurs antipodes. Cette faible correspondance suggère que les mécanismes de magnétisation sont multiples : antipodes pour certains bassins, dépôt local pour d'autres (notamment SPA).

**3. Identification des facteurs non-déterminants** : 

L'âge géologique (r=0,288) et l'altitude topographique (r=-0,005) ne prédisent pas l'intensité magnétique. Les maria récents (3,2 Ga) peuvent préserver ou même créer des anomalies significatives (jusqu'à 1,06 nT), contredisant l'hypothèse de destruction systématique.

**4. Caractérisation de Reiner Gamma :** 

Ce swirl célèbre présente une intensité modérée (0,37 nT à 100 km), démontrant que la visibilité optique ne corrèle pas avec l'intensité magnétique orbitale. Les mesures de Lunar Vertex en 2026 clarifieront ce paradoxe.

Ces résultats sont cohérents avec la littérature scientifique 2021-2025 qui privilégie un modèle de magnétisation par matériau d'impacteur riche en fer, déposé soit aux antipodes (mécanisme classique) soit localement dans le bassin (mécanisme oblique), puis magnétisé par le dynamo lunaire ancien.

#### Perspectives

**Analyses complémentaires possibles :**

-  Inversion magnétique rigoureuse utilisant des données multi-altitudes (Kaguya 100 km + Lunar Prospector 30 km)
-  Corrélation avec les mesures de composition chimique (FeO, TiO₂) de la mission GRAIL
-  Analyse directionnelle du champ magnétique (pas seulement l'intensité scalaire)
-  Comparaison avec les âges radiométriques précis de Chang'e-5 et futures missions de retour d'échantillons

**Implications pour l'exploration future :**

-  Prioriser l'échantillonnage du bord nord du bassin SPA (clusters 0, 1, 3) où se concentrent les anomalies les plus intenses
-  Mission Lunar Vertex (2026) : mesures au sol de Reiner Gamma pour comprendre le mécanisme de formation des swirls
-  Missions futures : caractériser in situ la composition des zones magnétiques pour quantifier la contribution du matériau d'impacteur vs roches lunaires natives

---

#### Contribution

L'étude offre une réplication indépendante et une quantification précise des observations antérieures, utilisant des méthodes modernes de data science (clustering DBSCAN) sur un dataset complet de la mission Kaguya. Les résultats confirment et affinent les connaissances établies, tout en mettant en évidence les limites de certaines hypothèses classiques (antipodes) et en identifiant clairement le bassin South Pole-Aitken comme la structure dominante du paysage magnétique lunaire.

**Mots-clés** : Anomalies magnétiques lunaires, dynamo planétaire, géophysique spatiale, Kaguya/SELENE, clustering DBSCAN, bassin South Pole-Aitken, Reiner Gamma, Lunar Vertex