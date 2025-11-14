# Fiche de Révision 2: Audit SSI - Contexte Cloud (IaaS/PaaS/SaaS)

**Contexte : Audit d'une solution hébergée sur un fournisseur Cloud public (ex: AWS, Azure, GCP), couvrant les modèles IaaS, PaaS et SaaS.**

---

### 1. Périmètre de l'Audit : Le Modèle de Responsabilité Partagée

Le point le plus crucial de l'audit Cloud est la compréhension du **modèle de responsabilité partagée**. L'audit ne porte pas sur la sécurité du Cloud lui-même, mais sur la **sécurité DANS le Cloud**.

#### A. Ce qu'il ne faut PAS auditer (Responsabilité du Fournisseur Cloud)
- **Infrastructure physique** : La sécurité des datacenters (accès physique, alimentation, climatisation).
- **Couche de virtualisation (IaaS)** : La sécurité de l'hyperviseur.
- **Services managés de bas niveau (PaaS)** : La maintenance et la sécurité de l'OS et du middleware gérés par le fournisseur (ex: base de données RDS, Azure SQL).
- **Infrastructure sous-jacente du SaaS** : L'ensemble de la pile technique pour une solution SaaS.

**Action de l'auditeur** : Ne pas auditer ces points, mais **exiger les certifications et attestations de conformité** du fournisseur Cloud (ISO 27001, SOC 2 Type II, HDS, etc.) et vérifier qu'elles couvrent les services utilisés.

#### B. Processus à Auditer en Priorité (Responsabilité du Client)
L'audit doit se concentrer sur la **configuration** et l'**utilisation** des services Cloud.
1.  **Gouvernance et Gestion des Risques Cloud** :
    - **Stratégie multi-cloud/hybride** : Complexité, flux de données entre les environnements.
    - **Gestion des coûts (FinOps)** : Une dérive des coûts peut indiquer une mauvaise configuration, une compromission (cryptomining) ou une utilisation non maîtrisée.
    - **Conformité réglementaire** : Localisation des données, respect du RGPD, etc.
2.  **Gestion des Identités et des Accès (IAM - Identity and Access Management)** :
    - **Utilisation du compte root/super-admin** : Doit être bannie pour l'usage quotidien et ultra-sécurisée (MFA matériel).
    - **Gestion des rôles et des politiques d'accès (Policies)** : Granularité, utilisation de conditions, moindre privilège.
    - **Fédération d'identité** avec l'annuaire d'entreprise (ex: Azure AD, Okta).
3.  **Sécurité des Données** :
    - **Configuration des services de stockage** (ex: buckets S3, Azure Blob Storage) : prévention des accès publics, chiffrement par défaut, versioning, cycle de vie.
    - **Gestion des secrets** (clés d'API, certificats, mots de passe) : Utilisation de services dédiés (AWS Secrets Manager, Azure Key Vault). **Aucun secret en clair dans le code ou les variables d'environnement.**
4.  **Sécurité Réseau** :
    - **Configuration des réseaux virtuels** (VPC, VNet) : Segmentation, listes de contrôle d'accès (ACL), groupes de sécurité (Security Groups).
    - **Exposition sur Internet** : Points d'accès, équilibreurs de charge, passerelles.
5.  **Sécurité des "Compute Services"** :
    - **IaaS (VMs)** : Processus de "hardening" des OS, gestion des patchs.
    - **PaaS (Conteneurs, Serverless)** : Sécurité des images de conteneurs (scan de vulnérabilités), sécurité du code des fonctions (Faas).
6.  **Infrastructure as Code (IaC)** :
    - Processus de déploiement (CI/CD), validation et scan de sécurité des templates (Terraform, CloudFormation).

---

### 2. Stratégie de Traitement des Risques (TARE) dans le Cloud

| Stratégie | Description | Mots-clés / Scénarios Cloud |
| :--- | :--- | :--- |
| **Traiter (Réduire)** | Configurer correctement les services Cloud pour réduire la surface d'attaque. | **Mauvaise configuration, Accès illégitime, Vulnérabilité applicative** : Configurer IAM avec le moindre privilège, utiliser les groupes de sécurité, chiffrer les données, scanner les conteneurs. |
| **Accepter** | Accepter un risque faible après analyse formelle. | **Risque résiduel faible** : ex: une fonction de bas privilège non critique exposée via une API Gateway avec authentification. |
| **Refuser (Éviter)** | Ne pas utiliser un service ou une configuration jugée trop risquée. | **Service non maîtrisé, Non-conformité** : Interdire l'utilisation de certaines régions Cloud, bloquer la création de ressources publiques via des "Service Control Policies" (SCP). |
| **Transférer** | Le choix du Cloud est en soi un transfert de risque (sécurité physique, maintenance de l'hyperviseur). | **Panne matérielle, Catastrophe naturelle** : Le fournisseur Cloud est responsable. Le client reste responsable de la configuration de la redondance (multi-AZ, multi-région). On peut aussi souscrire une cyber-assurance. |

---

### 3. Mesures de Sécurité Cloud (Regroupées par Thème)

| Thème | Mesures à Mettre en Œuvre |
| :--- | :--- |
| **IAM (Identités et Accès)** | - **Activer le MFA** pour tous les utilisateurs, en particulier les comptes à privilèges.<br>- **Utiliser des rôles IAM** pour les applications et services, jamais de clés d'accès long terme.<br>- **Politiques de moindre privilège** granulaires et conditionnelles (par IP, par heure...).<br>- **Surveiller l'activité IAM** (ex: AWS CloudTrail) pour détecter la création de comptes, l'escalade de privilèges. |
| **Protection des Données** | - **Chiffrement systématique au repos et en transit** (activé par défaut sur la plupart des services modernes).<br>- **Bloquer tout accès public** par défaut sur les services de stockage (S3 Block Public Access).<br>- **Utiliser des services de gestion de clés** (KMS, Key Vault) et contrôler l'accès à ces clés.<br>- **Mettre en place des alertes** sur les anomalies d'accès aux données. |
| **Sécurité Réseau** | - **Principe du "zero trust"** : tout est filtré par défaut.<br>- **Utiliser des groupes de sécurité (stateful)** comme firewall principal pour les VMs.<br>- **Limiter au maximum les ports d'administration (SSH, RDP)** ouverts sur Internet. Utiliser des bastions ou des services managés (SSM Session Manager).<br>- **Déployer un WAF** devant les applications web exposées. |
| **Journalisation et Surveillance** | - **Activer et centraliser les logs** de tous les services (CloudTrail, VPC Flow Logs, etc.).<br>- **Utiliser des services de détection de menaces** (AWS GuardDuty, Azure Sentinel).<br>- **Créer des alertes automatisées** sur les événements critiques (utilisation du compte root, suppression d'un bucket, etc.). |
| **Infrastructure as Code (IaC) & DevSecOps** | - **Scanner le code IaC** pour les mauvaises configurations avant le déploiement (ex: `tfsec`, `checkov`).<br>- **Intégrer les scans de sécurité** dans la pipeline CI/CD (scan de conteneurs, analyse de dépendances).<br>- **Interdire les modifications manuelles** en production ("GitOps"). |

---

### 4. Contrôle d'Accès, Moindre Privilège et RACI

#### A. Contrôle d'Accès et Moindre Privilège dans le Cloud
Le contrôle d'accès dans le cloud est presque exclusivement basé sur le **RBAC**, mis en œuvre via les services **IAM**.

- **Utilisateurs vs Rôles** : Les utilisateurs sont des identités permanentes (personnes). Les rôles sont des identités temporaires assumées par des utilisateurs ou des services (VMs, fonctions) pour exécuter des actions. **Toujours préférer les rôles.**
- **Moindre Privilège** : Le principe est appliqué en créant des politiques (policies) JSON qui définissent explicitement les actions autorisées (`Allow`) ou refusées (`Deny`) sur des ressources spécifiques.
    - **Exemple** : Une politique pour une application de traitement d'images pourrait autoriser `s3:GetObject` et `s3:PutObject` uniquement dans un bucket spécifique (`arn:aws:s3:::mon-bucket-images/*`), mais refuser `s3:DeleteObject`.

#### B. Matrice RACI
**Exemple de RACI pour le "Déploiement d'une nouvelle application via une pipeline CI/CD et IaC"**

| Tâche / Acteur | Équipe de Développement (Dev) | Équipe Sécurité (Sec) | Équipe Opérationnelle (Ops) | Product Owner (PO) |
| :--- | :---: | :---: | :---: | :---: |
| **Développement du code applicatif et IaC** | R | C | C | I |
| **Scan de sécurité statique (SAST, IaC scan)** | R | A | I | - |
| **Validation de la conformité de la configuration** | C | A | R | - |
| **Déclenchement du déploiement en production** | I | C | R | A |
| **Surveillance post-déploiement** | C | C | R | A |
| **Gestion de l'incident en cas d'échec** | R | C | A | I |

- **R (Responsible)** : Le développeur code et corrige les vulnérabilités trouvées. L'Ops surveille.
- **A (Accountable)** : La Sécurité est garante de la conformité. Le PO est responsable du "Go/No-Go". L'Ops est responsable de la stabilité.
- **C (Consulted)** : Les équipes collaborent pour définir les règles et résoudre les problèmes.
- **I (Informed)** : Le PO est informé de l'avancement technique.
