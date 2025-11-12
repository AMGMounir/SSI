# Correction Examen M2 MIAGE SSI

Ce document propose une correction détaillée de l'examen, en s'appuyant sur les principes et méthodes de la sécurité des systèmes d'information.

## I) Phase d'audit

*Positionnement : Membre du cabinet d'audit pour THALES.*

### 1) Positionnement de l'audit

#### a. Quels processus métier de la startup faut-il auditer en priorité ?

La startup a pour cœur de métier la **production et la maintenance d'applications de gestion de flotte mobile**. Le contrat avec la branche défense de THALES implique la manipulation de données et de contextes hautement sensibles. Par conséquent, les processus métier à auditer en priorité sont ceux qui garantissent la **confiance** que THALES peut placer dans la solution.

Il faut donc auditer en priorité :

1.  **Le processus de développement logiciel (A.14 de l'ISO 27002)** : C'est le processus "usine" de la startup. Il est critique de s'assurer que des pratiques de développement sécurisé (DevSecOps) sont en place. Cela inclut la sécurité du code, la gestion des dépendances, les tests de sécurité (statiques et dynamiques), et la protection de l'environnement d'intégration et de déploiement continus (CI/CD). Une faille à ce niveau pourrait impacter tous les clients, y compris THALES.
2.  **Le processus de gestion des données (client)** : Ce processus couvre la manière dont les données de THALES (liste des employés, types de téléphones, etc.) sont collectées, stockées, traitées, sauvegardées et détruites. La confidentialité, l'intégrité et la disponibilité de ces données sont primordiales. Il faut vérifier le chiffrement, la ségrégation des données et les contrôles d'accès.
3.  **Le processus de gestion des incidents de sécurité (A.16)** : En tant que jeune startup, sa capacité à détecter une attaque, à y réagir de manière structurée, à la contenir et à communiquer de façon transparente (notamment avec son client THALES) est un facteur clé de résilience et de confiance.

#### b. L'entreprise hésite à considérer la malveillance dans le champ d'audit. Qu'en pensez-vous ?

**C'est une erreur grave.** Refuser de considérer la malveillance dans le champ de l'audit est un non-sens, surtout dans ce contexte.

1.  **Contexte du client** : Le client est THALES, secteur de la **défense**. Ce secteur est une cible privilégiée pour des attaques étatiques ou des groupes de cybercriminels avancés. La startup, en tant que fournisseur, devient une cible indirecte, un maillon faible potentiel pour atteindre THALES.
2.  **Nature des menaces** : La méthode MEHARI, comme toute analyse de risques sérieuse, se base sur l'étude des menaces. La malveillance (interne ou externe) est une des sources de menaces les plus critiques (espionnage, sabotage, fraude). L'ignorer revient à réaliser une analyse de risques incomplète et à sous-évaluer drastiquement le niveau de risque réel.
3.  **Crédibilité de l'audit** : Un audit qui exclurait volontairement ce scénario n'aurait aucune valeur pour THALES et décrédibiliserait totalement le cabinet d'audit et la startup. Il est impératif d'évaluer la robustesse de la startup face à des attaquants déterminés.

En conclusion, il faut **impérativement inclure la malveillance** dans le périmètre de l'audit.

#### c. Ce choix vous paraît-il opportun ?

Le choix de travailler en priorité sur les **services de gestion des réseaux locaux** est **peu opportun**.

L'activité de la startup n'est pas de gérer un réseau local complexe, mais de développer une application. Le risque principal ne se situe pas sur le réseau Wi-Fi du bureau de la startup, mais bien sur la plateforme qui héberge l'application et les données de THALES. Les actifs critiques sont le code source, les serveurs de production et les données sensibles, qui sont probablement hébergés chez un fournisseur de Cloud (AWS, Azure, GCP) et non sur le LAN.

Auditer le réseau local est certes utile, mais ce n'est absolument pas la priorité. Cela dénote une mauvaise compréhension des véritables enjeux de sécurité de la startup.

#### d. Quels services de sécurité ISO 27002 préconisez-vous en complément ?

En complément, et de manière bien plus prioritaire, je préconise de se concentrer sur les domaines (services) suivants de la norme ISO 27002 :

1.  **A.14 - Acquisition, développement et maintenance des systèmes d'information** : Ce domaine est au cœur du métier de la startup. Il faut s'assurer que la sécurité est intégrée à toutes les phases du cycle de vie du développement logiciel (exigences de sécurité, sécurisation de l'environnement de développement, tests de sécurité, gestion des vulnérabilités).
2.  **A.9 - Contrôle d'accès (Access Control)** : C'est un service fondamental. Il faut définir et implémenter une politique de contrôle d'accès stricte pour savoir qui a accès à quoi : au code source, aux serveurs de production, aux bases de données, aux données de THALES. Le principe du moindre privilège doit être la règle.
3.  **A.12 - Sécurité de l'exploitation (Operations Security)** : Ce domaine couvre la protection des environnements de production. Il inclut la protection contre les maliciels, la gestion des sauvegardes, la journalisation et la surveillance (logging and monitoring), la gestion des vulnérabilités techniques et l'assurance que les procédures d'exploitation sont documentées et suivies.

### 2) Analyses et préconisations

#### a. Contrôle des droits d'administration

*   **Analyse critique** : Pour une startup, il est courant que les quelques employés (les fondateurs) aient tous les droits sur tous les systèmes pour des raisons d'agilité. Cependant, dans un contexte de contrat avec un client comme THALES, cette situation est inacceptable. Un seul compte compromis donnerait un accès total à l'attaquant. Il n'y a aucune traçabilité (qui a fait quoi ?) et le risque d'erreur humaine ou d'abus est maximal.

*   **Préconisations (basées sur MEHARI)** :
    *   **Action Organisationnelle** : Définir une **politique de contrôle d'accès** formelle basée sur le principe du **moindre privilège**. Créer des rôles distincts (ex: Développeur, Opérateur de production, Administrateur Base de Données) avec des permissions clairement délimitées. Les droits d'administration ne doivent être accordés que de manière temporaire et justifiée.
    *   **Action Procédurale** : Mettre en place un **processus de gestion des droits** : demande formelle, validation par un tiers, revue périodique des droits attribués et révocation immédiate lorsque l'accès n'est plus nécessaire. Toute action d'administration doit être tracée dans un journal sécurisé.
    *   **Action Technique** :
        *   Mettre en place un **bastion d'administration** (ou solution de Privileged Access Management - PAM). Personne ne se connecte directement aux serveurs de production.
        *   Imposer l'**authentification multifacteur (MFA)** pour toute connexion à un compte à privilèges.
        *   Supprimer les comptes partagés. Chaque administrateur doit avoir un compte nominatif.

#### b. Documentation des procédures d'exploitation

*   **Analyse critique** : L'absence de documentation est un "péché mignon" des startups. Le savoir est dans la tête des fondateurs. Cela pose plusieurs problèmes critiques : dépendance extrême à des personnes clés, incapacité à réagir correctement et rapidement à un incident (comment restaurer une sauvegarde ?), difficulté à intégrer de nouveaux collaborateurs, et impossibilité de prouver à un auditeur ou un client que les opérations sont maîtrisées.

*   **Préconisations (basées sur MEHARI)** :
    *   **Action Organisationnelle** : Rendre la documentation **obligatoire** et l'intégrer dans la "definition of done" de toute nouvelle tâche d'exploitation. Désigner un responsable pour la qualité et la mise à jour de la documentation.
    *   **Action Procédurale** : Définir des **modèles (templates)** pour les procédures critiques : déploiement d'une nouvelle version, restauration des données, gestion d'un incident de sécurité, arrivée/départ d'un collaborateur.
    *   **Action Technique** : Utiliser un outil centralisé et versionné pour héberger la documentation, comme un **wiki d'entreprise (Confluence) ou un dépôt Git** dédié. Cela garantit que tout le monde accède à la dernière version et permet de suivre les modifications.

## II) Evolutions du SI

*Positionnement : Un des deux étudiants fondateurs de la startup.*

### a) établissez le RACI du processus

Le RACI (Responsible, Accountable, Consulted, Informed) permet de clarifier les rôles et responsabilités dans un processus.

| étape du Processus | Employé (Demandeur) | Hiérarchique | Resp. Sécurité Téléphonie | Fournisseur | Département Téléphonie (THALES) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| 1. Demande de téléphone | **R** | I | I | | **A** |
| 2. Validation opportunité | I | **A/R** | C | | C |
| 3. Validation sécurité | I | I | **A/R** | | C |
| 4. Validation disponibilité | I | I | I | **A/R** | C |
| 5. Attribution du téléphone | I | I | I | **R** | **A** |

**Légende :**
*   **R (Responsible)** : Réalise l'action.
*   **A (Accountable)** : Est le responsable final, celui qui rend des comptes. Il ne doit y en avoir qu'un par ligne.
*   **C (Consulted)** : Doit être consulté avant l'action.
*   **I (Informed)** : Doit être informé après l'action.

**Justifications :**
*   Le **Département Téléphonie** est *Accountable* du processus global (étapes 1 et 5).
*   Pour les étapes de validation (2, 3, 4), le valideur est à la fois celui qui fait (**R**) et celui qui est responsable de sa décision (**A**). On peut noter A/R.
*   Le demandeur initie le processus et est ensuite informé des grandes étapes.

### b) Quel modèle de gestion de contrôle d'accès vous semble adapté ?

Le modèle le plus adapté est sans conteste le **RBAC (Role-Based Access Control)**, ou contrôle d'accès basé sur les rôles.

**Détails et justification :**

Le processus décrit est très structuré, organisationnel et basé sur des fonctions bien définies au sein de THALES. Le modèle RBAC est conçu précisément pour ce type de scénario.

Voici comment il s'appliquerait :

1.  **Définition des Rôles** : On définit des rôles dans l'application qui correspondent aux acteurs du processus :
    *   `Employe`
    *   `Manager` (correspond au "Hiérarchique")
    *   `AgentSecurite` (correspond au "Resp. Sécurité Téléphonie")
    *   `Fournisseur`
    *   `AdminTelephonie` (pour le "Département Téléphonie")

2.  **Définition des Permissions** : On définit les actions possibles dans l'application comme des permissions :
    *   `creer_demande`
    *   `valider_opportunite`
    *   `rejeter_opportunite`
    *   `valider_securite`
    *   `rejeter_securite`
    *   `confirmer_disponibilite`
    *   `attribuer_telephone`
    *   `voir_toutes_les_demandes`

3.  **Assignation des Permissions aux Rôles** : C'est le cœur du modèle RBAC.
    *   Le rôle `Employe` a la permission `creer_demande`.
    *   Le rôle `Manager` a les permissions `valider_opportunite` et `rejeter_opportunite`.
    *   Le rôle `AgentSecurite` a les permissions `valider_securite` et `rejeter_securite`.
    *   Le rôle `Fournisseur` a les permissions `confirmer_disponibilite` et `attribuer_telephone`.
    *   Le rôle `AdminTelephonie` a la permission `voir_toutes_les_demandes`.

4.  **Assignation des Utilisateurs aux Rôles** : Les utilisateurs de THALES se voient assigner un ou plusieurs rôles en fonction de leur poste.

Ce modèle est bien plus simple à gérer qu'un modèle où on assignerait les permissions directement à chaque utilisateur (modèle DAC). Il est également plus sécurisé car il suit la logique métier et organisationnelle de THALES.

### c) Quel système d'authentification préconisez-vous ?

Le contexte (client B2B de haute sécurité) impose un système d'authentification moderne, sécurisé et intégré. Je préconise la mise en place d'une **fédération d'identités** basée sur un standard comme **SAML 2.0** ou **OpenID Connect (OIDC)**.

**Justification détaillée :**

1.  **Sécurité et SSO (Single Sign-On)** : Les employés de THALES ne doivent PAS avoir un nouveau couple login/mot de passe à gérer pour notre application. Cela multiplierait les risques (mot de passe faible, réutilisé, etc.). La solution est que notre application (le *Service Provider* - SP) fasse confiance au système d'identité de THALES (l'*Identity Provider* - IdP), qui est très probablement une solution robuste comme Microsoft ADFS (Active Directory Federation Services). L'employé se connecte via le portail de THALES, et est ensuite redirigé vers notre application de manière transparente et sécurisée (SSO).

2.  **Maîtrise de la politique de sécurité par le client** : En utilisant la fédération, c'est THALES qui maîtrise la politique d'authentification. S'ils veulent imposer une **authentification forte** (MFA, carte à puce, biométrie), ils peuvent le faire au niveau de leur IdP sans que nous ayons à l'implémenter. Notre application reçoit simplement une "assertion" de la part de l'IdP de THALES lui disant "cet utilisateur est bien M. Dupont, il est authentifié et voici ses attributs (rôle, département, etc.)".

3.  **Gestion du cycle de vie des utilisateurs** : Quand un employé quitte THALES, son compte est désactivé dans leur annuaire. L'accès à notre application est alors **automatiquement et immédiatement coupé**, sans aucune action de notre part. C'est un avantage de sécurité majeur.

4.  **Gestion des différents acteurs** : Le modèle de fédération permet de gérer les différents cas :
    *   **Employés THALES** : via la fédération SAML/OIDC avec l'IdP de THALES.
    *   **Fournisseur** : Le fournisseur étant externe, il peut être géré soit via une fédération B2B si son SI le permet, soit via un compte local géré dans notre application, mais avec une politique de sécurité stricte (MFA obligatoire, renouvellement de mot de passe, etc.).

En conclusion, la fédération d'identité est la seule approche professionnelle et sécurisée qui réponde aux exigences d'un tel client.
