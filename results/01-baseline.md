# 1. Inicijalni sigurnosni audit sustava (baseline stanje)

U ovoj cjelini prikazani su rezultati inicijalnog sigurnosnog audita Linux poslužiteljskog sustava provedenog prije primjene bilo kakvih hardening mjera. Cilj ove analize je dokumentirati stvarno početno stanje sigurnosne konfiguracije sustava te identificirati slabosti koje će biti adresirane u kasnijim fazama projekta.

Sustav u trenutku provođenja audita odgovara stanju opisanom u implementacijskoj cjelini rada te predstavlja standardnu, svježe instaliranu i ažuriranu Ubuntu Server konfiguraciju. U ovoj cjelini ne provode se korektivne mjere niti izmjene sustava, već se isključivo analiziraju i dokumentiraju nalazi inicijalnog audita. Ovi nalazi služe kao referentna točka za usporedbu s rezultatima dobivenima nakon provedbe sigurnosnog hardeninga.

## 1.1. Opći podaci o provedenom auditu

U ovom poglavlju prikazani su osnovni podaci o provedenom sigurnosnom auditu, uključujući korišteni alat, način izvođenja audita i inicijalne metrike sigurnosti sustava. Ovi podaci omogućuju kontekstualno razumijevanje rezultata audita te osiguravaju ponovljivost i transparentnost analize.

### 1.1.1. Alat i verzija

Sigurnosni audit sustava proveden je korištenjem alata **Lynis**, open-source rješenja namijenjenog sigurnosnoj provjeri UNIX i Linux operacijskih sustava. Lynis provodi automatizirani audit konfiguracije operacijskog sustava, servisa, autentikacijskih mehanizama, boot procesa, logiranja, kernel postavki i mrežnih parametara te generira detaljan zapis rezultata i preporuka.

Audit je proveden korištenjem **Lynis verzije 3.0.7**, instalirane iz službenih Ubuntu repozitorija. Alat je pokrenut u zadanoj konfiguraciji, bez prilagodbe audit profila ili izuzetaka, kako bi inicijalni audit vjerno odražavao stvarno početno stanje sustava.

Sigurnosni audit izvršen je nad sljedećim operacijskim sustavom i okruženjem:

- Operacijski sustav: **Ubuntu**
- Puna oznaka sustava: **Ubuntu 22.04.5 LTS**
- Verzija distribucije: **22.04**
- Kernel verzija: **5.15.0**
- Puna verzija kernela: **5.15.0-164-generic**
- Init sustav: **systemd**
- Naziv računala: **ubuntu-lynis**
- Virtualizacijsko okruženje: **Oracle VirtualBox**
- Kontejnersko okruženje: **nije korišteno**

Audit je pokrenut nad svim dostupnim testnim kategorijama i testnim grupama alata Lynis. Nisu korištena ograničenja opsega audita, čime je osigurano da su provjereni svi relevantni sigurnosni aspekti sustava, uključujući autentikaciju, upravljanje korisnicima, boot sigurnost, servisne konfiguracije, kernel postavke, mrežne parametre, logiranje i auditing.

Vrijeme početka izvođenja sigurnosnog audita zabilježeno je u izvještaju alata Lynis, čime je osigurana vremenska sljedivost provedenog audita i jasna identifikacija *baseline* stanja sustava. Rezultati audita odnose se isključivo na konfiguraciju sustava u tom trenutku, bez naknadnih izmjena ili intervencija.

## 1.1.2. Hardening index – inicijalno stanje sustava

Nakon završetka inicijalnog sigurnosnog audita alat Lynis izračunao je ukupni *hardening index* sustava. Hardening index predstavlja numerički pokazatelj razine sigurnosne konfiguracije sustava, izražen u rasponu od 0 do 100, pri čemu veća vrijednost označava višu razinu implementiranih sigurnosnih mjera.

U inicijalnom (*baseline*) stanju sustava zabilježen je hardening index od:

**60 / 100**

Dobivena vrijednost ukazuje na djelomično utvrđen sustav, u kojem su prisutne određene osnovne sigurnosne mjere, ali bez sustavne primjene preporučenih hardening politika. Takav rezultat je očekivan za svježe instaliran Ubuntu Server sustav bez ručne konfiguracije sigurnosnih postavki.

Alat Lynis u izvještaju navodi da je sustav funkcionalan i stabilan, ali da postoji značajan prostor za poboljšanje sigurnosti kroz dodatne hardening mjere. Identificirani su brojni sigurnosni nalazi, upozorenja i preporuke koji se odnose na autentikaciju korisnika, upravljanje lozinkama, konfiguraciju PAM modula, boot sigurnost, logiranje i auditing, kernel parametre te mrežne postavke.

Hardening index u ovoj fazi rada služi isključivo kao referentna vrijednost. Ne koristi se za ocjenjivanje sigurnosti sustava u apsolutnom smislu, već kao polazišna točka za kasniju usporedbu s vrijednostima dobivenima nakon provedbe sigurnosnih hardening mjera.

U sljedećim poglavljima detaljno se dokumentiraju pojedinačni nalazi inicijalnog audita, grupirani po sigurnosnim područjima, uz jasno navođenje parametara koji su identificirani kao kandidati za hardening u kasnijim fazama projekta.


## 1.2. Identificirani sigurnosni nalazi inicijalnog audita

U ovoj cjelini dokumentirani su sigurnosni nalazi identificirani tijekom inicijalnog (*baseline*) audita alatom Lynis. Nalazi predstavljaju stvarno početno stanje sustava prije primjene hardening mjera i služe kao temelj za kasniju implementaciju sigurnosnih poboljšanja.

Prikazani su isključivo nalazi koji su imali utjecaj na sigurnosni profil sustava, odnosno oni koji su rezultirali upozorenjima (*warnings*) ili eksplicitnim preporukama za poboljšanje sigurnosti. Informativni testovi bez sigurnosnog utjecaja nisu detaljno analizirani u ovoj fazi rada.


### 1.2.1. Firewall i mrežna filtracija

Inicijalni sigurnosni audit alatom Lynis identificirao je nedostatke u konfiguraciji mrežne filtracije prometa koji zahtijevaju primjenu sigurnosnih hardening mjera. U nastavku su navedeni isključivo oni nalazi koji izravno predstavljaju ulazne točke za hardening firewall konfiguracije.

### Prazan iptables ruleset

Lynis je identificirao da su iptables kernel moduli učitani, ali da ne postoje aktivna pravila za filtriranje mrežnog prometa, što je označeno kao sigurnosno upozorenje:

```bash
Performing test ID FIRE-4512 (Check iptables for empty ruleset)
Result: iptables ruleset seems to be empty (found 0 rules)
Warning: iptables module(s) loaded, but no rules active [test:FIRE-4512]
```

Navedeni nalaz ukazuje da u početnom stanju sustava ne postoji definirana politika filtriranja mrežnog prometa putem iptables mehanizma.

---
### Ne-restriktivne zadane iptables politike

Provjerom zadanih politika iptables lanaca utvrđeno je da su sve osnovne politike postavljene na vrijednost ACCEPT:

```bash
Performing test ID FIRE-4508 (Check used policies of iptables chains)
Result: iptables filter -- INPUT ACCEPT FORWARD ACCEPT OUTPUT ACCEPT policy is ACCEPT
```


Ovaj nalaz potvrđuje da ulazni, izlazni i proslijeđeni mrežni promet nisu ograničeni zadanim firewall pravilima.

---
### Prazna nftables konfiguracija

Audit je također pokazao da nftables mehanizam, koji predstavlja zadani backend za mrežnu filtraciju na Ubuntu 22.04 sustavima, ne sadrži stvarnu konfiguraciju pravila:

```bash
Performing test ID FIRE-4540 (Check for empty nftables configuration)
Result: this firewall set has 3 rules or less and is considered to be empty
```

Ovaj nalaz ukazuje da ni nftables sustav u inicijalnom stanju ne provodi učinkovitu mrežnu filtraciju prometa.

---
### Izostanak firewall logiranja

Provjera firewall konfiguracije nije identificirala aktivno logiranje filtriranog ili odbijenog mrežnog prometa:

```bash
Performing test ID FIRE-4586 (Check firewall logging)
```

Izostanak firewall logiranja predstavlja dodatnu sigurnosnu slabost koja onemogućuje nadzor i analizu mrežnih sigurnosnih događaja.

---
### Sažetak nalaza relevantnih za hardening

Rezultati inicijalnog audita ukazuju da u *baseline* stanju sustava nije implementirana učinkovita firewall politika. Identificirani nedostaci odnose se na izostanak definiranih pravila mrežne filtracije, ne-restriktivne zadane politike iptables lanaca, praznu nftables konfiguraciju te nedostatak firewall logiranja. Navedeni nalazi predstavljaju temelj za primjenu sigurnosnih hardening mjera u sljedećoj fazi rada.


### 1.2.2. SSH konfiguracija

Inicijalni sigurnosni audit alatom Lynis identificirao je više slabosti u konfiguraciji SSH servisa koje zahtijevaju primjenu sigurnosnih hardening mjera. U nastavku su navedeni isključivo oni nalazi koji predstavljaju izravne ulazne točke za hardening SSH konfiguracije.

---

### Omogućen TCP forwarding

Lynis je utvrdio da je opcija AllowTcpForwarding omogućena, što je označeno kao slaba konfiguracija SSH servisa:

```bash
Result: Option AllowTcpForwarding found
Result: Option AllowTcpForwarding value is YES
Result: OpenSSH option AllowTcpForwarding is in a weak configuration state and should be fixed
Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:AllowTcpForwarding (set YES to NO)]
```

Omogućeni TCP forwarding povećava napadnu površinu SSH servisa i predstavlja sigurnosni rizik u poslužiteljskim okruženjima.

---

### Omogućena SSH kompresija

Audit je pokazao da je SSH kompresija omogućena, što Lynis označava kao slabu konfiguraciju:

```bash
Result: OpenSSH option Compression is in a weak configuration state and should be fixed
Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:Compression (set YES to NO)]
```

Omogućena kompresija može dovesti do sigurnosnih problema i preporučuje se njezino onemogućavanje.

---

### Korištenje zadanog SSH porta

Lynis je identificirao da SSH servis koristi zadani port 22, što je označeno kao konfiguracija koju je preporučljivo dodatno utvrditi:

```bash
Result: Option Port found
Result: Option Port value is 22
Result: OpenSSH option Port is in a weak configuration state and should be fixed
Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:Port (set 22 to <other>)]
```


Korištenje zadanog porta olakšava automatizirane napade i skeniranja.

---

### Neograničen pristup korisnika putem SSH-a

Audit je pokazao da nisu definirana ograničenja korisnika ili grupa koje se smiju prijavljivati putem SSH-a:

```bash
Performing test ID SSH-7440 (Check OpenSSH option: AllowUsers and AllowGroups)
Result: AllowUsers is not set
Result: AllowGroups is not set
Result: SSH has no specific user or group limitation. Most likely all valid users can SSH to this machine.
```


Izostanak ograničenja korisnika povećava rizik od neautoriziranog pristupa sustavu.

---



### Dozvoljeno agent forwarding prosljeđivanje

Lynis je utvrdio da je opcija AllowAgentForwarding omogućena, što je označeno kao slaba konfiguracija:

```bash
Result: Option AllowAgentForwarding found
Result: Option AllowAgentForwarding value is YES
Result: OpenSSH option AllowAgentForwarding is in a weak configuration state and should be fixed
Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:AllowAgentForwarding (set YES to NO)]
```


Omogućeni agent forwarding može dovesti do kompromitacije SSH ključeva u slučaju sigurnosnog incidenta.

---

### Dozvoljeno X11 forwarding prosljeđivanje

Audit je identificirao da je X11 forwarding omogućen, što Lynis označava kao konfiguraciju koju je potrebno dodatno utvrditi:


```bash
Suggestion: Consider hardening SSH configuration [test:SSH-7408] [details:X11Forwarding (set YES to NO)]
```


X11 forwarding nije potreban u poslužiteljskim okruženjima i povećava napadnu površinu SSH servisa.

---

### Neodgovarajuće dozvole datoteke sshd_config

Lynis je identificirao da dozvole konfiguracijske datoteke SSH servisa nisu postavljene na preporučenu razinu:

```bash
Test: checking if file /etc/ssh/sshd_config has the permissions set to 600 or more restrictive
Outcome: permissions of file /etc/ssh/sshd_config are not matching expected value (644 != rw-------)
```

Neodgovarajuće dozvole konfiguracijske datoteke predstavljaju sigurnosni rizik jer omogućuju neautorizirani uvid u konfiguraciju SSH servisa.

---

### Sažetak nalaza relevantnih za SSH hardening

Rezultati inicijalnog audita ukazuju da je SSH servis funkcionalan, ali konfiguriran s više postavki koje povećavaju sigurnosni rizik u poslužiteljskom okruženju. Identificirani nedostaci odnose se na omogućeni forwarding mehanizam, korištenje zadanog porta, izostanak ograničenja korisnika, omogućenu kompresiju te neodgovarajuće dozvole konfiguracijske datoteke. Navedeni nalazi predstavljaju temelj za primjenu sigurnosnih hardening mjera u sljedećoj fazi rada.


### 1.2.3. Autentikacija korisnika i politika lozinki

Inicijalni sigurnosni audit alatom Lynis identificirao je više nedostataka u području autentikacije korisnika, politike lozinki i PAM konfiguracije koji zahtijevaju primjenu sigurnosnih hardening mjera. U nastavku su navedeni isključivo oni nalazi koji izravno predstavljaju ulazne točke za hardening ove sigurnosne domene.

---

### Neodgovarajuće postavke hashiranja lozinki

Lynis je utvrdio da su korištene zadane metode hashiranja lozinki bez dodatnog pojačanja sigurnosti kroz povećani broj iteracija:

```bash
Performing test ID AUTH-9229 (Check password hashing methods)
Result: poor password hashing methods found: sha256crypt/sha512crypt (default=5000 rounds)
Suggestion: Check PAM configuration, add rounds if applicable and expire passwords to encrypt with new values [test:AUTH-9229]
```

Dodatno je utvrđeno da broj iteracija (rounds) za hashiranje lozinki nije eksplicitno konfiguriran:

```bash
Performing test ID AUTH-9230 (Check password hashing rounds)
Result: number of password hashing rounds is not configured
Suggestion: Configure password hashing rounds in /etc/login.defs [test:AUTH-9230]
```

Navedeni nalazi ukazuju na potrebu jačanja zaštite pohranjenih lozinki konfiguracijom sigurnijih parametara hashiranja.

---

### Neaktivna politika starosti lozinki

Audit je pokazao da minimalna i maksimalna starost lozinki nije definirana u sustavu:

```bash
Performing test ID AUTH-9286 (Checking user password aging)
Result: password minimum age is not configured
Suggestion: Configure minimum password age in /etc/login.defs [test:AUTH-9286]
```

```bash
Result: password aging limits are not configured
Suggestion: Configure maximum password age in /etc/login.defs [test:AUTH-9286]
```

Izostanak ovih postavki omogućuje neograničeno korištenje iste lozinke, što predstavlja sigurnosni rizik.

---

### Izostanak PAM modula za provjeru jačine lozinki

Lynis je identificirao da na sustavu nisu instalirani PAM moduli za provjeru jačine lozinki:

```bash
Performing test ID AUTH-9262 (Checking presence password strength testing tools)
Result: pam_cracklib.so NOT found
Result: pam_passwdqc.so NOT found
Result: pam_pwquality.so NOT found
Result: no PAM modules for password strength testing found
Suggestion: Install a PAM module for password strength testing like pam_cracklib or pam_passwdqc [test:AUTH-9262]
```

Ovaj nalaz ukazuje da sustav ne provodi tehničku kontrolu kompleksnosti korisničkih lozinki.

---

### Računi bez definiranog datuma isteka

Audit je identificirao korisničke račune bez postavljenog datuma isteka:


```bash
Performing test ID AUTH-9282 (Checking password protected account without expire date)
Result: found one or more accounts without expire date set
Account without expire date: luka
Suggestion: When possible set expire dates for all password protected accounts [test:AUTH-9282]
```


Izostanak kontrole životnog ciklusa korisničkih računa predstavlja dodatni sigurnosni rizik, osobito u dugotrajnim sustavima.

---

### Nedovoljno restriktivan zadani umask

Lynis je identificirao da je zadani umask postavljen na manje restriktivnu vrijednost od preporučene:

```bash
Suggestion: Default umask in /etc/login.defs could be more strict like 027 [test:AUTH-9288]
```

Ova postavka utječe na dozvole novo-stvorenih datoteka i direktorija te može dovesti do preširokog pristupa resursima sustava.

---

### Sažetak nalaza relevantnih za hardening autentikacije

Rezultati inicijalnog audita ukazuju da u *baseline* stanju sustava nisu implementirane preporučene sigurnosne politike za upravljanje lozinkama i autentikacijom korisnika. Identificirani nedostaci odnose se na slabe postavke hashiranja lozinki, izostanak kontrole starosti lozinki, nepostojanje PAM modula za provjeru jačine lozinki, nepostojanje datuma isteka korisničkih računa te nedovoljno restriktivan zadani umask. Navedeni nalazi predstavljaju temelj za primjenu sigurnosnih hardening mjera u sljedećoj fazi rada.

### 1.2.4. Sigurnost procesa podizanja sustava (GRUB)

Inicijalni sigurnosni audit alatom Lynis identificirao je nedostatak u konfiguraciji bootloadera koji zahtijeva primjenu sigurnosnih hardening mjera. U nastavku je naveden isključivo nalaz koji izravno predstavlja ulaznu točku za hardening sigurnosti procesa podizanja sustava.

### Izostanak lozinke za GRUB bootloader

Lynis je utvrdio da u GRUB konfiguraciji nije definirana lozinka za zaštitu procesa podizanja sustava:

```bash
Result: Didn't find hashed password line in GRUB configuration
Suggestion: Set a password on GRUB boot loader to prevent altering boot configuration (e.g. boot in single user mode without password) [test:BOOT-5122]
```

Izostanak lozinke za GRUB omogućuje neautorizirano mijenjanje parametara podizanja sustava, uključujući pokretanje sustava u single-user ili rescue načinu rada bez autentikacije. Ovaj nalaz predstavlja sigurnosni rizik te zahtijeva primjenu hardening mjera u skladu s preporukama sigurnosnih smjernica.

### Sažetak nalaza relevantnih za hardening boot procesa

Rezultati inicijalnog audita ukazuju da u *baseline* stanju sustava nije implementirana zaštita GRUB bootloadera lozinkom. Navedeni nedostatak predstavlja izravnu ulaznu točku za hardening sigurnosti procesa podizanja sustava i bit će adresiran u sljedećoj fazi rada.


### 1.2.5. Logging i auditing

U sklopu inicijalnog sigurnosnog audita sustava provedena je analiza mehanizama zapisivanja i nadzora događaja korištenjem alata Lynis. Cilj ove faze bio je utvrditi postojeće stanje sustava, bez primjene dodatnih sigurnosnih mjera.

### Postojeće stanje lokalnog zapisivanja događaja

Rezultati audita pokazuju da sustav ima aktivnu osnovnu infrastrukturu za lokalno zapisivanje događaja:

```
Found a logging daemon
IsRunning: process 'rsyslogd' found
IsRunning: process 'systemd-journald' found
```

Također je potvrđeno postojanje konfiguracije za rotaciju log zapisa:

```
/etc/logrotate.conf found
/etc/logrotate.d found
```

Ovi nalazi potvrđuju da lokalno zapisivanje događaja postoji i funkcionalno je, ali bez dodatnih sigurnosnih mehanizama za zaštitu integriteta i dostupnosti logova.

---

### Neaktivno udaljeno zapisivanje događaja

Analizom konfiguracijskih datoteka `rsyslog` servisa utvrđeno je da udaljeno zapisivanje događaja nije konfigurirano:

```
Result: no remote target found
Result: no remote logging found
```

Svi zapisi se pohranjuju isključivo lokalno na sustavu. U slučaju kompromitacije sustava, napadač bi imao mogućnost brisanja ili izmjene log zapisa, čime bi se onemogućila pouzdana forenzička analiza.

Ovaj nalaz identificiran je kao područje koje zahtijeva dodatno utvrđivanje sigurnosti.

---

### Neaktivan audit podsustav (auditd)

Provjerom stanja audit mehanizma utvrđeno je da servis `auditd` nije aktivan:

```
IsRunning: process 'auditd' not found
Result: auditd not active
```

Zbog nepostojanja aktivnog audit servisa, sustav ne prikuplja detaljne zapise o sigurnosno relevantnim događajima poput promjena konfiguracijskih datoteka, administrativnih aktivnosti ili pokušaja neovlaštenog pristupa.

Ovaj nedostatak predstavlja značajan sigurnosni rizik te je jasno identificiran kao kandidat za hardening.

---

### Sažetak identificiranih područja za hardening

Na temelju provedenog baseline audita, u području zapisivanja i nadzora događaja identificirana su sljedeća područja koja zahtijevaju hardening:

- auditd servis nije aktivan
- audit pravila nisu definirana
- udaljeno zapisivanje logova nije konfigurirano

Ovi nalazi poslužit će kao temelj za implementaciju hardening mjera u skladu s CIS Ubuntu Linux 22.04 LTS Benchmarkom, poglavlje 4.2 Logging and Auditing.

