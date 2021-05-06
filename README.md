# Creuse-Movie-Project

<img src="https://www.premiere.fr/sites/default/files/styles/scale_crop_1280x720/public/2018-04/salle-de-cinema-le-cinema-de-personne-1320.png"/>

## Introduction

Créé en 1998, “Netflix est un service de diffusion en streaming qui permet à ses membres de regarder une grande variété de séries TV, films, documentaires, etc. sur des milliers d'appareils connectés à Internet.” => Wikipédia Netflix

Aujourd’hui Netflix pèse plus de 20 milliards de dollars de chiffre d'affaires et consomme 12,6% de la bande passante internet mondiale.

Lorsqu’on accède au service Netflix, le système de recommandations aide l’utilisateur à trouver aussi facilement que possible les séries TV ou films qu’il pourrait apprécier, grâce à un système de recommandation. Netflix calcule ainsi la probabilité que l’utilisateur regarde un titre donné du catalogue de Netflix, et peut ainsi optimiser ces partenariats ou plus globalement sa stratégie marketing. Netflix est l'archétype de la société data driven.
Votre client n’est pas Netflix, mais il a de grandes ambitions !

## Objectif


Vous êtes un Data analyst freelance. Un cinéma en perte de vitesse situé dans la Creuse vous contacte. Il a décidé de passer le cap du digital en créant un site internet taillé pour les locaux. 



Un cinéma en perte de vitesse situé dans la Creuse vous contacte. Il a décidé de passer le cap du digital en créant un site internet taillé pour les locaux. 

Pour aller encore plus loin, il vous demande de créer un moteur de recommandations de films qui à terme, enverra des notifications aux clients via SMS ou mails.

Pour l’instant, aucun client n’a renseigné ses préférences, vous êtes dans une situation de cold start. Mais heureusement, le client vous donne une base de données de films basée sur la plateforme IMDb.

Vous allez commencer par proposer une analyse complète de la base de données (quels sont les pays qui produisent le plus de films ? Quels sont les acteurs les plus présents ? A quelle période ? La durée moyenne des films s’allonge ou se raccourcit avec les années ? Les acteurs de série sont-ils les mêmes qu’au cinéma ? Les acteurs ont en moyenne quel âge ? Quels sont les films les mieux notés ? Partagent-ils des caractéristiques communes ? Etc…) Suite à une première analyse, vous pouvez décider de spécialiser votre cinéma, par exemple sur la “période années 90”, ou alors sur “les films d’action et d’aventure”, afin d’afiner votre analyse.

Après cette étape analytique, sur la fin du projet, vous utiliserez des algorithmes de machine learning pour recommander des films en fonction de films qui ont été appréciés par le spectateur.

## Détails du jeu de données IMDb

Chaque ensemble de données est contenu dans un fichier gzippé au format TSV (tab-separated-values) dans le jeu de caractères UTF-8. La première ligne de chaque fichier contient des en-têtes qui décrivent le contenu de chaque colonne. Un '\N' est utilisé pour indiquer qu'un champ particulier est manquant ou nul pour ce titre/nom. Les ensembles de données disponibles sont les suivants :

### title.akas.tsv.gz - Contains the following information for titles:

titleId (string) - a tconst, an alphanumeric unique identifier of the title

ordering (integer) – a number to uniquely identify rows for a given titleId

title (string) – the localized title

region (string) - the region for this version of the title

language (string) - the language of the title

types (array) - Enumerated set of attributes for this alternative title. One or more of the following: "alternative", "dvd", "festival", "tv", "video", "working", "original", "imdbDisplay". New values may be added in the future without warning

attributes (array) - Additional terms to describe this alternative title, not enumerated

isOriginalTitle (boolean) – 0: not original title; 1: original title


### title.basics.tsv.gz - Contains the following information for titles:

tconst (string) - alphanumeric unique identifier of the title

titleType (string) – the type/format of the title (e.g. movie, short, tvseries, tvepisode, video, etc)

primaryTitle (string) – the more popular title / the title used by the filmmakers on promotional materials at the point of release

originalTitle (string) - original title, in the original language

isAdult (boolean) - 0: non-adult title; 1: adult title

startYear (YYYY) – represents the release year of a title. In the case of TV Series, it is the series start year

endYear (YYYY) – TV Series end year. ‘\N’ for all other title types

runtimeMinutes – primary runtime of the title, in minutes

genres (string array) – includes up to three genres associated with the title


### title.crew.tsv.gz – Contains the director and writer information for all the titles in IMDb. Fields include:

tconst (string) - alphanumeric unique identifier of the title

directors (array of nconsts) - director(s) of the given title

writers (array of nconsts) – writer(s) of the given title


### title.episode.tsv.gz – Contains the tv episode information. Fields include:

tconst (string) - alphanumeric identifier of episode

parentTconst (string) - alphanumeric identifier of the parent TV Series

seasonNumber (integer) – season number the episode belongs to

episodeNumber (integer) – episode number of the tconst in the TV series


### title.principals.tsv.gz – Contains the principal cast/crew for titles

tconst (string) - alphanumeric unique identifier of the title

ordering (integer) – a number to uniquely identify rows for a given titleId

nconst (string) - alphanumeric unique identifier of the name/person

category (string) - the category of job that person was in

job (string) - the specific job title if applicable, else '\N'

characters (string) - the name of the character played if applicable, else '\N'


### title.ratings.tsv.gz – Contains the IMDb rating and votes information for titles:

tconst (string) - alphanumeric unique identifier of the title

averageRating – weighted average of all the individual user ratings

numVotes - number of votes the title has received


### name.basics.tsv.gz – Contains the following information for names:

nconst (string) - alphanumeric unique identifier of the name/person

primaryName (string)– name by which the person is most often credited

birthYear – in YYYY format

deathYear – in YYYY format if applicable, else '\N'

primaryProfession (array of strings)– the top-3 professions of the person

knownForTitles (array of tconsts) – titles the person is known for


## Première exploration de la base de donnée

Voir : Exploration_de_données.ipynb

Dans cette exploration nous constatons qu'enormement d'informations , de films , d'acteurs sont présent dans les bases de donnée. Nous allons devoir remettre tout en ordre et récupérer dans chaques tables les éléments necéssaire à la réalisation de notre système de reccommandation. Beaucoup d'informations sont manquantes, donc un nettoyage préliminaire va devoir être fait.


## Nettoyage de la base de donnée

Voir : Nettoyage_préliminaire.ipynb

La première étape consiste à joindre les notes et le nombre de votes a la table des films, ensuite nous procédons donc au nettoyage des valeurs manquantes.


## Etude des variables

voir : Etude_des_variables.ipynb

Nous étudions ici chaques variables pour pouvoir filtrer un maximum notre base de données. Après cette étude nous decidons de ne prendre que les film d'une durée comprise en 45 minute et 600 minutes, nous observons que quelques genre sont très peu noté. Nous décidons directement de supprimer certain genre comme "Adult" , "Game-Show" , "News" , "Réality-TV" , "Talk-show" qui n'entre pas dans le cadre de notre projet, la recommendation de film pour un cinema. Nous décidons de commencer notre base de donnée a partir de l'année 1920, en effet le nombre de note commence a évoluer a partir de cette année, nous ne pouvons exclure certain films.


##Base de données final

Voir : Base_de_données_final.ipynb

Ici nous ajoutons une colonne "directors" pour afficher les réalisateurs de chaque films, et une colonne "actors" qui regrouperas les acteurs et actrices pour chaque films.
