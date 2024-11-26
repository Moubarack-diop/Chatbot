# Chatbot
## Aperçu du projet
Notre projet consiste à concevoir un chatbot destiné à assister le service client du Groupe Orange Sénégal. Ce chatbot est basé sur l'Intelligence Artificielle et utilise dans sa génération de réponse, un grand modèle de langage naturel (LLM). Dans notre cas nous avons utilisé le Llama2 13B Chat, un modèle de langage développé par Meta AI, disponible via l'interface de Hugging Face Transformers.
Cependant,bien que les LLM soient puissants et capables de générer du contenu créatif, ils peuvent produire des informations obsolètes ou incorrectes car ils sont formés sur des données statiques. Pour surmonter cette limitation, les systèmes de génération augmentée de récupération (RAG) peuvent être utilisés pour connecter notre LLM à des données externes et obtenir des réponses plus fiables. 
Nous avons alors récupéré les données FAQs contenues dans le site de [Orange Assistance](https://assistance.orange.sn/) grace au web scraping q'on a directement ingéré dans le LLM grace au RAG.
## C'est quoi le RAG
