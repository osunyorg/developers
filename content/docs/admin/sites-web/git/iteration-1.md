---
title: Itération 1
---

24 mars 2025

## Création d'un nouveau site 

Sans fonctionnalités

https://gist.github.com/SebouChu/95d8d441ad763534e1a6a1d451229256

140 requêtes loggées

107     INFO -- : request: GET
21      INFO -- : request: POST
10      INFO -- : request: PATCH
2       INFO -- : request: PUT

## Activation du cache Faraday

https://gist.github.com/SebouChu/5b110b9735df983735497b816711c66b

x-ratelimit-used: "1000"
x-ratelimit-used: "1138"

140 requêtes

shared_cache: false et toutes les requêtes sont privées, donc pas de cache

## shared_cache: true

https://gist.github.com/SebouChu/4eae82c9a18186fa0422e21ec6e74b64

x-ratelimit-used: "1344"
x-ratelimit-used: "1533"

140 requêtes

## set store

https://gist.github.com/SebouChu/e80c80bb78f1930bfd633b3c345ceb50

x-ratelimit-used: "1681"
x-ratelimit-used: "1843"

140 requêtes


## shared_cache: false

https://gist.github.com/SebouChu/667dfe4ef3a7ac6d937613caf29c39e6



On avait compris à l'envers :)

x-ratelimit-used: "1919"
x-ratelimit-used: "1923"

On a juste récupéré des ressources, pas de gain.

https://evilmartians.com/chronicles/down-the-caching-hole-adventures-in-http-caching-and-faraday-land

-> on supprime le cache


https://gist.github.com/SebouChu/efe6ce0822c8c8fe469a63f35a08fa54


## early return

https://gist.github.com/SebouChu/fe71c67514de1a39dc8c4c40d9ea5954

https://github.com/osunydev/demo-test-github-12


On n'interroge plus l'API pour `valid?` ni pour `default_branch`


76 requêtes (gain de 64 !)


x-ratelimit-used: "442"
x-ratelimit-used: "526"


43   GET
21   POST (la génération, les arbres et les commits)
10   PATCH (les mises à jour de main)
2    PUT (les secrets)