
# Mapování uživatelů v Galaxy/Pulsaru/PBS

Zohlednit všechny cílové scénáře:
- usegalaxy.cz
- usegalaxy.eu -> náš Pulsar
- RepeatExplorer
- Recetox

## AuthN

Jedna federace vládne všem, usegalaxy.eu používá Elixir AAI, chceme něco jiného (nanejvýš upgradem na LifeScience AAI)?

## AuthZ

- jak dostaneme informace o skupinách (minimálně z .eu, kdo je "náš" a má mít privilegovaný přístup)
- jak řešit výběr, která z více skupin je aktivní
...

## Galaxy

Asi není co řešit, běží pod jedním servisním unixovým uživatelem tak jak tak.

## Pulsar

TODO

## PBS

### Servisní uživatel

(případně více různých pro různé instance)

- default přístup v Galaxy, méně hacků
- v PBS neodlišíme uživatele, kvoty apod. musí řešit komplet Galaxy

### Uživatelské účty
- napojeno na Meta
- autentizovaný přímý přístup k souborům (teoreticky)
- možnost společného accountingu, fairshare atd. v PBS
- nezbytné řešení pro vyrobení a prodlužování kerberosího lístku
- pravděpodopbně řešení s více hacky
