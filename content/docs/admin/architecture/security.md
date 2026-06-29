---
title: Sécurisée
weight: 3
---


## Définition

Osuny intègre une architecture de sécurité complète conforme aux engagements publiés sur [osuny.org/engagements/securite](https://www.osuny.org/engagements/securite/) et certifiée avec un niveau de sécurité "bon" selon l'audit du 30 avril 2024 ([détails](https://www.osuny.org/actualites/2024-04-30-l-application-osuny-presente-le-niveau-de-securite-bon/)).

## Caractéristiques principales

**Authentification Multi-Facteurs (MFA)** : Osuny supporte l'authentification à deux facteurs avec TOTP (Time-based One-Time Password) et codes OTP directs. La configuration inclut 6 chiffres, une tolérance de 30 secondes pour la dérive temporelle, et une session mémorisée configurable.

**Gestion des rôles et permissions** : Système de contrôle d'accès basé sur les rôles (RBAC) avec 9 niveaux hiérarchiques (visiteur, contributeur, auteur, enseignant, gestionnaire de programme, gestionnaire de site, gestionnaire d'alumni, administrateur, super-administrateur).

**Protection des comptes** : Mécanismes de verrouillage après 3 tentatives infructueuses, déverrouillage par email ou temporisé (1 heure), avertissement avant dernier essai, et expiration des sessions après inactivité (60 minutes en production).

**Authentification unique (SSO)** : Intégration SAML avec mappage configurable des attributs, permettant aux universités de connecter leur annuaire existant tout en maintenant la sécurité des accès.

**Gestion des mots de passe** : Hachage bcrypt avec coût 12, longueur minimale de 8 caractères, aide pour les caractères spéciaux, expiration des tokens de réinitialisation après 6 heures, et notifications pour les changements de mot de passe et d'email.

**Conformité RGPD** : Suppression automatique des comptes inactifs après 3 ans (1095 jours) avec notification préalable 30 jours avant suppression, conformément aux engagements de protection des données.

## Implémentation concrète

1. **Devise avec configuration sécurisée** :
   - Clés d'authentification : email + university_id pour garantir l'isolation entre tenants
   - Mode paranoïde activé pour éviter les fuites d'information
   - Tokens CSRF nettoyés à l'authentification pour éviter les attaques
   - Confirmation des comptes avec délai de 1 jour pour l'accès temporaire

2. **Contrôle d'accès basé sur les rôles** :
   - Hiérarchie stricte : un rôle supérieur peut gérer les rôles inférieurs
   - Portées automatiques pour filtrer les ressources accessibles
   - Vérification systématique des permissions via CanCan::Ability
   - Isolation des données par université et par site web

3. **Protection des données sensibles** :
   - Chiffrement des secrets OTP avec clé dédiée
   - Hachage des mots de passe avec salt unique
   - Masquage des attributs sensibles dans les logs
   - Nettoyage des sessions et tokens après déconnexion

4. **Conformité RGPD** :
   - Suppression automatique des comptes inactifs après 3 ans (1095 jours)
   - Notification préalable 30 jours avant suppression
   - Processus de suppression complet incluant les données associées
   - Respect du droit à l'oubli et à la portabilité

5. **Sécurité des sessions** :
   - Expiration automatique après inactivité (60 min en prod, 30 jours en dev)
   - Invalidation des tokens "remember me" à la déconnexion
   - Protection contre les attaques CSRF et XSS
   - Gestion sécurisée des cookies avec options HTTP-only et Secure

## Niveau de sécurité "Bon"

Selon l'audit de sécurité du 30 avril 2024, Osuny présente un niveau de sécurité "bon" qui se traduit par :

- **Protection des données personnelles** : Chiffrement des données sensibles et respect strict du RGPD
- **Authentification forte** : MFA obligatoire pour les rôles sensibles et SSO sécurisé
- **Isolation des tenants** : Architecture multitenant garantissant l'étanchéité entre universités
- **Gestion des accès** : Contrôle d'accès granulaire et journalisation des activités
- **Mises à jour régulières** : Maintenance proactive et corrections des vulnérabilités

## Conséquences pratiques

- Les utilisateurs doivent s'authentifier avec leur email ET leur université
- Les comptes se verrouillent après 3 échecs de connexion consécutifs
- Les sessions expirent automatiquement après inactivité
- Les administrateurs ne peuvent pas modifier les rôles des utilisateurs ayant un rôle supérieur
- Les données des utilisateurs inactifs sont automatiquement supprimées après 3 ans
- Chaque université gère ses propres utilisateurs et permissions de manière isolée

## Avantages techniques

- **Sécurité renforcée** : Combinaison de plusieurs couches de protection
- **Isolation complète** : Pas d'interférence possible entre les tenants
- **Conformité réglementaire** : Respect des exigences RGPD et standards publics
- **Expérience utilisateur sécurisée** : Protection sans compromettre l'utilisabilité
- **Maintenabilité** : Centralisation des politiques de sécurité
