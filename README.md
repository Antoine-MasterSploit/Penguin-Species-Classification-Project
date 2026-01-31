Ce projet porte sur la classification supervisée multi-classes d’individus de manchots à partir de caractéristiques biologiques et environnementales.
L’objectif principal est de prédire l’espèce d’un manchot parmi trois espèces :

Adelie Penguin (Pygoscelis adeliae)

Chinstrap Penguin (Pygoscelis antarctica)

Gentoo Penguin (Pygoscelis papua)

à partir de variables morphologiques (bec, nageoires, masse corporelle), isotopiques et catégorielles (île, sexe).

Le jeu de données utilisé est celui des Palmer Penguins, un dataset réel largement utilisé pour l’apprentissage statistique et le machine learning, car il présente à la fois :

des données continues,

des variables catégorielles,

des valeurs manquantes,

et une structure biologique interprétable.

Ce projet vise à mettre en œuvre une démarche complète d’analyse de données, depuis les données brutes jusqu’à la comparaison entre méthodes paramétriques et non paramétriques.

Contenu du projet

Le projet est structuré de manière progressive et suit les étapes classiques d’un pipeline de data science.

1. Analyse des données brutes

La première étape consiste à explorer le dataset initial afin de comprendre :

la structure des données,

le type des variables (numériques / catégorielles),

la présence de valeurs manquantes,

la distribution des espèces.

Le jeu de données contient initialement 344 observations et 17 variables, avec plusieurs valeurs manquantes, notamment dans :

la variable Sex,

les mesures morphologiques,

les mesures isotopiques,

et une colonne Comments très peu renseignée.

Cette étape permet d’identifier les contraintes du dataset avant toute modélisation.

2. Nettoyage des données

Une stratégie de nettoyage claire a été mise en place :

conservation uniquement des individus dont le sexe est identifié comme MALE ou FEMALE ;

suppression implicite des observations ambiguës ;

imputation des valeurs manquantes numériques par la médiane, méthode robuste face aux valeurs extrêmes.

Après nettoyage, le dataset contient 333 individus propres, sans valeurs manquantes sur les variables utilisées pour la classification.

3. Analyse descriptive

Une analyse statistique descriptive a ensuite été réalisée afin de vérifier la pertinence du problème de classification.

Cette analyse comprend :

statistiques globales (moyenne, écart-type, quartiles),

moyennes morphologiques par espèce,

effectifs par espèce,

tableaux croisés entre l’île et l’espèce,

histogrammes par espèce.

Les résultats montrent des différences morphologiques nettes, notamment :

les manchots Gentoo ont des nageoires et une masse corporelle significativement plus élevées ;

les espèces Adelie et Chinstrap présentent des profils plus proches ;

certaines espèces sont fortement liées à certaines îles, ce qui apporte un fort pouvoir discriminant.

Ces observations expliquent en grande partie les performances élevées des modèles par la suite.

4. Prétraitement et pipeline

Les données contiennent des variables mixtes.

Le prétraitement est donc réalisé à l’aide d’une pipeline scikit-learn :

variables numériques :

imputation par la médiane,

standardisation (StandardScaler) ;

variables catégorielles :

imputation par la modalité la plus fréquente,

encodage One-Hot.

L’ensemble est intégré dans un ColumnTransformer, puis dans une Pipeline, ce qui garantit :

reproductibilité,

absence de fuite de données,

cohérence entre train et test.

Les données sont ensuite séparées via un train/test split 80/20 stratifié, afin de conserver les proportions de classes.

5. Modèles de classification classiques

Trois classifieurs supervisés ont été évalués :

Logistic Regression

Random Forest

Naive Bayes

Les performances sont évaluées à l’aide de :

balanced accuracy,

F1-score macro,

matrices de confusion.

Résultats principaux :

la régression logistique obtient des performances parfaites ;

la forêt aléatoire commet une seule erreur ;

Naive Bayes est plus faible en raison de son hypothèse d’indépendance conditionnelle, non respectée ici.

Ces résultats montrent l’impact direct des hypothèses statistiques des modèles sur leurs performances.

6. Classification non paramétrique par KDE

Une approche avancée basée sur les Kernel Density Estimators (KDE) a ensuite été implémentée.

Le principe est le suivant :

estimation de la densité conditionnelle 
𝑝
(
𝑥
∣
𝑦
)
p(x∣y) pour chaque espèce ;

application de la règle de Bayes pour la classification ;

utilisation d’un KDE multivarié afin de ne pas supposer l’indépendance des variables.

Plusieurs noyaux ont été comparés :

gaussian,

epanechnikov,

exponential,

tophat.

Le paramètre de lissage (bandwidth) est sélectionné via validation croisée stratifiée (GridSearchCV).

Les résultats montrent :

des performances quasi parfaites pour la majorité des noyaux ;

une légère sensibilité au choix du noyau (tophat légèrement moins performant).

Cette partie valide théoriquement et empiriquement l’approche non paramétrique.

7. Comparaison avec LDA

Enfin, les résultats sont comparés à une Analyse Discriminante Linéaire (LDA).

LDA est un modèle génératif paramétrique supposant :

des distributions gaussiennes par classe,

une covariance commune.

Sur ce dataset, LDA atteint également des performances parfaites, ce qui confirme que :

les distributions sont proches de gaussiennes,

les frontières de décision sont majoritairement linéaires.

Conclusion

Ce projet met en œuvre une démarche complète de data science et de machine learning, allant :

de l’analyse des données brutes,

au nettoyage raisonné,

à l’exploration statistique,

à la construction de pipelines robustes,

à la comparaison de modèles paramétriques et non paramétriques.

Les principaux enseignements sont :

les différences morphologiques entre espèces sont très marquées ;

des modèles simples et interprétables (Logistic Regression, LDA) suffisent à atteindre des performances maximales ;

les méthodes plus flexibles (Random Forest, KDE) confirment ces résultats ;

les hypothèses statistiques jouent un rôle central dans la qualité des prédictions.

Ce projet illustre ainsi l’importance de comprendre les données avant de complexifier les modèles, et montre qu’un modèle bien choisi, même simple, peut être aussi performant qu’une méthode plus sophistiquée.
