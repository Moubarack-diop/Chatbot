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
3. Les tokens seront introduits dans un modèle d'embedding qui convertit les tokens en vecteurs. L'embeddings sont la façon dont les mots et les phrases sont représentés dans un espace vectoriel.
4. Les vecteurs produits à partir du modèle d'embedding sont stockés dans des bases de données vectorielles. Dans notre cas, nous utiliserons ChromaDB.
5. Passons maintenant à la partie intéressante. Lorsqu'un utilisateur pose une question, nous convertissons sa requête en vecteur et recherchons les vecteurs les plus proches dans la base de données. Essentiellement, ce processus localise les fragments de texte les plus pertinents pour la requête de l'utilisateur et les reconvertit en texte.
La question de l'utilisateur et les fragments de texte pertinents (contexte) seront inclus dans un prompt qui sera fourni au LLM. Sans aucune modification du LLM d'origine, le modèle peut fournir des réponses impressionnantes à la requête en utilisant le contexte injecté.
