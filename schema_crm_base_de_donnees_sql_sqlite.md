
# Schéma CRM - Base de données SQL (SQLite 3.37.2)

Ce fichier SQL crée un système CRM compatible avec SQLite3 avec les tables **contacts**, **entreprises**, **opportunités** et **interactions**. Chaque table est commentée en français pour faciliter la compréhension.

## Création de la BD et des tables

```sql
-- Table des entreprises (companies)
-- Cette table stocke les informations relatives aux entreprises clientes.
CREATE TABLE companies (
    company_id INTEGER PRIMARY KEY AUTOINCREMENT,  -- Identifiant unique de l'entreprise
    company_name TEXT NOT NULL,                   -- Nom de l'entreprise
    industry TEXT,                                -- Secteur d'activité de l'entreprise
    website TEXT,                                 -- Site web de l'entreprise
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,    -- Date de création de l'enregistrement
    updated_at TEXT DEFAULT CURRENT_TIMESTAMP     -- Date de mise à jour
);

-- Table des contacts (contacts)
-- Cette table stocke les informations des contacts individuels associés aux entreprises.
CREATE TABLE contacts (
    contact_id INTEGER PRIMARY KEY AUTOINCREMENT, -- Identifiant unique du contact
    first_name TEXT NOT NULL,                     -- Prénom du contact
    last_name TEXT NOT NULL,                      -- Nom de famille du contact
    email TEXT UNIQUE NOT NULL,                   -- Adresse email du contact (doit être unique)
    phone TEXT,                                   -- Numéro de téléphone du contact
    company_id INTEGER,                           -- Référence à l'entreprise associée
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,    -- Date de création de l'enregistrement
    updated_at TEXT DEFAULT CURRENT_TIMESTAMP,    -- Date de mise à jour
    FOREIGN KEY (company_id) REFERENCES companies(company_id) -- Clé étrangère vers la table entreprises
);

-- Table des opportunités (opportunities)
-- Cette table suit les opportunités de vente associées aux contacts ou aux entreprises.
CREATE TABLE opportunities (
    opportunity_id INTEGER PRIMARY KEY AUTOINCREMENT, -- Identifiant unique de l'opportunité
    opportunity_name TEXT NOT NULL,                  -- Nom de l'opportunité
    contact_id INTEGER,                              -- Référence au contact associé
    company_id INTEGER,                              -- Référence à l'entreprise associée
    deal_value REAL,                                 -- Valeur estimée du contrat/de la vente
    stage TEXT NOT NULL,                             -- État de l'opportunité (ex. : "négociation", "proposition")
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,       -- Date de création de l'opportunité
    updated_at TEXT DEFAULT CURRENT_TIMESTAMP,       -- Date de mise à jour
    FOREIGN KEY (contact_id) REFERENCES contacts(contact_id),  -- Clé étrangère vers la table contacts
    FOREIGN KEY (company_id) REFERENCES companies(company_id)  -- Clé étrangère vers la table entreprises
);

-- Table des interactions (interactions)
-- Cette table enregistre les interactions entre l'entreprise et les contacts (emails, appels, réunions).
CREATE TABLE interactions (
    interaction_id INTEGER PRIMARY KEY AUTOINCREMENT, -- Identifiant unique de l'interaction
    contact_id INTEGER,                               -- Référence au contact associé
    interaction_type TEXT NOT NULL,                   -- Type d'interaction (ex. : "email", "appel", "réunion")
    interaction_date TEXT NOT NULL,                   -- Date de l'interaction
    notes TEXT,                                       -- Notes sur l'interaction
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,        -- Date de création de l'interaction
    FOREIGN KEY (contact_id) REFERENCES contacts(contact_id)  -- Clé étrangère vers la table contacts
);
```

## Insertion des données

```sql
-- Insertion de données dans la table entreprises
INSERT INTO companies (company_name, industry, website) VALUES
('TechCorp', 'Technologie', 'www.techcorp.com'),
('MediSolutions', 'Santé', 'www.medisolutions.com'),
('FinServe', 'Finance', 'www.finserve.com'),
('GreenEnergy', 'Énergie', 'www.greenenergy.com'),
('EduLearn', 'Éducation', 'www.edulearn.com');

-- Insertion de données dans la table contacts (20 contacts au total)
INSERT INTO contacts (first_name, last_name, email, phone, company_id) VALUES
('John', 'Doe', 'john.doe@techcorp.com', '123-456-7890', 1),
('Jane', 'Smith', 'jane.smith@medisolutions.com', '234-567-8901', 2),
('Michael', 'Brown', 'michael.brown@finserve.com', '345-678-9012', 3),
('Emily', 'Davis', 'emily.davis@greenenergy.com', '456-789-0123', 4),
('Daniel', 'Wilson', 'daniel.wilson@edulearn.com', '567-890-1234', 5),
-- Ajout de 15 autres contacts fictifs
('Alice', 'Taylor', 'alice.taylor@techcorp.com', '111-222-3333', 1),
('Robert', 'Miller', 'robert.miller@medisolutions.com', '222-333-4444', 2),
('James', 'Clark', 'james.clark@finserve.com', '333-444-5555', 3),
('Sophia', 'Martinez', 'sophia.martinez@greenenergy.com', '444-555-6666', 4),
('Ethan', 'Anderson', 'ethan.anderson@edulearn.com', '555-666-7777', 5),
('Olivia', 'Garcia', 'olivia.garcia@techcorp.com', '666-777-8888', 1),
('Benjamin', 'Thomas', 'benjamin.thomas@medisolutions.com', '777-888-9999', 2),
('Lucas', 'Jackson', 'lucas.jackson@finserve.com', '888-999-0000', 3),
('Ava', 'White', 'ava.white@greenenergy.com', '999-000-1111', 4),
('William', 'Harris', 'william.harris@edulearn.com', '000-111-2222', 5),
('Sophia', 'Johnson', 'sophia.johnson@techcorp.com', '123-123-1234', 1),
('Henry', 'Lewis', 'henry.lewis@medisolutions.com', '234-234-2345', 2),
('Liam', 'Walker', 'liam.walker@finserve.com', '345-345-3456', 3),
('Charlotte', 'Young', 'charlotte.young@greenenergy.com', '456-456-4567', 4),
('Amelia', 'King', 'amelia.king@edulearn.com', '567-567-5678', 5);

-- Insertion de données dans la table opportunités
INSERT INTO opportunities (opportunity_name, contact_id, company_id, deal_value, stage) VALUES
('Nouvelle plateforme web', 1, 1, 50000.00, 'Proposition'),
('Service de télémédecine', 2, 2, 75000.00, 'Négociation'),
('Consultation financière', 3, 3, 100000.00, 'Clôturé'),
('Solution d'énergie renouvelable', 4, 4, 200000.00, 'En discussion'),
('Programme de formation', 5, 5, 15000.00, 'En discussion');

-- Insertion de données dans la table interactions
INSERT INTO interactions (contact_id, interaction_type, interaction_date, notes) VALUES
(1, 'email', '2023-09-20 10:00:00', 'Discussion initiale sur la plateforme web'),
(2, 'appel', '2023-09-21 11:30:00', 'Négociation des termes du service de télémédecine'),
(3, 'réunion', '2023-09-22 14:00:00', 'Consultation sur les finances de l'entreprise'),
(4, 'email', '2023-09-23 09:15:00', 'Présentation de la solution d'énergie'),
(5, 'appel', '2023-09-24 16:45:00', 'Discussion préliminaire sur le programme de formation');
```

## Instructions pour exécuter ce code

1. Copiez le code SQL ci-dessus.
2. Collez-le dans l'éditeur SQLite de **MyCompiler** ou tout autre environnement SQLite compatible.
3. Exécutez-le pour créer les tables, insérer les données, et voir les résultats.

Ce code crée une base de données CRM basique avec 20 contacts, 5 entreprises, des opportunités de vente, et des interactions enregistrées.
