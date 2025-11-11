# Corrigé de l'Examen de Sécurité des Systèmes d’Information

---

## I) Externalisation de l'hébergement

### Préparation du cahier des charges (CDC)

**1-a. Pour la sécurité de l'exploitation, le rédacteur propose de demander au candidat de décrire le processus en place pour d'autres clients. Ce positionnement vous semble-t-il approprié ?**

**Analyse :**
Ce positionnement est **partiellement approprié mais insuffisant**.

*   **Approprié :** Demander la description des processus existants permet d'évaluer la maturité de l'hébergeur en matière de sécurité et de vérifier qu'il ne s'agit pas de promesses sans fondement. Cela donne un aperçu de ses pratiques réelles et de son expérience. Le cours "Processus de la Sécurité des Systèmes d'information" (M2 SSI 01, slide 19) souligne l'importance de la "Sécurité de l'information dans les relations avec les fournisseurs", qui inclut la vérification des politiques et des accords.

*   **Insuffisant :** Chaque client a des besoins spécifiques. Un processus, même robuste, pour un autre client peut ne pas être adapté au contexte de "LaBoite" (développement logiciel, manipulation de données RH/paie sensibles). Il est impératif de ne pas se contenter d'une description générique. Il faut exiger de l'hébergeur qu'il décrive comment ses processus seront **adaptés** pour répondre aux exigences spécifiques de "LaBoite", notamment en matière de protection des données de paie et de sécurisation des environnements de développement. Il faut demander des engagements contractuels (SLA - Service Level Agreement) clairs et précis, et non une simple description.

**Conclusion :** C'est une bonne base de départ pour juger de l'existant, mais il est crucial d'exiger une personnalisation et des garanties contractuelles adaptées au contexte de "LaBoite".

**1-b. Une gestion du plan de continuité de l'activité devra être décrite dans les réponses. Ce positionnement vous semble-t-il approprié ?**

**Analyse :**
Ce positionnement est **totalement approprié et même indispensable**.

*   **Justification :** L'entreprise "LaBoite" externalise des serveurs critiques pour son activité : développement (code source, recette) et hébergement du service de paie pour ses clients PME. Une interruption de ces services aurait des conséquences graves :
    *   Arrêt du développement, perte de productivité.
    *   Indisponibilité du service de paie, insatisfaction des clients PME, potentiels litiges et impacts financiers et d'image désastreux.
*   **Exigences :** Le Plan de Continuité d'Activité (PCA) ou Plan de Reprise d'Activité (PRA) de l'hébergeur doit être détaillé. Comme vu dans le cours sur les SMSI (M2 SSI 02, slides 58-60), il doit couvrir les scénarios de sinistre (panne matérielle, cyberattaque, catastrophe naturelle...) et définir clairement :
    *   Les **RTO (Recovery Time Objective)** : délai maximal de reprise du service.
    *   Les **RPO (Recovery Point Objective)** : perte de données maximale acceptable.
    *   Les solutions techniques (sauvegardes, redondance, site de secours...).
    *   Les procédures de déclenchement et les responsabilités.

**Conclusion :** Exiger la description du PCA/PRA est une exigence fondamentale de sécurité pour garantir la résilience du service externalisé.

**2- Citez deux exigences qui vous semblent devoir être ajoutées au CDC.**

1.  **Exigence de Conformité Réglementaire et de Localisation des Données :**
    *   **Description :** Le cahier des charges doit exiger que l'hébergeur garantisse la conformité avec les réglementations en vigueur, notamment le **RGPD (Règlement Général sur la Protection des Données)**, puisque les données de paie sont des données personnelles et sensibles. Il doit préciser la **localisation géographique** des datacenters où seront stockées et traitées les données, et s'engager contractuellement à ne pas les transférer hors de l'Union Européenne sans garanties appropriées. Ceci est crucial pour maîtriser le cadre légal applicable. (Référence : M2 SSI 01, slide 21 "Conformité").

2.  **Exigence de Sécurisation de la Chaîne d'Approvisionnement (Supply Chain) et de Gestion des Accès :**
    *   **Description :** Le CDC doit demander à l'hébergeur de décrire comment il sécurise sa propre chaîne d'approvisionnement (matériel, logiciel, sous-traitants). De plus, il doit détailler la **gestion des identités et des accès (GIA)** pour ses propres administrateurs. Qui a accès aux serveurs de "LaBoite" ? Comment ces accès sont-ils tracés, contrôlés et revus ? Une politique de **moindre privilège** doit être exigée pour les équipes de l'hébergeur. Ceci est fondamental pour prévenir les accès illégitimes et les fuites de données, notamment le code source et les données de paie. (Référence : M2 SSI 01, slide 19 "Relations avec les fournisseurs" et M2 SSI 03 GIA, slide 6 "Couverture fonctionnelle de la GIA").

---
### Analyse des réponses

**1-a. En se basant sur une approche MEHARI, il indique que les erreurs de conception ou les incidents logiques doivent être analysés afin qu'ils puissent garantir son niveau de service d'hébergement. Ce positionnement vous semble-t-il approprié ?**

**Analyse :**
Ce positionnement est **totalement inapproprié et dénote une confusion des périmètres de responsabilité**.

*   **Le rôle de l'hébergeur :** Un hébergeur est responsable de la sécurité de l'infrastructure qu'il fournit (sécurité physique, sécurité réseau, disponibilité des serveurs, etc.). Il garantit un "contenant".
*   **Le rôle de "LaBoite" :** "LaBoite" est responsable de la sécurité de son application (le "contenu"), ce qui inclut la correction des erreurs de conception et des bugs fonctionnels ou logiques dans son logiciel de paie.
*   **Confusion des responsabilités :** En proposant d'analyser les erreurs de conception du logiciel de "LaBoite", l'hébergeur outrepasse son rôle. C'est une ingérence dans le processus de développement qui relève de la seule responsabilité de "LaBoite". La méthode MEHARI (vue dans M2 SSI 02, slide 8) est une méthode d'analyse de risques qui s'applique au périmètre que l'on souhaite auditer. L'hébergeur doit l'appliquer à **son propre périmètre de service**, pas à celui de son client.

**Conclusion :** Ce positionnement n'est pas approprié. Il traduit soit une incompréhension des responsabilités respectives, soit une tentative de vendre des prestations de conseil additionnelles qui ne relèvent pas de l'hébergement. "LaBoite" doit refuser cette proposition et recentrer le débat sur les garanties de sécurité de l'infrastructure d'hébergement.

**1-b. Pour chacun de ces services, quelle stratégie de gestion de risques préconisez-vous ? Vous proposerez un plan de réduction du risque pour le service de votre choix.**

Les quatre stratégies de traitement du risque (vues dans M2 SSI 02, slide 40) sont : l'acceptation, la réduction, le transfert et l'évitement.

*   **i- Les services relatifs à la sécurité des procédures d'exploitation des systèmes de production.**
    *   **Analyse du risque :** Un niveau insuffisant sur ce point signifie que les opérations courantes (mises à jour, sauvegardes, gestion des configurations) sont mal maîtrisées, ce qui peut entraîner des pannes, des failles de sécurité ou des pertes de données. Le service de paie étant critique, le risque est très élevé.
    *   **Stratégie préconisée :** **Réduction du risque**. Il est inacceptable de tolérer une faiblesse à ce niveau. L'acceptation est exclue car l'impact est trop fort. Le transfert est difficile (à qui transférer la responsabilité de l'exploitation de l'hébergeur ?). L'évitement signifierait changer d'hébergeur, ce qui est une option si la réduction est impossible.
    *   **Plan de réduction (si ce service est choisi) :**
        1.  **Exiger la formalisation :** Demander à l'hébergeur de fournir des procédures d'exploitation écrites, détaillées et validées.
        2.  **Exiger des preuves de contrôle :** Imposer la mise en place de journaux d'événements (logs) pour toutes les actions d'administration et de rapports de suivi réguliers.
        3.  **Audit et validation :** Exiger un audit des procédures par une tierce partie ou par "LaBoite" avant la mise en production.
        4.  **Contractualisation :** Inscrire ces exigences (formalisation, logging, audits) dans le contrat avec des pénalités en cas de non-respect.

*   **j- Les services relatifs au contrôle d'accès au bac à sable.**
    *   **Analyse du risque :** Le bac à sable (sandbox) héberge des prototypes. Bien que moins critique que la production, un contrôle d'accès faible peut permettre à un attaquant de voler des concepts innovants, du code source partiel ou de se servir de ce serveur comme d'un point de rebond pour attaquer d'autres systèmes.
    *   **Stratégie préconisée :** **Réduction du risque**. C'est la stratégie la plus logique. Cependant, selon la sensibilité réelle de ce qui se trouve dans le bac à sable, une **acceptation** temporaire du risque (si jugé faible) pourrait être envisagée le temps de mettre en place la réduction, à condition que le bac à sable soit bien isolé du reste de l'infrastructure.
    *   **Plan de réduction du risque (choix de ce service) :**
        1.  **Clarification des besoins d'accès :** Définir précisément qui (quelles équipes, quels développeurs) a besoin d'accéder au bac à sable.
        2.  **Mise en place d'une authentification forte :** Imposer l'utilisation d'une authentification multi-facteurs (MFA) ou a minima de mots de passe complexes avec renouvellement régulier pour tous les accès.
        3.  **Cloisonnement réseau :** S'assurer que le serveur de bac à sable est sur un segment réseau isolé des serveurs de développement, de recette et surtout de production. Les flux réseau doivent être strictement filtrés.
        4.  **Journalisation et surveillance :** Activer la journalisation de tous les accès (réussis et échoués) et mettre en place des alertes sur les comportements suspects (ex: connexions depuis des IP inconnues, tentatives d'élévation de privilèges).

---

## II) Évolution de la solution de paie

**1- La définition des différents rôles qui seront présentés à la cible dans le logiciel, le RACI résultant est un modèle d'implémentation de ces rôles.**

Sur la base du principe de **Séparation des Tâches (Separation of Duties - SoD)**, essentiel en gestion de la paie pour éviter les fraudes et les erreurs, on peut définir les rôles fonctionnels suivants (inspiré de M2 SSI 03 GIA, slides 16-18).

**Définition des Rôles :**

1.  **Administrateur Paie (ou Paramétreur Paie) :**
    *   **Responsabilités :** Configure les éléments structurels et légaux de la paie qui s'appliquent à tous. Ce rôle est très sensible car une erreur peut impacter toutes les fiches de paie.
    *   **Actions :** *La configuration des éléments de paye (niveau de taxes, de prélèvements, …).*

2.  **Gestionnaire RH :**
    *   **Responsabilités :** Gère le "dossier" de l'employé, de son entrée à sa sortie.
    *   **Actions :** *La définition des employés ainsi que le montant brut de leur paye.*

3.  **Manager Opérationnel (ou Assistant Paie) :**
    *   **Responsabilités :** Gère les éléments qui varient chaque mois pour les employés de son périmètre.
    *   **Actions :** *L'enregistrement des éléments variables de chacune des paies (frais, prime, …).*

4.  **Comptable Paie :**
    *   **Responsabilités :** Valide et exécute la paie. Il contrôle la cohérence des éléments et génère les livrables financiers.
    *   **Actions :** *L'édition des fiches de paie ainsi que des ordres de virement correspondants.*

5.  **Déclarant Social :**
    *   **Responsabilités :** S'assure de la conformité légale post-paie et de la transmission des informations aux organismes sociaux et fiscaux.
    *   **Actions :** *L'édition des états de relevés de taxes, pour transmission à l'administration.*

**Matrice RACI :**

| Tâche / Activité                                                | Administrateur Paie | Gestionnaire RH | Manager Opérationnel | Comptable Paie | Déclarant Social |
| --------------------------------------------------------------- | ------------------- | --------------- | -------------------- | -------------- | ---------------- |
| 1. Configurer les éléments de paie (taxes, prélèvements)        | **R** (Realizer)    | **I** (Informed)| **I** (Informed)     | **C** (Consulted) | **C** (Consulted) |
| 2. Définir les employés et leur salaire brut                    | **I** (Informed)    | **R** (Realizer)  | **I** (Informed)     | **A** (Accountable) | **I** (Informed) |
| 3. Enregistrer les éléments variables (primes, frais)           | **I** (Informed)    | **C** (Consulted) | **R** (Realizer)     | **A** (Accountable) | **I** (Informed) |
| 4. Éditer les fiches de paie et les ordres de virement          | **I** (Informed)    | **I** (Informed)  | **I** (Informed)     | **R/A** (Realizer/Accountable) | **I** (Informed) |
| 5. Éditer les relevés de taxes pour l'administration            | **C** (Consulted)   | **I** (Informed)  | **I** (Informed)     | **C** (Consulted) | **R/A** (Realizer/Accountable) |

*Légende : R=Realizer, A=Accountable (Approbateur/Responsable), C=Consulted, I=Informed.*
*Note : Le 'A' est placé sur le rôle qui a la responsabilité finale de la validité de la donnée. Pour les tâches 2 et 3, le Comptable est 'Accountable' car il doit contrôler ces saisies avant de payer.*

**2- Sachant que les PME ont généralement un service de ressources humaines réduit à un responsable et un gestionnaire. Proposez une répartition des rôles permettant de garantir la séparation des tâches.**

Dans un contexte de PME avec un **Responsable RH** et un **Gestionnaire RH**, la séparation des tâches (SoD) est un défi mais reste cruciale. On ne peut pas attribuer à la même personne un rôle de saisie et son rôle de contrôle direct.

**Répartition proposée :**

*   **Le Gestionnaire RH (profil plus opérationnel) :**
    *   Il se verra attribuer les rôles de **Gestionnaire RH** (création des fiches employés) et de **Manager Opérationnel** (saisie des variables de paie). Il est en charge de la préparation de la paie.
    *   *Rôles combinés : Gestionnaire RH + Manager Opérationnel.*

*   **Le Responsable RH (profil avec plus de responsabilités) :**
    *   Il se verra attribuer les rôles de **Comptable Paie** et de **Déclarant Social**. Son rôle principal sera de **contrôler** les données saisies par le gestionnaire, de **valider** le lancement des paiements (édition des fiches et virements) et de gérer les déclarations.
    *   Il sera également **Accountable** (A) pour la plupart des processus.
    *   *Rôles combinés : Comptable Paie + Déclarant Social.*

**Gestion du rôle "Administrateur Paie" :**

Le rôle d'**Administrateur Paie** (paramétrage des taxes) est le plus sensible et doit être traité avec un soin particulier. Deux options pour garantir la SoD :

1.  **Option 1 (Préférée) : Externalisation du paramétrage.** Le paramétrage des règles de paie est confié à un tiers de confiance (l'expert-comptable de la PME, par exemple). Le Responsable RH et le Gestionnaire RH n'ont alors que des droits de consultation sur cette partie. C'est la meilleure option pour une SoD stricte.
2.  **Option 2 (Interne avec contrôle renforcé) :** Le rôle est attribué au **Responsable RH**. Cependant, toute modification de paramétrage doit suivre un **processus de validation à quatre yeux** : le Responsable RH propose une modification, mais celle-ci ne peut être appliquée dans le système qu'après validation formelle (par exemple, par le dirigeant de la PME ou via un export de contrôle validé par l'expert-comptable). De plus, un audit de toutes les modifications de paramétrage doit être réalisé et revu périodiquement.

Cette répartition permet de s'assurer qu'aucune personne ne peut à la fois créer/modifier un employé ou ses variables de paie ET valider le paiement final sans une forme de contrôle, respectant ainsi le principe fondamental de la séparation des tâches.
