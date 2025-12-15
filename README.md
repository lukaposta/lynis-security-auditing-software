# Lynis Security Auditing Software

## Pregled projekta

Ovaj projekt bavi se sigurnosnom analizom i osnovnim učvršćivanjem Linux poslužiteljskog sustava pomoću alata Lynis, uz korištenje CIS Benchmarks dokumenata kao referentnog sigurnosnog standarda.

Cilj projekta je identificirati sigurnosne slabosti na svježe instaliranom sustavu, analizirati rezultate sigurnosne provjere, primijeniti odabrane mjere učvršćivanja te provjeriti poboljšanje sigurnosti ponovnim pokretanjem sigurnosne analize.

Projekt se provodi u kontroliranom virtualnom okruženju. Sigurnosne preporuke koje generira alat Lynis uspoređuju se s odgovarajućim CIS Benchmarks smjernicama kako bi se opravdale i objasnile provedene konfiguracijske promjene.

## Korišteni operacijski sustav

- Ubuntu Server 22.04 LTS

## Alati i standardi

- VirtualBox - platforma za virtualizaciju  
- Lynis - alat za sigurnosnu analizu sustava  
- CIS Benchmarks - referentni sigurnosni standard  
- NIST SP 800-123 - teorijska podloga za učvršćivanje sustava  

## Tijek rada

1. Instalacija Ubuntu Server operacijskog sustava u virtualnom okruženju  
2. Pokretanje početne sigurnosne analize pomoću alata Lynis  
3. Analiza pronađenih upozorenja i preporuka  
4. Odabir i primjena osnovnih mjera učvršćivanja sustava  
5. Ponovno pokretanje sigurnosne analize nakon primjene promjena  
6. Usporedba rezultata prije i nakon učvršćivanja sustava s CIS preporukama  

## Struktura repozitorija

- `docs/` - pisani dio rada prema zadanoj strukturi (uvod, teorija, praktični dio, usporedba, zaključak, literatura)  
- `implementation/` - opis korištenog okruženja, instalacije alata i primijenjenih hardening koraka  
- `results/` - sažeti rezultati sigurnosnih analiza prije i nakon učvršćivanja sustava  
- `presentation/` - završna prezentacija projekta  

## Članovi tima

- Lidija Mudri  
- Paula Narančić  
- Vito Petrinjak  
- Luka Pošta  

## Pokretanje projekta

Projekt ne sadrži izvršni aplikacijski kod.

Za ponavljanje provedene sigurnosne analize potrebno je instalirati Ubuntu Server 22.04 LTS u virtualnom okruženju te slijediti korake opisane u direktoriju `implementation/`.
