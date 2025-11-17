# Assistant IA RAG interne – Projet 1

> **J’ai développé hier en 2h un chatbot IA à la Gare du Nord.**  
> Ce repo contient la version propre de ce projet. 🎯

## 🧩 Contexte

L’objectif de ce projet est de construire un **assistant IA interne** capable de :

- Lire des documents internes (PDF / TXT)
- Indexer leur contenu de façon sémantique
- Répondre aux questions des collaborateurs
- Citer les documents sources utilisés

C’est un cas d’usage typique des grands groupes qui cherchent à transformer leur documentation en copilote intelligent pour les équipes (RH, IT, support, etc.).

---

## ⚙️ Stack technique

- **Python** (Google Colab)
- **OpenAI API**
  - `text-embedding-3-small` pour les embeddings
  - `gpt-4.1-mini` pour la génération de réponses
- **FAISS** : index vectoriel pour la recherche sémantique
- **LangChain** : chunking avancé des documents
- **PyPDF / pypdf** : extraction de texte depuis les PDF
- **Gradio** : interface chat type “assistant interne”

---

## 🏗️ Architecture du système

1. **Ingestion des documents**
   - Upload de fichiers `.pdf` ou `.txt`
   - Extraction du texte
2. **Préparation / Chunking**
   - Découpage en morceaux de ~800 caractères avec overlap
   - Chunking “intelligent” via `RecursiveCharacterTextSplitter`
3. **Indexation**
   - Calcul des embeddings (OpenAI)
   - Normalisation et stockage dans un index FAISS
   - Cache des embeddings et des métadonnées (`faiss_index.bin`, `chunks_meta.json`)
4. **Retrieval + Reranking**
   - Embedding de la question
   - Recherche des `top_k` passages via FAISS
   - Reranking léger via LLM pour garder les passages les plus pertinents
5. **Génération de réponse (RAG)**
   - Construction d’un CONTEXTE à partir des meilleurs chunks
   - Prompt avec guardrails :
     - ne répondre que sur la base du contexte
     - signaler si l’info n’est pas disponible
6. **Interface utilisateur**
   - Chat Gradio (`gr.ChatInterface`)
   - Historique des échanges
   - Affichage des sources utilisées

---

## 🚀 Utilisation (version Colab)

1. Ouvrir le notebook :

```text
notebooks/assistant_rag_gare_du_nord.ipynb
Renseigner sa clé API OpenAI dans la cellule prévue.

2. Exécuter les cellules dans l’ordre :

3. installation des dépendances

4. configuration client OpenAI

5. ingestion des documents

6. construction / chargement de l’index FAISS

7. lancement de l’interface Gradio

8. Uploader des documents internes (procédures, notes, etc.)

9. Poser des questions dans l’interface chat.
