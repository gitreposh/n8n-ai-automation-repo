Projet n8n et Strapi avec PostgreSQL
Introduction
Ce projet vise à créer un workflow automatisé avec n8n et un backend CMS avec Strapi, en utilisant PostgreSQL comme base de données. 
L'objectif est de générer et gérer des articles de blog de manière automatisée, avec une image Docker pour faciliter le déploiement.

Prérequis
Docker et Docker Compose installés
Node.js et npm installés
PostgreSQL installé
Accès à une API Hugging Face pour le modèle de génération de contenu

Étapes du Projet
1. Installation et configuration de la base de données PostgreSQL.
2. Installation et création du projet Strapi.
3. Création collections nécessaires via l'interface admin de Strapi:
	Authors avec les champs: Name, Bio
	Blogs avec les champs: Title, Content, Author (relation avec Authors)
	Logs avec les champs: Message, Error
4. Utiliser l'interface graphique de n8n pour créer le workflow suivant:
	Webhook : Ce noeud reçoit une requête initiale via un webhook pour activer le workflow en recevant une requête HTTP via l'url http://localhost:5678/webhook-test/blog.
	Get Authors : Récupère tous les auteurs depuis Strapi.
	Check Authors : Vérifie si les données des auteurs récupérées ne sont pas vides. Si les auteurs existent, passe à l'étape suivante; sinon, génère des auteurs.
	Generate Author : Génère un profil d'auteur en utilisant un modèle de langage.Crée un profil d'auteur comprenant le nom et la biographie.
	Edit Fields : Édite les champs de données pour l'auteur. Prépare les données de l'auteur pour être enregistrées.
	If : Vérifie si les données de l'auteur ne sont pas vides. Si les données de l'auteur sont valides, crée l'auteur; sinon, enregistre une erreur.
	Mapping datas1 : Mappe les données pour la création d'auteur dans Strapi. Prépare les données pour être enregistrées dans Strapi.
	Create Author : Crée un nouvel auteur dans Strapi. Enregistre le profil de l'auteur dans le CMS Strapi.
	Edit error log ; Édite le log d'erreur en cas de problème. Enregistre les détails de l'erreur dans un log.
	Edit error log1 : Édite le log d'erreur si le contenu existe déjà. Enregistre les détails de l'erreur dans un log.
	Create logs : Crée un log dans Strapi. Enregistre les logs d'erreur dans le CMS Strapi.
	Hugging Face Inference Model : Utilisé pour générer un auteur, des sujets de blog et du contenu détaillé.
	Generate topics : Génère des sujets de blog en utilisant un modèle de langage de Hugging Face. Demande à l'IA de générer 5 sujets de blog sur les dernières tendances en technologie.
	Check topics : Vérifie si les sujets générés ne sont pas vides. Si des sujets sont générés, les édite; sinon, enregistre une erreur.
	Edit topics : Édite les sujets générés pour les structurer correctement. Formate les sujets pour un traitement ultérieur.
	Loop Over topics : Boucle sur chaque sujet généré. Traite chaque sujet individuellement.
	write blog content : Génère le contenu détaillé du blog pour chaque sujet. Utilise l'IA pour écrire un article de blog détaillé sur le sujet donné.
	Mapping datas : Mappe les données pour la création de contenu dans Strapi. Prépare les données pour être enregistrées dans Strapi.
	Create content : Crée un nouveau contenu de blog dans Strapi. Enregistre le contenu du blog dans le CMS Strapi.
	Strapi : Récupère tous les blogs depuis Strapi et les retourne au webhook.
	Respond to Webhook : Répond à la requête HTTP initiale et retourne la réponse finale au client HTTP http://localhost:1337/api/blogs?populate=*
5. Création de l'Image Docker : Construction et Lancement des Conteneurs docker-compose up --build
6. config post déploiement : 
  - Sur http://localhost:1337/ --> creer un compte admin --> settings/API Tokens/create new API Token full access
	- Sur Users & Permissions plugin / roles / public / autoriser les acces en lecture aux collections
	- Sur http://localhost:5678/ --> créer un compte admin --> ouvrir le workflow StrapiBlog ou se rendre sur http://localhost:5678/workflow/t7oOzrI2kJBPUUJR
	- Ouvrir un noeud strapi et configurez la connexion à strapi avec votre Token en ajoutant l'url : http://strapi:1337/ et version 4 
	- Ouvrir le noeud du model huggingface et ajouter votre apikey
	- Demarrez le test du workflow puis rendez vous sur http://localhost:5678/webhook-test/blog pour le déclancher 
