+++
authors = ["xkaynit"]
title = "Menaces sur la supply-chain - NPM"
date = "2026-06-06"
description = "Article présentant les menaces sur la supply chain pour NPM"
tags = [
    "xkaynit",
    "DevSecOps",
]
# series = ["Theme Demo"]
+++

## Menaces sur la supply-chain - NPM

1. **Introduction**

La supply chain (chaîne logistique en français) décrit l'ensemble des processus de production et de livraison d'un produit/logiciel, depuis l'approvisionnement en matières premières jusqu'à la livraison au consommateur. En informatique, cela comprend le développement du logiciel jusqu'à la livraison au client. Les attaquants ne cherchent plus à s'attaquer uniquement au produit final, ils essaient désormais de s'en prendre aux étapes en amont dans le but de compromettre la chaîne d'intégration et de développement continue de manière à délivrer un produit vulnérable. Face à la recrudescence de ce type d'attaque, il est important de comprendre comment se sécuriser efficacement pour limiter les risques liés à la supply chain. 

Dans cet article nous allons nous pencher sur Node Package Manager (NPM). NPM est un gestionnaire de paquets utilisé pour le langage de programmation JavaScript. Il permet d'installer et de gérer des librairies associées à ce langage et est utilisé par plus de 17 millions de développeurs selon NPM lui-même. Il est ainsi possible de créer un projet, de télécharger des paquets ou encore de les mettre à jour. NPM fait donc partie intégrante de la chaîne d'approvisionnement en allant télécharger les paquets nécessaires à la bonne exécution du projet. 

2. **Exemples réels d'attaques**

Il existe plusieurs attaques récentes liées à la CI/CD, j'ai choisi d'en présenter deux qui ont eu un impact important de par l'utilisation de la librairie par la communauté mais également la manière dont cela s'est produit. J'aurais également pu citer l'attaque sur le dépôt Github de Trivy suite à un token non révoqué sur Github. 


### Axios

La première attaque concerne le paquet Axios et s'est déroulée au mois de mars 2026. C'est un paquet qui a plus de 100 millions de téléchargements quotidiens et est présent dans une majorité d'applications car il permet notamment à une application web d'interagir avec des API. L'attaquant a eu accès à un compte utilisateur d'un des mainteneurs du paquet Axios. Il s'est ensuite servi d'une fonctionnalité populaire sous NPM et transparente pour l'utilisateur : le lifecycle hook. Cette fonctionnalité permet d'exécuter un script dis de "post-installation" pour le paquet qui a ici été détourné pour aller exécuter du code malveillant injecté par l'attaquant grâce à son accès au dépôt. 

Il a ensuite publié les modifications dans une fausse dépendance du paquet Axios (plain-crypto-js@4.2.1) dont le but était d'exécuter son script de post-installation qui, selon l'OS déployait un RAT (Remote Access Trojan) permettant à l'attaquant d'avoir un contrôle à distance sur l'équipement cible. Tout le trafic envoyé se rapprochait d'un échange classique NPM pour masquer ces pistes et passer inaperçu auprès des outils de détection. 

L'incident a rapidement été détecté et les correctifs associés appliqués (environ 40 minutes). Les paquets malveillants portaient la signature du groupe UNC109 (nord-coréen).

Source : [Attaque Axios](https://itsocial.fr/cybersecurite/cybersecurite-actualites/anatomie-dune-attaque-comment-axios-a-servi-de-vecteur-a-un-cheval-de-troie-multiplateforme-dissimule-dans-une-dependance-npm/)

### Github

Cette seconde attaque s'est quant à elle déroulée il y a quelques jours à l'heure où cet article est écrit. Pour rappel, Github est une plateforme étant utilisée partout dans le monde et permettant de partager ses projets avec un historique de ce qui a été effectué. L'attaquant s'est cette fois-ci servi d'une extension VS Code malveillante qui a permis de compromettre un compte d'un des mainteneurs de Github.

Il a ainsi eu accès à des milliers (environ 3800) de dépôts internes de l'entreprise. Cela n'a pas impacté les dépôts publics et privés des utilisateurs selon les constatations. En effet, le groupe TeamPCP a revendiqué être à l'origine de l'attaque et a mis en vente les données pour 50 000 dollars. Lorsque l'on installe une extension VS Code il n'y a qu'à cliquer sur Installer et le logiciel s'occupe du reste. La marketplace de ses extensions est gérée par Microsoft qui n'a pas détecté de comportement malveillant dans celle installée par le développeur. 

Les mesures et correctifs nécessaires à la situation ont directement été appliqués par Github. 

Source : [Attaque Github](https://korben.info/github-hack-vs-code-extension.html)

3. **Mesures de sécurité**

Une fois que l'on à ces exemples en tête, il faut désormais pouvoir limiter la zone d'exposition de la supply-chain. Ce chapitre sera donc découpé en une partie qui se concentrera sur NPM et une autre plus globale regroupant des bonnes pratiques dans l'optique de sécuriser et limiter les risques. 

### NPM

Voici quelques recommandations applicables dans le cadre de l'utilisation de NPM dans un processus de CI/CD :

- **Désactivation des scripts lifecycle hooks** : L'option --ignore-scripts durant un npm install va permettre de limiter l'exécution des scripts tiers. Cela peut cependant faire dysfonctionner certaines dépendances, il est alors possible d'ajouter le module **allow-scripts** pour autoriser uniquement certains paquets de confiance à pouvoir exécuter ces scripts.
- **Utilisation de l'attribut minimumReleaseAge** : Cet attribut est pertinent car il permet de télécharger une nouvelle version d'un paquet après un temps défini. La majorité des paquets malveillants étant découverts rapidement (quelques heures), mettre cet attribut à une valeur de deux ou trois jours permet de se protéger du téléchargement d'un paquet compromis.
- **Utilisation de pnpm** : Ce gestionnaire de paquets embarque des fonctionnalités de sécurité nativement que n'a pas NPM. Par exemple il embarque le paquet **allow-scripts** par défaut, il embarque également un attribut nommé TrustPolicy qui fait en sorte qu'une nouvelle version d'un paquet ne puisse être téléchargée si le niveau de confiance de celle-ci a baissé par rapport à la précédente (système basé sur la réputation du mainteneur). 

### Global

Ces recommandations permettront de limiter l'impact si de nouveaux paquets viennent à être compromis. Les mesures de sécurité ci-dessous s'appliquent dans un contexte plus global et pas uniquement à NPM, ce sont des bonnes pratiques liées à la CI/CD :

- **Utilisation du hash SHA256** : Lorsque cela est possible, il est nécessaire de marquer les versions des composants utilisées avec le hash de la version correspondante. Cela permet de se prémunir d'une modification dans la version du paquet utilisée(exemple : mise en place d'une backdoor). 
- **Secrets, clé API... en clair dans le code** : Aucun token, clé API ou mot de passe ne doit être stocké en clair dans le code ou les pipelines CI/CD. Un outil comme [Gitleaks](https://github.com/gitleaks/gitleaks) peut être utilisé pour s'assurer qu'aucune information sensible n'est affichée en clair dans le code. De plus, un commit où des informations sensibles seraient en clair doit être systématiquement refusée. 
- **Vérification des librairies utilisées** : Avant de télécharger et d'utiliser une librairie, il est nécessaire de vérifier qu'elle est de confiance. Cela peut se faire par le biais de la vérification de l'activité du mainteneur ainsi que sa popularité notamment. Cela n'est cependant pas une science exacte et un paquet populaire et actif peut être un paquet malveillant dans certains cas. 
- **Gestion des tokens** : Une gestion des tokens et plus globalement des accès doit être mise en place au sein du projet. Cela comprend des droits granulaires sur les tokens fournis aux différentes équipes avec uniquement les droits nécessaires pour chacune d'elles et également une rotation des tokens sur des périodes courtes (environ 60 jours) doit être mise en place.

4. **Conclusion**

Désormais, les attaquants ne ciblent plus uniquement le produit final mais bien la chaîne logicielle complète dans le but de compromettre celui-ci dès la phase d'intégration et de déploiement. Face à cela, les développeurs et équipe de sécurité doivent être vigilants sur les librairies utilisées dans le code et celles téléchargées. En cas de suspicion de compromission ou comprimission avérée, il est nécessaire de changer/révoquer chaque token, mots de passe et clés API utilisés au sein du projet concerné ainsi que d'isoler celui-ci. 