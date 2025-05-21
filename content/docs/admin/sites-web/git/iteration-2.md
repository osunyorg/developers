---
title: Itération 2
---

24 mars 2025, suite

## Création d'une nouvelle page

https://gist.github.com/SebouChu/a3c07aa94d9f40be9c0d05d8bd0e6f37

### 8 jobs

4 souris, 4 éléphants

| Order | Job | Explication | Paramètres |
| - | - | - | -  |
| 8 | Communication::Website::DestroyObsoleteGitFilesJob | Nettoyage effectif bis | "21561195-2fb4-4964-abe7-249fbd227dc1" |
| 7 | Communication::Website::IndirectObject::SyncWithGitJob | Synchro de la page l10n | "21561195-2fb4-4964-abe7-249fbd227dc1" |
| | | | indirect_object : gid://osuny/Communication::Website::Page::Localization/2ac6de57-bd44-49fe-bb9a-c2a745769049 |
| 6 | Communication::Website::CleanJob | Nettoyage | "21561195-2fb4-4964-abe7-249fbd227dc1" |
| 5 | Communication::Website::DirectObject::SyncWithGitJob | Synchro de la page | 21561195-2fb4-4964-abe7-249fbd227dc1 |
| | | | direct_object : gid://osuny/Communication::Website::Page/17ceed62-23cb-4970-adf6-e7914f7c3765 |
| 4 | Communication::Website::DirectObject::SyncWithGitJob | Synchro du menu | 21561195-2fb4-4964-abe7-249fbd227dc1 |
| | | | direct_object : gid://osuny/Communication::Website::Menu/25ffb66f-1556-4b1f-89c4-1d80aa2b9b73 |
| 3 | Dependencies::CleanWebsitesIfNecessaryJob | Test nettoyage | gid://osuny/Communication::Website::Page/17ceed62-23cb-4970-adf6-e7914f7c3765 |
| 2 | Communication::Website::IndirectObject::ConnectAndSyncDirectSourcesJob | Connexion et sync de la page l10n | gid://osuny/Communication::Website::Page::Localization/2ac6de57-bd44-49fe-bb9a-c2a745769049 |
| 1 | Dependencies::CleanWebsitesIfNecessaryJob | Test nettoyage (inutile en création) | gid://osuny/Communication::Website::Page::Localization/2ac6de57-bd44-49fe-bb9a-c2a745769049


### 2 commits

- [Save du menu](https://github.com/osunydev/demo-test-github-12/commit/372c2c5b7b8ae1148abc355dbe0f0cd5f66a249f)
- [Save de la page](https://github.com/osunydev/demo-test-github-12/commit/af5e94fdbb40c59746303eb8a8d4d2c458f06ce3)

### 12 requêtes Octokit


```
1. 16:46:44 worker.1 | [Octokit] request: GET https://api.github.com/repos/osunydev/demo-test-github-12/branches/main
2. 16:46:44 worker.1 | [Octokit] request: GET https://api.github.com/repos/osunydev/demo-test-github-12/git/trees/1272ab4468d846670c04290c3b14f0711abd0695?recursive=true
3. 16:46:44 worker.1 | [Octokit] request: POST https://api.github.com/repos/osunydev/demo-test-github-12/git/trees
4. 16:46:45 worker.1 | [Octokit] request: POST https://api.github.com/repos/osunydev/demo-test-github-12/git/commits
5. 16:46:45 worker.1 | [Octokit] request: PATCH https://api.github.com/repos/osunydev/demo-test-github-12/git/refs/heads/main
6. 16:46:46 worker.1 | [Octokit] request: GET https://api.github.com/repos/osunydev/demo-test-github-12/branches/main
7. 16:46:46 worker.1 | [Octokit] request: GET https://api.github.com/repos/osunydev/demo-test-github-12/git/trees/372c2c5b7b8ae1148abc355dbe0f0cd5f66a249f?recursive=true
8. 16:46:47 worker.1 | [Octokit] request: POST https://api.github.com/repos/osunydev/demo-test-github-12/git/trees
9. 16:46:47 worker.1 | [Octokit] request: POST https://api.github.com/repos/osunydev/demo-test-github-12/git/commits
10. 16:46:47 worker.1 | [Octokit] request: PATCH https://api.github.com/repos/osunydev/demo-test-github-12/git/refs/heads/main
11. 16:46:48 worker.1 | [Octokit] request: GET https://api.github.com/repos/osunydev/demo-test-github-12/branches/main
12. 16:46:49 worker.1 | [Octokit] request: GET https://api.github.com/repos/osunydev/demo-test-github-12/git/trees/af5e94fdbb40c59746303eb8a8d4d2c458f06ce3?recursive=true
```

1. Récupération du sha (menu)
2. Récupération de l'arbre
3. Création de l'arbre de modifications
4. Commit
5. Définition du commit comme actuel
6. Récupération du sha (page)
7. Récupération de l'arbre
8. Création de l'arbre de modifications
9. Commit
10. Définition du commit comme actuel
11. Récupération du sha (page l10n)
12. Récupération de l'arbre


## Création d'un bloc chapitre

https://gist.github.com/SebouChu/5be8f6e3817814087ec2410d28d49dee

### 7 jobs

| Order | ID / Job | Explication | Paramètres |
| - | - | - | - |
| 7 | e4096045-a669-43fc-9755-f04389b2c516 Communication::Website::DestroyObsoleteGitFilesJob | Nettoyage bis | |
| 6 | faa41862-1421-440c-9d67-caa2ee4a93a7 Communication::Website::IndirectObject::SyncWithGitJob | Synchro de la page l10n | "21561195-2fb4-4964-abe7-249fbd227dc1", {"indirect_object" => {"_aj_globalid" => "gid://osuny/Communication::Website::Page::Localization/2ac6de57-bd44-49fe-bb9a-c2a745769049"}, "_aj_ruby2_keywords" => ["indirect_object"]} |
| 5 | 9dfb9891-2bb2-49a9-b6b6-0233787a6f58 Communication::Website::IndirectObject::SyncWithGitJob | Synchro du bloc | "21561195-2fb4-4964-abe7-249fbd227dc1", {"indirect_object" => {"_aj_globalid" => "gid://osuny/Communication::Block/362a56a2-cbc1-4a35-9121-67e9210789bc"}, "_aj_ruby2_keywords" => ["indirect_object"]} | 
| 4 | ed650a88-be2e-49ac-addd-d0887dcb36f2 Communication::Website::CleanJob | Nettoyage du site | |
| 3 | 522422f1-863d-48d3-9fbc-5ae81766cc2c Communication::Website::IndirectObject::ConnectAndSyncDirectSourcesJob | Connexion et sync de la page l10n | {"_aj_globalid" => "gid://osuny/Communication::Website::Page::Localization/2ac6de57-bd44-49fe-bb9a-c2a745769049"} |
| 2 | 228a6f48-e217-42c3-a38d-b3c104589074 Communication::Website::IndirectObject::ConnectAndSyncDirectSourcesJob | Connexion et sync du bloc | {"_aj_globalid" => "gid://osuny/Communication::Block/362a56a2-cbc1-4a35-9121-67e9210789bc"} |
| 1 | 110f535b-d09e-4916-b6cd-be685ebe5733 Dependencies::CleanWebsitesIfNecessaryJob | Test nettoyage | |

### 1 commit

- [Ajout du bloc](https://github.com/osunydev/demo-test-github-12/commit/ddcc256d01cd835a924ed841d953951c50559677)

### 7 requêtes Octokit

```
1. 17:11:25 worker.1 | [Octokit] request: GET https://api.github.com/repos/osunydev/demo-test-github-12/branches/main
2. 17:11:26 worker.1 | [Octokit] request: GET https://api.github.com/repos/osunydev/demo-test-github-12/git/trees/af5e94fdbb40c59746303eb8a8d4d2c458f06ce3?recursive=true
3. 17:11:26 worker.1 | [Octokit] request: POST https://api.github.com/repos/osunydev/demo-test-github-12/git/trees
4. 17:11:26 worker.1 | [Octokit] request: POST https://api.github.com/repos/osunydev/demo-test-github-12/git/commits
5. 17:11:27 worker.1 | [Octokit] request: PATCH https://api.github.com/repos/osunydev/demo-test-github-12/git/refs/heads/main
6. 17:11:28 worker.1 | [Octokit] request: GET https://api.github.com/repos/osunydev/demo-test-github-12/branches/main
7. 17:11:28 worker.1 | [Octokit] request: GET https://api.github.com/repos/osunydev/demo-test-github-12/git/trees/ddcc256d01cd835a924ed841d953951c50559677?recursive=true
```

1. Récupération du sha (bloc)
2. Récupération de l'arbre
3. Création de l'arbre de modifications
4. Commit
5. Définition du commit comme actuel
6. Récupération du sha (page)
7. Récupération de l'arbre

## Création d'un bloc chapitre V2

En désactivant le partie "Connexion et sync du bloc" qui peut faire doublon avec la partie "Connexion et sync de la page l10n", on s'économise la création des jobs 2 et 5, et ainsi 2 requêtes Octokit.
