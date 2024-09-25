
# Schéma de base de données CRM

Ce document décrit un schéma de base de données pour un CRM (Customer Relationship Management). Ce schéma est conçu pour gérer les contacts, les entreprises, les opportunités de vente, et les interactions avec les clients.

## Table des `contacts`

La table `contacts` stocke les informations personnelles des clients ou prospects. Chaque contact est lié à une entreprise via une clé étrangère.

```sql
-- Table des contacts
CREATE TABLE contacts (
    contact_id INT PRIMARY KEY AUTO_INCREMENT, -- Identifiant unique pour chaque contact
    first_name VARCHAR(50), -- Prénom du contact
    last_name VARCHAR(50), -- Nom de famille du contact
    email VARCHAR(100) UNIQUE, -- Adresse email du contact (doit être unique)
    phone VARCHAR(20), -- Numéro de téléphone du contact
    company_id INT, -- Référence à l'entreprise associée (clé étrangère vers la table des entreprises)
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- Date de création du contact
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, -- Date de mise à jour du contact
    FOREIGN KEY (company_id) REFERENCES companies(company_id) -- Clé étrangère vers la table des entreprises
);
```

## Table des `entreprises`

La table `companies` stocke les informations relatives aux entreprises clientes.

```sql
-- Table des entreprises
CREATE TABLE companies (
    company_id INT PRIMARY KEY AUTO_INCREMENT, -- Identifiant unique pour chaque entreprise
    company_name VARCHAR(100), -- Nom de l'entreprise
    industry VARCHAR(50), -- Secteur d'activité de l'entreprise
    website VARCHAR(100), -- Site web de l'entreprise
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- Date de création de l'enregistrement
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP -- Date de mise à jour de l'enregistrement
);
```

## Table des `opportunités`

La table `opportunities` permet de suivre les opportunités de vente associées à un contact ou à une entreprise.

```sql
-- Table des opportunités de vente
CREATE TABLE opportunities (
    opportunity_id INT PRIMARY KEY AUTO_INCREMENT, -- Identifiant unique pour chaque opportunité
    opportunity_name VARCHAR(100), -- Nom de l'opportunité (par exemple, "Contrat annuel de maintenance")
    contact_id INT, -- Référence au contact associé
    company_id INT, -- Référence à l'entreprise associée
    deal_value DECIMAL(10, 2), -- Valeur financière de l'opportunité
    stage VARCHAR(50), -- Statut de l'opportunité (par exemple, "En négociation", "Fermée")
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- Date de création de l'enregistrement
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, -- Date de mise à jour de l'enregistrement
    FOREIGN KEY (contact_id) REFERENCES contacts(contact_id), -- Clé étrangère vers la table des contacts
    FOREIGN KEY (company_id) REFERENCES companies(company_id) -- Clé étrangère vers la table des entreprises
);
```

## Table des `interactions`

La table `interactions` enregistre les interactions entre les représentants de l'entreprise et les contacts, comme les appels, les emails, ou les réunions.

```sql
-- Table des interactions (emails, appels, réunions)
CREATE TABLE interactions (
    interaction_id INT PRIMARY KEY AUTO_INCREMENT, -- Identifiant unique pour chaque interaction
    contact_id INT, -- Référence au contact avec lequel l'interaction a eu lieu
    interaction_type VARCHAR(50), -- Type d'interaction (par exemple, "email", "appel", "réunion")
    interaction_date TIMESTAMP, -- Date de l'interaction
    notes TEXT, -- Notes ou résumé de l'interaction
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- Date de création de l'enregistrement
    FOREIGN KEY (contact_id) REFERENCES contacts(contact_id) -- Clé étrangère vers la table des contacts
);
```

## Améliorations possibles :
- Ajout d'index sur les colonnes fréquemment recherchées, comme les emails ou les noms d'entreprises.
- Ajout d'une table de `utilisateurs` pour suivre les représentants ou les agents commerciaux.
- Création d'une table de `tâches` pour organiser le suivi des actions à entreprendre pour chaque opportunité.

Ce schéma peut être personnalisé en fonction des besoins spécifiques de votre entreprise ou de votre application CRM.
