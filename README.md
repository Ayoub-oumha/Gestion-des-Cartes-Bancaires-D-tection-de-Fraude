# 💳 Gestion des Cartes Bancaires & Détection de Fraude

## 📌 Contexte du projet
La gestion des cartes bancaires et la détection de fraude sont devenues une priorité pour les banques.  

Chaque client possède une ou plusieurs cartes (débit, crédit, prépayée). Ces cartes génèrent des transactions diverses (paiement, retrait, achat en ligne).  

Les banques doivent :
- Gérer le cycle de vie d’une carte (création, activation, suspension, renouvellement).
- Suivre en temps réel les opérations liées aux cartes.
- Détecter automatiquement des comportements suspects (ex : achats dans deux pays différents à quelques minutes d’intervalle).
- Alerter les responsables ou bloquer la carte en cas de fraude potentielle.

---

## 🏗️ Structure de l’application

### Couche présentation (UI/Menu)
- Interface textuelle avec navigation par menu.

### Couche métier (Services)
- Logique applicative pour gérer les cartes, les transactions associées et les alertes de fraude.

### Couche Entity
- Objets persistants : `Client`, `Carte`, `OperationCarte`, `AlerteFraude`.
- `record` pour `OperationCarte` (immutable).
- `sealed` pour `Carte` (`Débit`, `Crédit`, `Prépayée`).

### Couche DAO
- Gestion CRUD pour cartes, opérations et alertes.

### Couche utilitaire
- Vérification des règles de fraude.  
- Gestion des dates et lieux.  
- Génération de numéros de carte uniques (simulés).  

---

## 📦 Contenu des classes

### Couche Entity (Modèle)
- **Client (record)** : `id`, `nom`, `email`, `téléphone`.  
- **Carte (sealed class)** : `id`, `numero`, `dateExpiration`, `statut (ACTIVE, SUSPENDUE, BLOQUEE)`, `idClient`.  
  - `CarteDebit` : plafond journalier.  
  - `CarteCredit` : plafond mensuel, taux d’intérêt.  
  - `CartePrepayee` : solde disponible.  
- **OperationCarte (record)** : `id`, `date`, `montant`, `type (ACHAT, RETRAIT, PAIEMENTENLIGNE)`, `lieu`, `idCarte`.  
- **AlerteFraude (record)** : `id`, `description`, `niveau (INFO, AVERTISSEMENT, CRITIQUE)`, `idCarte`.  

### Couche DAO
- **ClientDAO** : CRUD client.  
- **CarteDAO** : CRUD carte, recherche par client.  
- **OperationDAO** : CRUD opération, recherche par carte, filtrage par type/date.  
- **AlerteDAO** : CRUD alerte, recherche par carte.  

### Couche Services (Logique Métier)
- **ClientService** : gestion des clients, recherche par email/téléphone.  
- **CarteService** :
  - Créer/activer/bloquer une carte.  
  - Vérifier le plafond avant autorisation d’une opération.  
- **OperationService** :
  - Enregistrer une opération (paiement, retrait, achat).  
  - Rechercher les opérations par carte ou client.  
- **FraudeService** :
  - Détecter les anomalies (montant élevé, opérations rapprochées dans des lieux différents, dépassement de plafond).  
  - Générer une alerte de fraude.  
- **RapportService** :
  - Top 5 des cartes les plus utilisées.  
  - Statistiques mensuelles des opérations par type.  
  - Liste des cartes bloquées ou suspectes.  
- **ImportExportService** :
  - Importer un fichier Excel contenant des opérations ou cartes.  

### Couche UI (Interface Utilisateur)
Menu textuel permettant de :  
- Créer un client.  
- Émettre une carte (débit, crédit, prépayée).  
- Effectuer une opération (achat, retrait, paiement en ligne).  
- Consulter l’historique d’une carte.  
- Lancer une analyse des fraudes.  
- Bloquer/suspendre une carte.  

---

## 🗄️ Base de données

### Tables
- **Client** : `id`, `nom`, `email`, `téléphone`.  
- **Carte** : `id`, `numero`, `dateExpiration`, `statut`, `typeCarte`, `idClient`.  
- **OperationCarte** : `id`, `date`, `montant`, `type`, `lieu`, `idCarte`.  
- **AlerteFraude** : `id`, `description`, `niveau`, `idCarte`.  

### Relations
- `1..n` entre **Client** et **Carte**.  
- `1..n` entre **Carte** et **OperationCarte**.  
- `1..n` entre **Carte** et **AlerteFraude**.  

---

## ⚙️ Exigences techniques
- **Java 17** (`record`, `sealed`, `Stream API`, `Optional`).  
- **JDBC + MySQL**.  
- **Architecture en couches** (Entity, DAO, Service, UI).  
- Gestion des exceptions.  
- Utilisation de **Git**.  
- Génération d’un **JAR exécutable**.  
