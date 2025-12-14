# Lynis Security Auditing Software

## Pregled projekta

Ovaj projekt bavi se sigurnosnom analizom i učvršćivanjem UNIX-baziranih operacijskih sustava pomoću alata Lynis, uz korištenje CIS Benchmarks dokumenata kao referentnog sigurnosnog standarda.

Cilj projekta je identificirati sigurnosne slabosti na svježe instaliranim sustavima, analizirati rezultate sigurnosne provjere, primijeniti odabrane mjere učvršćivanja sustava te provjeriti poboljšanje sigurnosti ponovnim pokretanjem analize.

Projekt se provodi u kontroliranom virtualnom okruženju korištenjem više virtualnih računala. Sigurnosne preporuke koje generira alat Lynis uspoređuju se s odgovarajućim CIS Benchmarks smjernicama kako bi se opravdale i objasnile provedene konfiguracijske promjene.

## Korišteni operacijski sustavi

- Ubuntu Server 22.04 LTS  
- Rocky Linux 9  
- FreeBSD 14  

## Alati i standardi

- VirtualBox - platforma za virtualizaciju  
- Lynis - alat za sigurnosnu analizu sustava  
- CIS Benchmarks - referentni sigurnosni standard  

## Tijek rada

1. Instalacija virtualnih računala s početnim postavkama sustava  
2. Pokretanje početne sigurnosne analize pomoću alata Lynis  
3. Pregled i analiza pronađenih upozorenja i preporuka  
4. Odabir i primjena mjera učvršćivanja sustava  
5. Ponovno pokretanje sigurnosne analize nakon primjene promjena  
6. Usporedba i analiza rezultata prije i nakon učvršćivanja sustava  

## Struktura repozitorija

- `docs/` - teorijska podloga, metodologija, CIS reference i zaključci  
- `implementation/` - upute za postavljanje okruženja i koraci učvršćivanja za svaki operacijski sustav  
- `results/` - rezultati sigurnosnih analiza, usporedbe i snimke zaslona  
- `presentation/` - završna prezentacija projekta  

## Članovi tima

- Lidija Mudri  
- Paula Narančić  
- Vito Petrinjak  
- Luka Pošta  

## Pokretanje projekta

Projekt ne sadrži izvršni aplikacijski kod.

Za ponavljanje provedenih analiza potrebno je slijediti upute za postavljanje sustava i izvođenje sigurnosne analize koje se nalaze u direktoriju `implementation/` za svaki operacijski sustav.
