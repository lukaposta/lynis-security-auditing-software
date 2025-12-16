# 2. Instalacija alata Lynis i provođenje inicijalnog sigurnosnog audita

U ovoj cjelini opisuje se instalacija alata Lynis te provođenje inicijalnog sigurnosnog audita Linux poslužiteljskog sustava. Cilj ove faze je uspostaviti početno, referentno stanje sigurnosne konfiguracije sustava prije primjene bilo kakvih hardening mjera.

Sigurnosni audit provodi se nad sustavom u stanju koje je dokumentirano u prethodnoj cjelini, bez dodatnih sigurnosnih prilagodbi. Dobiveni rezultati služe isključivo kao polazna točka za kasniju analizu i usporedbu s post-hardening stanjem sustava. Interpretacija nalaza i evaluacija sigurnosnih poboljšanja ne provode se u ovoj cjelini, već u zasebnom dijelu rada posvećenom analizi rezultata.

## 2.1. Instalacija alata Lynis

Lynis je open-source alat za sigurnosni audit namijenjen UNIX i Linux sustavima. Omogućuje provođenje sveobuhvatne provjere konfiguracije operacijskog sustava, instaliranih servisa, sigurnosnih postavki i potencijalnih ranjivosti, uz generiranje preporuka za poboljšanje sigurnosti sustava.

Za potrebe ovog projekta Lynis je instaliran korištenjem službenih Ubuntu repozitorija. Time je osigurana jednostavna instalacija alata, automatsko rješavanje ovisnosti te usklađenost s paketnim sustavom operacijskog sustava.

Instalacija alata provedena je korištenjem sljedećih naredbi:

```bash
sudo apt update
sudo apt install lynis -y
```
Nakon završetka instalacije izvršena je provjera dostupnosti alata i instalirane verzije Lynisa kako bi se potvrdilo da je alat ispravno instaliran i spreman za korištenje:

```bash
lynis --version
```
Uspješnim izvođenjem navedene naredbe potvrđeno je da je Lynis ispravno instaliran na sustav i da se može koristiti za provođenje sigurnosnog audita u sljedećim fazama projekta.

## 2.2. Osnovna konfiguracija i priprema alata Lynis

Nakon uspješne instalacije alata Lynis nije provedena dodatna konfiguracija alata niti prilagodba audit profila. Lynis je korišten u svojoj zadanoj konfiguraciji kako bi inicijalni sigurnosni audit odražavao stvarno početno stanje sustava bez unaprijed primijenjenih sigurnosnih politika ili prilagodbi.

Lynis se izvršava kao lokalni alat za sigurnosni audit te zahtijeva administratorske ovlasti kako bi mogao provjeriti konfiguracijske datoteke, sistemske postavke, kernel parametre i sigurnosne mehanizme operacijskog sustava. Iz tog razloga audit se pokreće korištenjem korisničkog računa s `sudo` ovlastima.

Tijekom pripreme alata identificirane su zadane lokacije u kojima Lynis pohranjuje rezultate i zapise audita. Ključne datoteke koje Lynis generira tijekom izvođenja audita su:

- `/var/log/lynis.log` - detaljan zapis provedenog audita i izvršenih testova  
- `/var/log/lynis-report.dat` - sažeti izvještaj s identificiranim nalazima, upozorenjima i preporukama  
- `/usr/share/lynis/` - direktorij koji sadrži testove, module i zadane profile alata

Alat Lynis korišten je isključivo u lokalnom, open-source izdanju. Komercijalne značajke poput centraliziranog upravljanja, Enterprise modula i naprednih izvještaja nisu bile u fokusu ovog rada, budući da je cilj projekta analiza i ručna primjena sigurnosnih preporuka na pojedinačnom Linux poslužiteljskom sustavu.

## 2.3. Provođenje inicijalnog (baseline) sigurnosnog audita

Nakon instalacije i osnovne pripreme alata Lynis proveden je inicijalni sigurnosni audit sustava. Ovaj audit predstavlja tzv. *baseline* stanje sigurnosti sustava, odnosno referentnu točku prije primjene bilo kakvih hardening mjera.

Sigurnosni audit pokrenut je s administratorskim ovlastima kako bi Lynis imao puni pristup konfiguracijskim datotekama, sistemskim postavkama, servisima i kernel parametrima operacijskog sustava. Audit je obuhvatio provjeru širokog raspona sigurnosnih aspekata, uključujući konfiguraciju korisničkih računa, autentikaciju, mrežne postavke, servisne konfiguracije, boot sigurnost, logiranje te osnovne kernel postavke.

Audit je pokrenut sljedećom naredbom:

```bash
sudo lynis audit system
```

Tijekom izvođenja audita Lynis je interaktivno prikazivao informacije o testovima koji se provode, njihovom statusu te eventualnim pronađenim problemima i preporukama. Izvođenje audita trajalo je nekoliko minuta, ovisno o broju instaliranih paketa i konfiguraciji sustava.

Po završetku audita Lynis je automatski generirao zapise i izvještaje koji sadrže detaljne rezultate provedenih testova. Ovi zapisi predstavljaju temelj za kasniju analizu sigurnosnog stanja sustava te usporedbu s rezultatima dobivenima nakon primjene sigurnosnih hardening mjera.

## 2.4. Pohrana i organizacija rezultata inicijalnog audita

Nakon završetka inicijalnog sigurnosnog audita rezultati audita pohranjeni su u zadanim Lynis lokacijama na sustavu. Ove datoteke predstavljaju službeni zapis *baseline* sigurnosnog stanja sustava te služe kao temelj za kasniju usporedbu s rezultatima dobivenima nakon primjene hardening mjera.

Kako bi se osigurala jasna razlika između inicijalnog i post-hardening stanja sustava, rezultati inicijalnog audita izdvojeni su i arhivirani kao *baseline* zapisi. Time se sprječava prepisivanje podataka tijekom kasnijih ponovnih pokretanja Lynis audita te se omogućuje transparentna usporedba sigurnosnog stanja sustava prije i nakon primjene sigurnosnih mjera.

Uz lokalnu pohranu na virtualnom stroju, datoteke s rezultatima inicijalnog audita dodatno su kopirane u dijeljeni direktorij konfiguriran putem Oracle VirtualBox *Shared Folder* mehanizma. Na taj je način omogućeno jednostavno preuzimanje *baseline* rezultata na host sustav te njihovo dugoročno čuvanje i daljnja obrada izvan virtualnog okruženja.

Korištenje dijeljenog direktorija omogućuje pouzdanu pohranu rezultata audita, neovisno o stanju virtualnog stroja, te dodatno doprinosi ponovljivosti i transparentnosti provedenog eksperimenta. Analiza sadržaja navedenih datoteka, interpretacija pronađenih nalaza i usporedba s post-hardening auditom provode se u zasebnoj cjelini rada posvećenoj rezultatima sigurnosnog audita.

## 2.5. Završna napomena o inicijalnom sigurnosnom auditu

Provedbom inicijalnog sigurnosnog audita alatom Lynis uspostavljena je jasna referentna točka sigurnosnog stanja sustava prije primjene hardening mjera. Dobiveni *baseline* rezultati dokumentiraju stvarno početno stanje konfiguracije operacijskog sustava te omogućuju objektivnu procjenu učinkovitosti kasnije implementiranih sigurnosnih poboljšanja.

U ovoj cjelini rada nije provedena interpretacija niti evaluacija identificiranih nalaza. Svi rezultati inicijalnog audita analiziraju se u zasebnom dijelu rada posvećenom rezultatima sigurnosnog audita, gdje se provodi usporedba *baseline* i post-hardening stanja sustava.

Na temelju inicijalnog audita definirane su prioritetne sigurnosne mjere koje se implementiraju u sljedećim fazama projekta, uz referencu na preporuke alata Lynis i smjernice CIS Benchmarks za Ubuntu Linux. Time je osigurana metodološka konzistentnost i jasna razdvojenost između faze prikupljanja podataka i faze primjene sigurnosnih mjera.

