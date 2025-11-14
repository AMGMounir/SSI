# Fiche de Révision 1: Audit SSI - Contexte B2B

**Contexte : Fourniture d'une solution (Applicative, Hardware ou Cloud) pour une autre entreprise (le client) impliquant la gestion de ses données sensibles.**

---

### 1. Périmètre de l'Audit : Ce qu'il faut auditer et ce qu'il ne faut pas auditer

#### A. Ce qu'il ne faut PAS auditer (Hors Périmètre)
- **Processus internes du client non liés au service** : Les processus RH, financiers, ou commerciaux du client qui n'interagissent pas avec la solution fournie.
- **Infrastructure physique du client** : Sauf si le service implique un hardware spécifique installé chez lui, auquel cas l'audit se limite à ce hardware.
- **Audits déjà réalisés par des tiers de confiance** : Ne pas ré-auditer les composants certifiés (ex: datacenters certifiés ISO 27001/SOC 2), mais vérifier que le niveau de certification correspond au besoin et que le rapport est valide. Demander l'attestation de conformité.
- **Détails financiers ou stratégiques confidentiels** du fournisseur sans lien direct avec la continuité ou la sécurité du service (ex: marges, R&D sur d'autres produits).

#### B. Processus Métier à Auditer en Priorité (Dans le Périmètre)
Le focus est mis sur le cycle de vie de la donnée sensible du client et la résilience du service.
1.  **Gouvernance et Gestion des Risques** :
    - Le **SMSI** (Système de Management de la Sécurité de l'Information) du fournisseur.
    - L'analyse de risques spécifique au service fourni au client.
    - La clarté des engagements contractuels (SLA, clauses de sécurité, réversibilité).
2.  **Gestion du Cycle de Vie de la Donnée Sensible** :
    - **Collecte/Création** : Canaux de transmission, validation et classification à la source.
    - **Stockage et Traitement** : Mesures de chiffrement (en transit, au repos), anonymisation/pseudonymisation, isolation des données (multi-tenant).
    - **Partage/Transfert** : Contrôles sur les API, les exports, et les flux de données vers des tiers.
    - **Archivage et Suppression** : Politiques de rétention, processus de suppression sécurisée et preuve de suppression.
3.  **Gestion des Identités et des Accès (GIA)** :
    - Processus d'enrôlement et de départ des utilisateurs (du client et du fournisseur).
    - Gestion des privilèges et revue périodique des accès.
4.  **Gestion des Incidents et Continuité d'Activité** :
    - Processus de détection, de réponse et de notification en cas d'incident de sécurité.
    - Le **PRA/PCA** (Plan de Reprise/Continuité d'Activité) : vérifier qu'il est testé et qu'il respecte les RTO/RPO contractuels.

---

### 2. Stratégie de Traitement des Risques (TARE)

Chaque risque identifié doit être traité selon l'une des stratégies suivantes :

| Stratégie | Description | Mots-clés / Scénarios |
| :--- | :--- | :--- |
| **Traiter (Réduire)** | Mettre en place des mesures de sécurité pour réduire la probabilité ou l'impact du risque. | **Malveillance, Erreur (humaine, utilisateur), Fraude** : Chiffrement, Contrôle d'accès, MFA, IDS/IPS, Sensibilisation, Procédures de validation. |
| **Accepter** | Accepter le risque car son traitement coûterait plus cher que l'impact potentiel. Le risque doit être faible et formellement accepté par la direction. | **Risque résiduel faible, Impact financier minime** : Risque documenté et suivi. Uniquement pour les menaces à très faible probabilité et impact. |
| **Refuser (Éviter)** | Arrêter l'activité à l'origine du risque. | **Risque majeur, Impact critique (financier, réputationnel), Non-conformité légale** : Changement de processus métier, abandon d'une fonctionnalité, refus d'un certain type de données. |
| **Transférer** | Déplacer l'impact financier du risque vers un tiers. | **Sinistre, Catastrophe naturelle, Panne majeure** : Souscription à une cyber-assurance, externalisation d'un service à un partenaire spécialisé (ex: hébergement dans un cloud public, qui transfère le risque de sécurité physique). |

---

### 3. Mesures de Sécurité (Regroupées par Thème)

| Thème | Mesures à Mettre en Œuvre |
| :--- | :--- |
| **Gestion des Identités et des Accès** | - **Politique de mot de passe robuste** (longueur, complexité, renouvellement).<br>- **MFA (Multi-Factor Authentication)** obligatoire pour tous les accès, surtout pour les comptes à privilèges.<br>- **Principe du Moindre Privilège** (voir section 4).<br>- **Revue trimestrielle/semestrielle des comptes et des droits**.<br>- **Processus formel de provisioning/déprovisioning des utilisateurs**. |
| **Sécurité des Données** | - **Chiffrement systématique** : en transit (TLS 1.2+), au repos (AES-256), et potentiellement en usage (Confidential Computing).<br>- **Classification de la donnée** dès sa création pour appliquer les bons contrôles.<br>- **Masquage / Pseudonymisation** pour les environnements de test/développement.<br>- **Prévention de la fuite de données (DLP)** pour surveiller les exports et les partages. |
| **Sécurité de l'Infrastructure** | - **Segmentation réseau** (DMZ, réseaux de production, d'administration).<br>- **Protection périmétrique** (Firewall, WAF).<br>- **Protection des postes de travail et serveurs** (EDR/XDR, anti-malware, HIDS).<br>- **Gestion des vulnérabilités** : scans réguliers, politique de patching rigoureuse. |
| **Journalisation et Surveillance** | - **Collecte centralisée des logs** (système, applicatif, réseau).<br>- **Corrélation d'événements et détection d'anomalies** (SIEM).<br>- **Horodatage fiable** (synchronisation NTP).<br>- **Politique de rétention des logs** conforme aux exigences légales et contractuelles. |
| **Continuité d'Activité** | - **Sauvegardes régulières, chiffrées et testées** (tests de restauration).<br>- ** PRA/PCA documenté et testé** au moins annuellement.<br>- **Infrastructure redondante** (clusters, load balancing). |

---

### 4. Contrôle d'Accès, Moindre Privilège et RACI

#### A. Contrôle d'Accès et Principe du Moindre Privilège
- **Contrôle d'Accès** : Ensemble des mesures permettant de s'assurer qu'un utilisateur (ou un service) ne peut accéder qu'aux ressources (données, fonctionnalités) pour lesquelles il a explicitement reçu l'autorisation. On distingue :
    - **DAC (Discretionary)** : Le propriétaire de la ressource donne les droits. (Peu adapté aux entreprises).
    - **MAC (Mandatory)** : Le système impose l'accès basé sur des niveaux de sensibilité (ex: Confidentiel, Secret).
    - **RBAC (Role-Based)** : Le modèle le plus courant. Les droits sont assignés à des rôles, et les utilisateurs se voient attribuer des rôles. C'est la base du **moindre privilège**.

- **Principe du Moindre Privilège** : Un utilisateur ne doit disposer que des droits strictement nécessaires à la réalisation de ses tâches, et ce, pour la durée la plus courte possible.
    - **Application** : Pas d'administrateur par défaut. Créer des rôles granulaires (ex: "Lecteur de logs", "Validateur de données", "Administrateur de base de données"). Les droits d'écriture/suppression sont à limiter au maximum. La revue périodique garantit que les droits ne s'accumulent pas inutilement.

#### B. Matrice RACI (Responsible, Accountable, Consulted, Informed)
Le RACI clarifie "qui fait quoi" dans un processus. C'est un outil d'audit essentiel pour vérifier la séparation des tâches.

**Exemple de RACI pour la "Gestion d'un Incident de Sécurité Majeur"**

| Tâche / Acteur | Responsable DSI (Client) | RSSI (Fournisseur) | Support N1 (Fournisseur) | Équipe Technique (Fournisseur) | Communication (Fournisseur) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Détection et qualification de l'incident** | I | A | R | C | - |
| **Isolation du système affecté** | C | A | I | R | - |
| **Analyse et correction du problème** | C | A | - | R | - |
| **Communication sur la résolution (au client)** | I | A | - | C | R |
| **Validation du retour à la normale** | R | A | - | C | - |
| **Rapport post-incident** | C | R | - | C | A |

- **R (Responsible / Réalisateur)** : La ou les personnes qui effectuent la tâche.
- **A (Accountable / Approbateur)** : La seule personne qui est redevable de la bonne exécution de la tâche. C'est le décisionnaire final.
- **C (Consulted / Consulté)** : Personnes dont l'avis est requis (communication à double sens).
- **I (Informed / Informé)** : Personnes tenues au courant de l'avancement (communication à sens unique).
