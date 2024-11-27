# Chatbot

## Aperçu du projet
Notre projet consiste à concevoir un chatbot destiné à assister le service client du Groupe Orange Sénégal. Ce chatbot est basé sur l'Intelligence Artificielle et utilise dans sa génération de réponse, un grand modèle de langage naturel (LLM). Dans notre cas nous avons utilisé le Llama2 13B Chat, un modèle de langage développé par Meta AI, disponible via l'interface de Hugging Face Transformers.
Cependant,bien que les LLM soient puissants et capables de générer du contenu créatif, ils peuvent produire des informations obsolètes ou incorrectes car ils sont formés sur des données statiques. Pour surmonter cette limitation, les systèmes de génération augmentée de récupération (RAG) peuvent être utilisés pour connecter notre LLM à des données externes et obtenir des réponses plus fiables. 
Nous avons alors récupéré les données FAQs contenues dans le site de [Orange Assistance](https://assistance.orange.sn/) grace au web scraping qu'on a directement ingéré dans le LLM grace au RAG.
## C'est quoi le RAG

Les LLM sont formés sur un corpus de données volumineux mais fixe, ce qui limite leur capacité à raisonner sur des informations privées ou récentes. Le fine tuning est un moyen d'atténuer ce problème, mais il n'est souvent pas bien adapté au rappel factuel et peut être coûteux. La génération augmentée de récupération (RAG) est devenue un mécanisme populaire et puissant pour élargir la base de connaissances d'un LLM, en utilisant des documents récupérés à partir d'une source de données externe pour fonder la génération de LLM via l'injection de contexte.

![Workflow_rag (1)](https://github.com/user-attachments/assets/d8179e97-5b5f-4c6c-9688-0aee3b5eff3a)

L'architecture ci-dessus est une excellente introduction aux bases de l'injection de contexte. Nous allons résumer le processus d'injection de contexte comme suit :
1. D'abord nous collectons d'abord les données extraites du site de Orange Assistance grace au web scraping. Ces dernières sont ensuite transformées en fichier CSV.
   
2. Ensuite, les données sont chargées (text loader) puis segmentées (text splitting). Les segments (tokens) sont essentiellement une courte chaîne de caractères, généralement de 4 caractères. Par exemple, le mot « génératif » peut être divisé en segments comme « ge », « n », « erat » et « ive ». Le LLM traite les données sous forme de segments.
 
3. Les tokens seront introduits dans un modèle d'embedding qui convertit les tokens en vecteurs. L'embeddings sont la façon dont les mots et les phrases sont représentés dans un espace vectoriel. Il existe plusieurs modèles d'embedding dans le marché dans notre cas nous avons choisi celui de Hugging Face.
  
4. Les vecteurs produits à partir du modèle d'embedding sont stockés dans des bases de données vectorielles. Dans notre cas, nous utiliserons ChromaDB.
   
5. Passons maintenant à la partie intéressante. Lorsqu'un utilisateur pose une question, nous convertissons sa requête en vecteur et recherchons les vecteurs les plus proches dans la base de données. Essentiellement, ce processus localise les fragments de texte les plus pertinents pour la requête de l'utilisateur et les reconvertit en texte.
   
6. La question de l'utilisateur et les fragments de texte pertinents (contexte) seront inclus dans un prompt qui sera fourni au LLM. Sans aucune modification du LLM d'origine, le modèle peut fournir des réponses impressionnantes à la requête en utilisant le contexte injecté.

7. La réponse générée et la question précédente de l'utilisateur seront introduites dans le prompt pour servir d'historique des conversations. Le LLM pourra s'en inspiré lorsque l'utilisateur pose une autre question en rapport avec les question et réponse précédentes.

## Technologies utilisées

### 🦜️🔗 Langchain
LangChain est un framework open-source conçu pour faciliter le développement d'applications basées sur des modèles de langage (LLMs). Il fournit des ouils pour les pipelines de traitement de langage, gère les conversations longues en gérant l'historique contextuel. Il facilite l'intégration des bases de connaissances de meme que la conception des prompts.

### 🤗 Hugging Face
Hugging Face est une plateforme qui offre la posibilité d'accéder à des modèles de langage préentrainés Open source. Pour le cadre de notre projet, nous y avons téléchargé le modèle Llama-2-13b-chat-hf. C'est un modèle gratuit fourni par Meta AI. Néanmoins il existe d'autres LLM performants sur le marché tels que GPT, Gemini,...; leur utilisation étant payante, nous avons préféré travailler avec des LLM gratuits.

### ChromaDB
ChromaDB est une base de données vectorielle spécialement conçue pour stocker et rechercher des données vectorielles. Elle va nous permettre de gérer nos données vectorisées Ces vecteurs seront utilisés pour effectuer des recherches basées sur la similarité.

## Installation
Pour le développement de notre Chatbot nous avons utilisé Google Colab. C'est une plateforme qui offre gratuitement de la mémoire GPU indispensable au chargement de notre LLM (llama2 13B Chat). Cependant cette mémoire est assez limitée si on veut charger des modèles avec de plus grands paramètres. Fort heuresement, Hugging Face a développé des modèles de quantification avec la bibliothèque Bitsandbytes.La quantification va ainsi améliorer les performances en réduisant les besoins en bande passante mémoire GPU et en augmentant l'utilisation du cache.
# Chatbot

## Aperçu du projet
Notre projet consiste à concevoir un chatbot destiné à assister le service client du Groupe Orange Sénégal. Ce chatbot est basé sur l'Intelligence Artificielle et utilise dans sa génération de réponse, un grand modèle de langage naturel (LLM). Dans notre cas nous avons utilisé le Llama2 13B Chat, un modèle de langage développé par Meta AI, disponible via l'interface de Hugging Face Transformers.
Cependant,bien que les LLM soient puissants et capables de générer du contenu créatif, ils peuvent produire des informations obsolètes ou incorrectes car ils sont formés sur des données statiques. Pour surmonter cette limitation, les systèmes de génération augmentée de récupération (RAG) peuvent être utilisés pour connecter notre LLM à des données externes et obtenir des réponses plus fiables. 
Nous avons alors récupéré les données FAQs contenues dans le site de [Orange Assistance](https://assistance.orange.sn/) grace au web scraping q'on a directement ingéré dans le LLM grace au RAG.
## C'est quoi le RAG

Les LLM sont formés sur un corpus de données volumineux mais fixe, ce qui limite leur capacité à raisonner sur des informations privées ou récentes. Le fine tuning est un moyen d'atténuer ce problème, mais il n'est souvent pas bien adapté au rappel factuel et peut être coûteux. La génération augmentée de récupération (RAG) est devenue un mécanisme populaire et puissant pour élargir la base de connaissances d'un LLM, en utilisant des documents récupérés à partir d'une source de données externe pour fonder la génération de LLM via l'injection de contexte.

![Workflow_rag (1)](https://github.com/user-attachments/assets/d8179e97-5b5f-4c6c-9688-0aee3b5eff3a)

L'architecture ci-dessus est une excellente introduction aux bases de l'injection de contexte. Nous allons résumer le processus d'injection de contexte comme suit :
1. D'abord nous collectons d'abord les données extraites du site de Orange Assistance grace au web scraping. Ces dernières sont ensuite transformées en fichier CSV.
   
2. Ensuite, les données sont chargées (text loader) puis segmentées (text splitting). Les segments (tokens) sont essentiellement une courte chaîne de caractères, généralement de 4 caractères. Par exemple, le mot « génératif » peut être divisé en segments comme « ge », « n », « erat » et « ive ». Le LLM traite les données sous forme de segments.
 
3. Les tokens seront introduits dans un modèle d'embedding qui convertit les tokens en vecteurs. L'embeddings sont la façon dont les mots et les phrases sont représentés dans un espace vectoriel.Il existe plusieurs modèles d'embedding dans le marché dans notre cas nous avons choisi celui de Hugging Face.
  
4. Les vecteurs produits à partir du modèle d'embedding sont stockés dans des bases de données vectorielles. Dans notre cas, nous utiliserons ChromaDB.
   
5. Passons maintenant à la partie intéressante. Lorsqu'un utilisateur pose une question, nous convertissons sa requête en vecteur et recherchons les vecteurs les plus proches dans la base de données. Essentiellement, ce processus localise les fragments de texte les plus pertinents pour la requête de l'utilisateur et les reconvertit en texte.
   
6. La question de l'utilisateur et les fragments de texte pertinents (contexte) seront inclus dans un prompt qui sera fourni au LLM. Sans aucune modification du LLM d'origine, le modèle peut fournir des réponses impressionnantes à la requête en utilisant le contexte injecté.

7. La réponse générée et la question précédente de l'utilisateur seront introduites dans le prompt pour servir d'historique des conversations. Le LLM pourra s'en inspiré lorsque l'utilisateur pose une autre question en rapport avec les question et réponse précédentes.

## Technologies utilisées

### 🦜️🔗 Langchain
LangChain est un framework open-source conçu pour faciliter le développement d'applications basées sur des modèles de langage (LLMs). Il fournit des ouils pour les pipelines de traitement de langage, gère les conversations longues en gérant l'historique contextuel. Il facilite l'intégration des bases de connaissances de meme que la conception des prompts.

### 🤗 Hugging Face
Hugging Face est une plateforme qui offre la posibilité d'accéder à des modèles de langage préentrainés Open source. Pour le cadre de notre projet, nous y avons téléchargé le modèle Llama-2-13b-chat-hf. C'est un modèle gratuit fourni par Meta AI. Néanmoins il existe d'autres LLM performants sur le marché tels que GPT, Gemini,...; leur utilisation étant payante, nous avons préféré travailler avec des LLM gratuits.

### ChromaDB
ChromaDB est une base de données vectorielle spécialement conçue pour stocker et rechercher des données vectorielles. Elle va nous permettre de gérer nos données vectorisées Ces vecteurs seront utilisés pour effectuer des recherches basées sur la similarité.

## Installation
Pour le développement de notre Chatbot nous avons utilisé Google Colab. C'est une plateforme qui offre gratuitement de la mémoire GPU indispensable au chargement de notre LLM (llama2 13B Chat). Cependant cette mémoire est assez limitée si on veut charger des modèles avec de plus grands paramètres. Fort heuresement, Hugging Face a développé des modèles de quantification avec la bibliothèque Bitsandbytes.La quantification va ainsi améliorer les performances en réduisant les besoins en bande passante mémoire GPU et en augmentant l'utilisation du cache.

1. Pour accéder au modèle llama2 13B Chat via Hugging Face veuillez suivre le étapes suivantes:
   `Etape 1: Création de compte Hugging Face et récupération du Token d'accés`
   Pour télécharger des modèles depuis Hugging Face, vous devez d'abord avoir un compte Huggingface. Inscrivez-vous à cette [URL](https://huggingface.co/welcome) , puis 
   obtenez votre token à cet endroit.
    ![tokenacess](https://github.com/user-attachments/assets/5ee429fd-386e-4e5d-bdf0-8187e707fa43)

   `Étape 2. Demande de Llama 2 `
   Pour télécharger et utiliser le modèle Llama 2, il vous suffit de remplir le formulaire de Meta pour demander l'accès. Veuillez noter que l'utilisation de Llama 2 est 
   soumise à l'acceptation du contrat de licence Meta. Après avoir rempli le formulaire, vous recevrez un email contenant une URL qui pourra être utilisée pour télécharger 
   le modèle.

   `Étape 3. Accédez au modèle Llama-2 sur Huggingface , soumettez le formulaire d'accès`
   _Veuillez noter que l'e-mail que vous saisissez à l'étape 2 doit correspondre à celui que vous avez utilisé pour créer votre compte Hugging Face à l'étape 1. S'ils ne 
   correspondent pas, l'étape 3 ne réussira pas._
   Après quelques minutes, une fois votre soumission approuvée, vous recevrez le mail suivant. À ce stade, toutes les étapes préalables ont été complétées.
   
   ![llamaaccess](https://github.com/user-attachments/assets/c8e1b77d-0b63-4d7b-8a61-78a2fe0bc01d)
   
 2. Pour l'intégration du Chatbot sur Télégram veuillez suivre ces étapes suivantes:
     `Etape 1. Ouvrir Telegram `
         Lancez l’application Telegram et recherchez le bot BotFather.  `BotFather est un bot officiel de Telegram utilisé pour créer et gérer d’autres bots.

     `Etape 2. Créer un Nouveau Bot `
         – Envoyez la commande `/newbot` à BotFather.
         – Donnez un nom et un nom d’utilisateur (username) à votre bot.
         – BotFather vous fournira un Token d’API. Conservez ce token en lieu sûr, car il sera inséré dans le code.
