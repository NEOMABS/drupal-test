# 📌 Test Technique Drupal 10 – Développeur Backend

**Durée estimée** : 1h30 à 2h30 (à répartir selon votre rythme).

**Objectif** : Évaluer vos compétences en développement **Drupal 10**, **PHP**, et **architecture de modules custom**.

---
---
---

***Consignes Générales :***
   
- Respectez les conventions Drupal : nommage des classes (PSR-4), fichiers, et méthodes.

- Optimisez votre code : vous pouvez installer Composer pour gérer le cache Drupal si besoin et drush pour installation / activation module

- Documentation : n'hésitez pas à ajouter des commentaires pertinents dans votre code si besoin.

- Livraison : poussez votre code dans une branche dédiée ;
  

## **🔹 Exercice 1 : Module de Statistiques de Contenu**
**Objectif** : Créer un module custom qui **compte automatiquement** le nombre de contenus par type (`formations`, `actualités`, `événements`) et affiche un tableau récapitulatif dans une page d’administration.

### **📋 Tâches à réaliser**
1. **Créer manuellement les types de contenu** :
   - **Formation** (`machine_name: programmes`)
     - Champs : `title`, `body` (texte long), `cost` (date).
   - **Actualité** (`machine_name: news`)
     - Champs : `title`, `body`, `publication_date` (date), `is_homepage` (boolean).
   - **Événement** (`machine_name: events`)
     - Champs : `title`, `description` (texte long), `start_date` (date), `end_date` (date), `location` (liste de sélection : Rouen, Paris, Reims).

2. **Créer au moins 2-3 contenus par type** (via `/node/add/programmes`, `/node/add/news`, `/node/add/events`).

3. **Développer un module custom `content_stats`** avec :
   - Un **service** (`StatsService`) qui :
     - Compte le **nombre total** de contenus par type.
     - Compte le **nombre de contenus publiés/non publiés** par type.
   - Un **contrôleur** (`StatsController`) qui :
     - Affiche un **tableau récapitulatif** sur la page `/admin/content/stats`.
     - Format attendu :
       ```
       Type          Total | Publié | Non publié
       ---------------------------------------------
       formation   | 5     | 4      | 1
       actualite   | 3     | 3      | 0
       evenement   | 2     | 1      | 1
       ```

   - Une **route** (`/admin/content/stats`) accessible depuis le menu d’administration.

5. **Activer le module** et vérifier que la page `/admin/content/stats` fonctionne.

---
---
---

## **🔹 Exercice 2 : Filtrage des Événements par Date et Catégorie**
**Objectif** : Ajouter une **taxonomie de catégories** aux événements et créer une page pour les filtrer par **date** et **catégorie**.

### **📋 Tâches à réaliser**
1. **Créer un vocabulaire de taxonomie** :
   - Nom : **Catégorie** (`machine_name: category`).
   - Ajouter les **termes** :
     - Conférence
     - Online
     - Webinaire

2. **Ajouter un champ de référence à la taxonomie** au type de contenu **Événement** :
   - Nom du champ : **Catégorie** (`field_category`).
   - Type : **Référence à un terme de taxonomie** (vocabulaire `Catégory`).
   - Autoriser **plusieurs valeurs**.

3. **Alimenter les événements existants** avec des catégories (au moins 1-2 par terme).

4. **Créer une page de filtrage** (`/events`) **au choix** :
   - **Option A : Avec Views** :
     - Créer une **vue** (`Filtre Événements`) qui :
       - Affiche une **liste des événements** (titre, date, catégorie).
       - Ajoute un **formulaire de filtre** pour :
         - **Date** (filtre "à partir de" ou "avant/après").
         - **Catégorie** (dropdown ou checkboxes).
       - Définir le chemin de la page sur `/events`.
   - **Option B : Avec un contrôleur custom** :
     - Créer un **module `event_filter`** avec :
       - Un **formulaire** (`EventFilterForm`) pour filtrer par date et catégorie.
       - Un **contrôleur** (`EventFilterController`) qui :
         - Récupère les événements filtrés (via `EntityQuery`).
         - Affiche les résultats dans un tableau.
       - Une **route** `/events`.

5. **Tester** que le filtrage fonctionne correctement.

---
---
---

## **🔹 Exercice 3 (Bonus) : API REST pour les Contenus**
**Objectif** : Créer un module custom qui **expose des endpoints REST** (GET) pour récupérer les contenus (formations, actualités, événements) au format JSON.

### **📋 Tâches à réaliser**
1. **Créer un module `content_api`** avec :
   - **3 ressources REST** (une par type de contenu) :
     - `/api/programmes` → Liste toutes les formations.
     - `/api/news` → Liste toutes les actualités.
     - `/api/events` → Liste tous les événements.
   - Chaque endpoint doit retourner un **JSON** avec tous les champs :
     - `id` (nid du nœud)
     - `title` (titre du contenu)
     - `body` (corps du contenu, si applicable)
     - `date` (date associée, si applicable)

3. **Tester les endpoints** avec `curl` ou Postman :
   ```bash
   curl http://url-du-site/api/programmes
   curl http://url-du-site/api/news
   curl http://url-du-site/api/events
