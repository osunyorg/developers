---
title: Multitenant
weight: 1
---

## Définition

Osuny est une application multitenant : une seule instance logicielle sert plusieurs clients instances (les universités). Chaque instance (ou tenant) bénéficie d'une isolation complète de ses données et d'une personnalisation de son environnement, tout en partageant la même infrastructure et le même code.

## Caractéristiques principales

**Isolation des données** : Chaque université ne voit que ses propres données. Les contenus, utilisateurs et configurations d'un tenant sont invisibles et inaccessibles aux autres.

**Base de données partagée** : Tous les tenants partagent le même schéma de base de données, mais chaque enregistrement est associé à une université spécifique via des clés étrangères (`university_id`).

**Personnalisation** : Chaque tenant peut configurer son identité visuelle, ses paramètres SSO, ses langues et ses flux de travail tout en utilisant la même base de code.

**Gouvernance distribuée** : Les administrateurs de chaque université gèrent leurs propres utilisateurs, contenus et paramètres, sans pouvoir interférer avec d'autres tenants.

## Implémentation concrète

1. **Modèle Central** : La table `universities` contient tous les tenants avec leurs paramètres spécifiques (nom, logo, configuration SSO, etc.).

2. **Association systématique** : Presque tous les modèles (utilisateurs, organisations, sites web, programmes, médias, etc.) appartiennent à une université via `belongs_to :university`.

3. **Validation obligatoire** : Chaque création/modification de contenu vérifie la présence de l'association à une université.

4. **Filtrage automatique** : Les requêtes sont systématiquement filtrées par université pour garantir l'isolation.

5. **Authentification isolée** : Chaque tenant peut configurer son propre système SSO (SAML) avec son certificat et son mappage d'attributs.

## Conséquences pratiques

- Un utilisateur connecté à l'Université A ne peut pas accéder aux données de l'Université B
- Les contenus créés sont automatiquement marqués avec l'ID de l'université du créateur
- Les administrateurs ne voient que les utilisateurs et contenus de leur propre institution
- Les paramètres globaux (comme la configuration SSO) sont spécifiques à chaque tenant
- Les mises à jour de la plateforme bénéficient à tous les tenants simultanément

## Avantages techniques

- **Maintenance simplifiée** : Une seule instance à déployer et surveiller
- **Économies de ressources** : Infrastructure mutualisée entre tous les tenants
- **Mises à jour instantanées** : Les nouvelles fonctionnalités sont disponibles pour tous
- **Sécurité renforcée** : Corrections de sécurité appliquées globalement
- **Extensibilité** : Ajout de nouveaux tenants sans modification du code

## Limites et contraintes

- **Pas de personnalisation du code** : Tous les tenants utilisent la même version du logiciel
- **Performances partagées** : Les ressources serveurs sont mutualisées entre tenants
- **Migrations globales** : Les changements de schéma affectent tous les tenants simultanément
- **Configuration limitée** : Seuls les paramètres prévus peuvent être personnalisés par tenant

Cette architecture multitenant permet à Osuny de servir efficacement de nombreuses universités tout en maintenant un code cohérent et une infrastructure optimisée.