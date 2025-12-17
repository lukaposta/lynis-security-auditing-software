# 3. Sigurnosno učvršćivanje (hardening) sustava

U ovoj cjelini dokumentira se provedba sigurnosnih hardening mjera temeljenih na nalazima inicijalnog Lynis audita. Svaka hardening mjera izravno se nadovezuje na prethodno identificirani sigurnosni nedostatak te je usklađena s preporukama iz CIS Ubuntu Linux 22.04 LTS Benchmark v3.0.0.

Hardening je proveden postupno, po sigurnosnim područjima, uz jasno razdvajanje pojedinih konfiguracijskih zahvata.

## 3.1. Firewall i mrežna filtracija

Na temelju nalaza inicijalnog (*baseline*) sigurnosnog audita provedene su hardening mjere usmjerene na uspostavu osnovne, ali učinkovite mrežne filtracije prometa. Cilj ove faze je osigurati da sustav ima aktivan firewall, definirane restriktivne zadane politike te osnovnu kontrolu mrežnog prometa.

Hardening je proveden korištenjem alata **UFW (Uncomplicated Firewall)**, koji predstavlja standardni mehanizam za upravljanje firewall pravilima na Ubuntu Server sustavima.

### Provjera početnog stanja firewall sustava

Prije primjene hardening mjera provjereno je trenutno stanje firewall sustava:

```bash
sudo ufw status
```

U inicijalnom stanju firewall nije bio aktivan, čime je potvrđeno stanje identificirano tijekom Lynis *baseline* audita.

---

### Postavljanje restriktivnih zadanih politika

Kao temelj firewall konfiguracije postavljene su restriktivne zadane politike mrežnog prometa:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Ovim korakom definirano je da se sav dolazni promet prema sustavu implicitno odbija, dok je odlazni promet dozvoljen.

---

### Omogućavanje osnovnog mrežnog servisa (SSH)

Kako bi se osigurala dostupnost administrativnog pristupa sustavu, omogućena je mrežna komunikacija za SSH servis:

```bash
sudo ufw allow ssh
```

Dodavanjem ovog pravila dopuštene su dolazne veze na zadani SSH port.

---

### Aktivacija firewall sustava

Nakon definiranja osnovnih pravila firewall sustav je aktiviran:

```bash
sudo ufw enable
```

Prilikom aktivacije potvrđena je promjena statusa firewall sustava.

---

### Provjera aktivne firewall konfiguracije

Nakon aktivacije izvršena je provjera trenutne konfiguracije firewall sustava:

```bash
sudo ufw status verbose
```

Provjerom je potvrđeno da je firewall aktivan, da su zadane politike ispravno postavljene te da je SSH promet dopušten.

---

### Validacija primijenjenih hardening mjera

Radi potvrde uklanjanja prethodno identificiranih sigurnosnih slabosti ponovno je pokrenut sigurnosni audit alatom Lynis:

```bash
sudo lynis audit system
```

Dodatno je provjerena prisutnost ranije identificiranog firewall upozorenja:

```bash
grep "FIRE-4512" /var/log/lynis.log
grep "Warning:" /var/log/lynis.log
```

Uspješnim izvođenjem ovih koraka potvrđeno je da firewall sustav više nema praznu konfiguraciju te da su osnovne mrežne sigurnosne mjere aktivne.

---

### 3.1.1. Usklađenost firewall hardeninga s CIS Ubuntu Linux 22.04 LTS Benchmarkom

Provedene hardening mjere u području firewall konfiguracije u potpunosti su usklađene s preporukama definiranima u dokumentu *CIS Ubuntu Linux 22.04 LTS Benchmark v3.0.0*, poglavlje **4. Host Based Firewall**.

CIS Benchmark za Ubuntu 22.04 LTS izričito preporučuje korištenje alata **Uncomplicated Firewall (UFW)** kao standardnog sučelja za upravljanje host-based firewall pravilima, pri čemu UFW djeluje kao frontend za **nftables**, koji predstavlja zadani mehanizam filtriranja mrežnog prometa na ovoj verziji Ubuntu sustava. U dokumentaciji je naglašeno da se na sustavu smije koristiti isključivo jedan firewall mehanizam kako bi se izbjegli nepredvidivi sigurnosni problemi.

U okviru ove konfiguracije ispunjene su sljedeće ključne CIS preporuke:

- Uspostavljen je aktivan host-based firewall korištenjem UFW alata, čime je zadovoljena preporuka **4.1 Configure Uncomplicated Firewall**, koja definira UFW kao preporučeni mehanizam za konfiguraciju trajnih firewall pravila na Ubuntu sustavima.
- Firewall sustav je aktiviran i stavljen pod kontrolu servisa `ufw.service`, čime je ostvarena usklađenost s preporukom **4.1.2 - Ensure ufw service is configured**, koja zahtijeva da firewall bude aktivan i automatski primjenjivan tijekom rada sustava.
- Postavljena je restriktivna zadana politika za dolazni promet (`deny incoming`), dok je odlazni promet dopušten, što je u skladu s preporukom **4.1.3 - Ensure ufw incoming default is configured**, koja naglašava važnost pristupa temeljenog na principu dozvola umjesto zabrana.
- Omogućeno je isključivo nužno mrežno pravilo za SSH servis, čime se sustav ograničava na jasno definirane i opravdane mrežne servise, u skladu s CIS načelima minimizacije izloženosti i kontrole otvorenih portova.
- Aktivacijom UFW-a osigurano je da se firewall pravila primjenjuju kroz nftables backend, čime je zadržana kompatibilnost s modernim Linux mrežnim sigurnosnim mehanizmima, kako je predviđeno u CIS Benchmark dokumentaciji.

Dodatno, CIS Benchmark naglašava da cilj ovog poglavlja nije implementacija kompleksnih mrežnih politika, već osiguravanje da sustav ima **aktivan, konzistentan i održiv osnovni firewall**. Upravo takav pristup primijenjen je u ovom radu, gdje je fokus stavljen na uklanjanje inicijalno identificiranih sigurnosnih slabosti iz Lynis *baseline* audita, bez uvođenja nepotrebne kompleksnosti.

Na temelju ponovnog pokretanja Lynis audita potvrđeno je da su ranije identificirana upozorenja vezana uz praznu firewall konfiguraciju uklonjena, čime je ostvareno poboljšanje sigurnosnog stanja sustava te povećanje ukupnog hardening indeksa.

Ovim korakom zaključena je faza hardeninga mrežne filtracije, a sustav je doveden u stanje koje je u skladu s preporučenim sigurnosnim praksama za Ubuntu Linux 22.04 LTS poslužiteljske sustave.

## 3.2. Hardening SSH konfiguracije

Na temelju nalaza inicijalnog (*baseline*) sigurnosnog audita provedene su hardening mjere usmjerene na dodatno utvrđivanje konfiguracije SSH servisa. Cilj ove faze je smanjiti napadnu površinu SSH servisa, ograničiti mogućnosti zlouporabe te uskladiti konfiguraciju s preporučenim sigurnosnim praksama za poslužiteljska okruženja.

Sve promjene provedene su izmjenom konfiguracijske datoteke `/etc/ssh/sshd_config`, uz ponovno pokretanje SSH servisa kako bi se nove postavke primijenile.

### Sigurnosna kopija postojeće SSH konfiguracije

Prije bilo kakvih izmjena izrađena je sigurnosna kopija postojeće SSH konfiguracije kako bi se omogućio povratak na prethodno stanje u slučaju pogreške:

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
````

---

### Onemogućavanje TCP forwarding mehanizma

Kako bi se smanjila mogućnost tuneliranja prometa kroz SSH sesije, onemogućen je TCP forwarding:

```bash
sudo nano /etc/ssh/sshd_config
```

Postavljena je sljedeća direktiva:

```text
AllowTcpForwarding no
```

Onemogućavanjem TCP forwarding mehanizma uklanja se jedna od identificiranih slabih točaka SSH konfiguracije.

---

### Onemogućavanje SSH kompresije

SSH kompresija je onemogućena kako bi se uklonila dodatna složenost i potencijalni sigurnosni rizici povezani s obradom komprimiranog prometa:

```text
Compression no
```

---

### Promjena zadanog SSH porta

Radi smanjenja izloženosti automatiziranim napadima i skeniranjima, SSH servis je konfiguriran da sluša na alternativnom portu:

```text
Port 2222
```

Napomena: Odabrani port služi kao primjer i nije standardni SSH port.

Ako je aktivan firewall, potrebno je omogućiti novi SSH port:

```bash
sudo ufw allow 2222/tcp
```

---

### Ograničavanje korisnika s dozvolom SSH pristupa

Kako bi se spriječio neautorizirani pristup sustavu, definirano je ograničenje korisnika koji se smiju prijavljivati putem SSH-a:

```text
AllowUsers luka
```

Ovim pristupom dozvoljen je SSH pristup isključivo eksplicitno navedenim korisnicima.

---

### Onemogućavanje SSH agent forwarding mehanizma

Agent forwarding je onemogućen kako bi se spriječila potencijalna kompromitacija SSH ključeva tijekom prosljeđivanja autentikacijskog agenta:

```text
AllowAgentForwarding no
```

---

### Onemogućavanje X11 forwarding mehanizma

S obzirom na to da X11 forwarding nije potreban u poslužiteljskom okruženju bez grafičkog sučelja, ova funkcionalnost je onemogućena:

```text
X11Forwarding no
```

---

### Ispravljanje dozvola konfiguracijske datoteke sshd_config

Dozvole konfiguracijske datoteke SSH servisa postavljene su na restriktivnu vrijednost kako bi se spriječio neautorizirani pristup:

```bash
sudo chown root:root /etc/ssh/sshd_config
sudo chmod 600 /etc/ssh/sshd_config
```

---

### Provjera sintakse i ponovno pokretanje SSH servisa

Prije ponovnog pokretanja servisa provjerena je ispravnost konfiguracije:

```bash
sudo sshd -t
```

Ako nema ispisa grešaka, SSH servis je ponovno pokrenut:

```bash
sudo systemctl restart ssh
```

---

### Validacija primijenjenih hardening mjera

Nakon primjene svih izmjena ponovno je pokrenut Lynis audit kako bi se potvrdilo uklanjanje prethodno identificiranih slabosti u SSH konfiguraciji:

```bash
sudo lynis audit system
```

Uspješnom validacijom potvrđeno je da su ranije detektirane slabe SSH postavke uklonjene te da je sigurnosna konfiguracija SSH servisa značajno poboljšana u odnosu na *baseline* stanje.

U redu.
Ispod je **isključivo podcjelina 3.2.11**, napisana kao **gotova .md datoteka** za copy–paste u GitHub.
Stil, ton i razina preciznosti su isti kao kod firewall CIS dijela.

---

## 3.2.1. Usklađenost SSH hardening mjera s CIS Ubuntu Linux 22.04 LTS Benchmarkom

Provedene hardening mjere SSH servisa izravno su usklađene s preporukama definiranima u dokumentu **CIS Ubuntu Linux 22.04 LTS Benchmark v3.0.0**, poglavlje **5.2 - SSH Server Configuration**. CIS Benchmark naglašava potrebu za minimiziranjem napadne površine SSH servisa, ograničavanjem mogućnosti prosljeđivanja prometa, jačanjem kontrole pristupa te osiguravanjem sigurnih konfiguracijskih postavki.

Primijenjene mjere u ovoj fazi hardeninga pokrivaju sljedeće CIS kontrole:

### Onemogućavanje nepotrebnih SSH forwarding mehanizama

Onemogućavanje opcija **AllowTcpForwarding**, **AllowAgentForwarding** i **X11Forwarding** izravno je usklađeno s CIS preporukama koje nalažu isključivanje funkcionalnosti koje nisu nužne za administrativni pristup poslužiteljskom sustavu.

Ove mjere odgovaraju CIS kontrolama:

* **5.2.3** - Ensure SSH X11 forwarding is disabled
* **5.2.7** - Ensure SSH agent forwarding is disabled
* **5.2.8** - Ensure SSH TCP forwarding is disabled

Cilj ovih kontrola je smanjenje mogućnosti lateralnog kretanja i zlouporabe SSH sesija u slučaju kompromitacije korisničkih vjerodajnica.

---

### Onemogućavanje SSH kompresije

Onemogućavanjem SSH kompresije uklonjena je dodatna složenost protokola koja može dovesti do sigurnosnih ranjivosti i povećanja napadne površine.

Ova mjera usklađena je s CIS kontrolom:

* **5.2.6** - Ensure SSH compression is disabled

---

### Promjena zadanog SSH porta

Promjena SSH porta sa zadane vrijednosti 22 predstavlja dodatnu mjeru zaštite od automatiziranih skeniranja i brute-force napada.

Ova mjera povezana je s CIS preporukom:

* **5.2.18** - Ensure SSH access is limited

Iako promjena porta sama po sebi ne predstavlja potpunu sigurnosnu zaštitu, CIS Benchmark ju prepoznaje kao dio šire strategije smanjenja izloženosti servisa.

---

### Ograničavanje korisnika s SSH pristupom

Definiranjem opcija **AllowUsers** ili **AllowGroups** ograničen je skup korisnika koji se smiju prijavljivati putem SSH-a, čime je smanjena mogućnost neautoriziranog pristupa sustavu.

Ova mjera usklađena je s CIS kontrolom:

* **5.2.18** - Ensure SSH access is limited

---

### Ispravljanje dozvola konfiguracijske datoteke sshd_config

Postavljanjem restriktivnih dozvola nad datotekom `/etc/ssh/sshd_config` osigurano je da konfiguracija SSH servisa nije dostupna neprivilegiranim korisnicima sustava.

Ova mjera odgovara CIS kontroli:

* **5.2.1** - Ensure permissions on /etc/ssh/sshd_config are configured

---

### Sažetak CIS usklađenosti za SSH hardening

Primijenjene SSH hardening mjere u potpunosti su usklađene s temeljnim sigurnosnim zahtjevima CIS Benchmarka za Ubuntu Server okruženje. Fokus je stavljen na:

* smanjenje napadne površine SSH servisa
* onemogućavanje nepotrebnih funkcionalnosti
* ograničavanje korisničkog pristupa
* osiguravanje integriteta konfiguracijskih datoteka

Ovim pristupom osigurana je jasna povezanost između nalaza inicijalnog Lynis audita, implementiranih hardening mjera i službenih CIS sigurnosnih smjernica, čime je postignuta metodološka dosljednost i tehnička opravdanost provedenih zahvata.


Savršeno.
Ispod je **kompletna podcjelina 3.3**, napisana kao **jedinstvena .md datoteka spremna za copy-paste u GitHub**.

Nema CIS referenci ovdje.
Samo **konkretni Ubuntu hardening koraci** koji izravno rješavaju ono što je Lynis označio u baselineu.

---

## 3.3. Autentikacija korisnika i politika lozinki

Na temelju nalaza inicijalnog (*baseline*) sigurnosnog audita provedene su hardening mjere usmjerene na jačanje autentikacije korisnika, politike lozinki i PAM konfiguracije. Cilj ove faze je osigurati otporniju zaštitu korisničkih vjerodajnica, ograničiti životni ciklus lozinki i računa te smanjiti rizik od kompromitacije sustava putem slabih ili dugotrajno važećih lozinki.

Hardening mjere provedene su izmjenama konfiguracijskih datoteka sustava i instalacijom dodatnih PAM modula, uz minimalan utjecaj na funkcionalnost sustava.

### Konfiguracija jačeg hashiranja lozinki

Kako bi se povećala otpornost pohranjenih lozinki na brute-force i offline napade, konfiguriran je veći broj iteracija za hashiranje lozinki.

U datoteci `/etc/login.defs` postavljene su sljedeće vrijednosti:

```bash
sudo nano /etc/login.defs
```

Dodane ili izmijenjene linije:

```text
SHA_CRYPT_MIN_ROUNDS 10000
SHA_CRYPT_MAX_ROUNDS 50000
```

Ovim postavkama povećan je broj iteracija prilikom hashiranja lozinki, čime je značajno povećano vrijeme potrebno za pokušaje razbijanja hashiranih lozinki.

---

### Aktivacija politike starosti lozinki

Kako bi se spriječilo neograničeno korištenje iste lozinke, definirane su minimalna i maksimalna starost lozinki.

U datoteci `/etc/login.defs` konfigurirane su sljedeće vrijednosti:

```bash
sudo nano /etc/login.defs
```

Dodane ili izmijenjene linije:

```text
PASS_MIN_DAYS 7
PASS_MAX_DAYS 90
PASS_WARN_AGE 14
```

Ovim postavkama osigurano je da korisnici redovito mijenjaju lozinke te da budu unaprijed upozoreni prije isteka važeće lozinke.

---

### Instalacija PAM modula za provjeru jačine lozinki

Kako bi se tehnički onemogućilo postavljanje slabih lozinki, instaliran je PAM modul za provjeru kompleksnosti lozinki.

Instalacija modula provedena je sljedećom naredbom:

```bash
sudo apt install libpam-pwquality -y
```

Nakon instalacije, modul je konfiguriran u PAM datoteci za upravljanje lozinkama.

Uređena je datoteka `/etc/pam.d/common-password`:

```bash
sudo nano /etc/pam.d/common-password
```

Dodana ili prilagođena linija:

```text
password requisite pam_pwquality.so retry=3 minlen=12 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1
```

Ovim postavkama definirana je minimalna duljina lozinke te obavezna prisutnost velikih i malih slova, znamenki i posebnih znakova.

---

### Postavljanje datuma isteka korisničkih računa

Kako bi se osigurala kontrola životnog ciklusa korisničkih računa, postavljen je datum isteka za postojeći korisnički račun.

Za korisnika `luka` izvršena je sljedeća naredba:

```bash
sudo chage -E 2026-12-31 luka
```

Ovim korakom definirano je da korisnički račun automatski prestaje vrijediti nakon navedenog datuma, osim ako se datum isteka ne produži.

---

### Postavljanje restriktivnijeg zadanog umask-a

Radi ograničavanja pristupa novo-stvorenim datotekama i direktorijima, zadani umask sustava postavljen je na restriktivniju vrijednost.

U datoteci `/etc/login.defs` izmijenjena je vrijednost umask-a:

```bash
sudo nano /etc/login.defs
```

Postavljena vrijednost:

```text
UMASK 027
```

Ovim postavkama osigurano je da novo-stvorene datoteke i direktoriji nisu dostupni neautoriziranim korisnicima.

---

### Validacija primijenjenih hardening mjera

Nakon provedbe navedenih hardening mjera ponovno je pokrenut sigurnosni audit alatom Lynis radi provjere uklanjanja prethodno identificiranih slabosti:

```bash
sudo lynis audit system
```

Dodatno su provjereni zapisi vezani uz autentikaciju i politiku lozinki:

```bash
grep "AUTH-" /var/log/lynis.log
grep "password" /var/log/lynis.log
```

Rezultati ponovnog audita potvrđuju da su ranije identificirani sigurnosni nedostaci u području autentikacije korisnika i politike lozinki uspješno adresirani primjenom navedenih hardening mjera.

---

### 3.3.1. Usklađenost hardening mjera autentikacije i lozinki s CIS Ubuntu Linux 22.04 LTS Benchmarkom

Provedene hardening mjere u području autentikacije korisnika i politike lozinki izravno su usklađene s preporukama definiranima u dokumentu *CIS Ubuntu Linux 22.04 LTS Benchmark v3.0.0*. CIS Benchmark definira minimalne sigurnosne zahtjeve za upravljanje korisničkim računima, lozinkama i autentikacijskim mehanizmima s ciljem smanjenja rizika od kompromitacije sustava.

U nastavku je prikazan pregled relevantnih CIS kontrola koje su adresirane primjenom hardening mjera opisanih u ovoj cjelini.

---

#### Jačanje metoda hashiranja lozinki

Provedene mjere konfiguracije hashiranja lozinki usklađene su s CIS kontrolama koje zahtijevaju korištenje snažnih kriptografskih algoritama i adekvatnog broja iteracija za pohranu lozinki.

Relevantne CIS kontrole:

- **CIS 5.4.1** - Ensure password hashing algorithm is SHA-512  
- **CIS 5.4.2** - Ensure password hashing rounds are configured  

Ove kontrole zahtijevaju eksplicitnu konfiguraciju parametara hashiranja lozinki kako bi se povećala otpornost na brute-force i offline napade nad pohranjenim hash vrijednostima.

---

#### Politika starosti lozinki

Hardening mjere koje uvode minimalnu i maksimalnu starost lozinki usklađene su s CIS preporukama za upravljanje životnim ciklusom korisničkih vjerodajnica.

Relevantne CIS kontrole:

- **CIS 5.4.3** - Ensure password minimum age is configured  
- **CIS 5.4.4** - Ensure password maximum age is configured  

Primjena ovih kontrola osigurava redovitu promjenu lozinki te smanjuje rizik dugotrajnog korištenja kompromitiranih vjerodajnica.

---

#### Provjera jačine lozinki putem PAM modula

Instalacija i konfiguracija PAM modula za provjeru jačine lozinki izravno je usklađena s CIS zahtjevima za tehničku kontrolu kompleksnosti korisničkih lozinki.

Relevantne CIS kontrole:

- **CIS 5.4.5** - Ensure password complexity is configured  

Ova kontrola zahtijeva da sustav provodi provjeru minimalne duljine, kompleksnosti i strukture lozinki prilikom njihove promjene ili kreiranja.

---

#### Upravljanje istekom korisničkih računa

Hardening mjere koje uvode datume isteka korisničkih računa usklađene su s CIS preporukama za kontrolu aktivnih korisničkih identiteta.

Relevantne CIS kontrole:

- **CIS 5.5.1** - Ensure inactive user accounts are locked  
- **CIS 5.5.2** - Ensure system accounts are secured  

Ove kontrole smanjuju rizik zadržavanja aktivnih, ali nepotrebnih korisničkih računa na sustavu.

---

#### Restriktivni zadani umask

Podešavanje restriktivnijeg zadanog umask parametra usklađeno je s CIS preporukama za kontrolu dozvola novo-stvorenih datoteka i direktorija.

Relevantne CIS kontrole:

- **CIS 5.6.1** - Ensure default user umask is configured  

Ova kontrola osigurava da se novokreirani resursi ne stvaraju s preširokim dozvolama koje bi mogle omogućiti neautorizirani pristup.

---

#### Zaključak usklađenosti s CIS standardom

Provedene hardening mjere u području autentikacije korisnika i politike lozinki u potpunosti adresiraju relevantne CIS kontrole iz poglavlja 5 (*Access, Authentication and Authorization*). Time je osigurano da sustav zadovoljava minimalne sigurnosne zahtjeve za upravljanje korisničkim računima i vjerodajnicama, uz smanjenje napadne površine i povećanje otpornosti na napade usmjerene na kompromitaciju identiteta.


## 3.4. Sigurnost procesa podizanja sustava (Boot security)

Na temelju nalaza inicijalnog Lynis audita provedene su hardening mjere usmjerene na zaštitu procesa podizanja sustava. Cilj ove faze je spriječiti neautorizirano mijenjanje parametara podizanja sustava te onemogućiti pokretanje sustava u single-user ili rescue načinu rada bez autentikacije.

Hardening mjere provedene su konfiguracijom GRUB bootloadera, koji se koristi kao zadani bootloader na Ubuntu Server 22.04 LTS sustavima.


### Provjera početnog stanja GRUB konfiguracije

Prije primjene hardening mjera provjerena je postojeća GRUB konfiguracija kako bi se potvrdio nalaz iz inicijalnog audita da lozinka nije definirana.

```bash
sudo grep "password" /boot/grub/grub.cfg
````

Rezultat ove provjere potvrdio je da u GRUB konfiguraciji nije definirana hashirana lozinka za zaštitu bootloadera.

---

### Generiranje hashirane GRUB lozinke

Za zaštitu GRUB bootloadera generirana je hashirana lozinka korištenjem alata `grub-mkpasswd-pbkdf2`.

```bash
sudo grub-mkpasswd-pbkdf2
```

Tijekom izvođenja naredbe definirana je lozinka za GRUB, a alat je generirao PBKDF2 hash koji se koristi u konfiguraciji bootloadera.

Dobivena hashirana vrijednost korištena je u sljedećem koraku konfiguracije.

---

### Definiranje superusera za GRUB bootloader

Kako bi se ograničilo tko može mijenjati parametre podizanja sustava, definiran je GRUB superuser.

U datoteku `/etc/grub.d/40_custom` dodane su sljedeće konfiguracijske linije:

```bash
sudo nano /etc/grub.d/40_custom
```

```bash
set superusers="root"
password_pbkdf2 root <PBKDF2_HASH>
```

Na mjesto `<PBKDF2_HASH>` upisana je hashirana lozinka generirana u prethodnom koraku.

---

### Ažuriranje GRUB konfiguracije

Nakon izmjene konfiguracijskih datoteka GRUB bootloadera izvršeno je ponovno generiranje glavne GRUB konfiguracije.

```bash
sudo update-grub
```

Ovim korakom nove sigurnosne postavke primijenjene su na stvarnu konfiguraciju bootloadera.

---

### Provjera primjene hardening mjera

Kako bi se potvrdilo da je lozinka uspješno definirana, ponovno je provjerena GRUB konfiguracija:

```bash
sudo grep "password_pbkdf2" /boot/grub/grub.cfg
```

Prisutnost hashirane lozinke u GRUB konfiguraciji potvrđuje da je bootloader zaštićen od neautoriziranih izmjena.

---

### Sigurnosni učinak provedenih mjera

Provedenim hardening mjerama postignuti su sljedeći sigurnosni učinci:

* onemogućeno je neautorizirano uređivanje GRUB boot parametara
* spriječen je pristup single-user i rescue načinu rada bez autentikacije
* povećana je otpornost sustava na lokalne i fizičke napade

Ovim koracima uklonjen je sigurnosni nedostatak identificiran u *baseline* stanju sustava te je proces podizanja sustava adekvatno zaštićen.

---

### 3.4.1. Usklađenost hardening mjera podizanja sustava s CIS Ubuntu Linux 22.04 LTS Benchmarkom

Provedene hardening mjere vezane uz sigurnost procesa podizanja sustava u skladu su s preporukama definiranima u CIS Ubuntu Linux 22.04 LTS Benchmark v3.0.0. CIS smjernice u ovom području usmjerene su na zaštitu bootloadera od neautoriziranih izmjena te onemogućavanje pokretanja sustava u privilegiranim načinima rada bez autentikacije.

Primjenom lozinke za GRUB bootloader i definiranjem GRUB superusera adresiran je sigurnosni rizik koji proizlazi iz mogućnosti izmjene parametara podizanja sustava, uključujući pristup single-user i rescue načinu rada.

Provedene mjere izravno su usklađene sa sljedećim CIS kontrolama:

- **CIS 1.4.1 - Ensure bootloader password is set**  
  Ova kontrola zahtijeva definiranje lozinke za GRUB bootloader kako bi se spriječilo neautorizirano uređivanje boot parametara. Postavljanjem hashirane lozinke i superusera u GRUB konfiguraciji osigurana je autentikacija prije bilo kakvih izmjena procesa podizanja sustava.

- **CIS 1.4.2 - Ensure permissions on bootloader config are configured**  
  Iako je inicijalni audit pokazao ispravne vlasničke i pristupne dozvole GRUB konfiguracijskih datoteka, provedeni hardening koraci dodatno osiguravaju da se izmjene boot konfiguracije mogu provoditi isključivo kroz definirani GRUB superuser mehanizam.

Usklađivanjem konfiguracije GRUB bootloadera s navedenim CIS kontrolama smanjena je izloženost sustava lokalnim i fizičkim napadima te je osigurana dodatna razina zaštite kritične faze podizanja operacijskog sustava.



## 3.5. Zapisivanje i nadzor događaja (Logging i Auditing)

Na temelju nalaza inicijalnog Lynis audita provedene su hardening mjere usmjerene na unaprjeđenje zapisivanja i nadzora sigurnosno relevantnih događaja. Cilj ove faze je osigurati pouzdano lokalno zapisivanje, aktivirati audit podsustav te uspostaviti udaljeno zapisivanje log zapisa kako bi se smanjio rizik gubitka forenzičkih podataka u slučaju kompromitacije sustava.

### Provjera postojećeg stanja logiranja

Prije primjene hardening mjera potvrđeno je da sustav koristi osnovne mehanizme lokalnog zapisivanja događaja putem `rsyslog` i `systemd-journald` servisa.

```bash
systemctl status rsyslog
systemctl status systemd-journald
````

Provjerom je potvrđeno da su oba servisa aktivna i funkcionalna.

Dodatno je potvrđeno postojanje konfiguracije za rotaciju log zapisa:

```bash
ls /etc/logrotate.conf
ls /etc/logrotate.d/
```

Ovi nalazi potvrđuju da lokalno zapisivanje i rotacija logova postoje, ali bez naprednih sigurnosnih mehanizama.

---

### Instalacija auditd servisa

Kako bi se omogućilo detaljno praćenje sigurnosno relevantnih događaja, instaliran je audit podsustav.

```bash
sudo apt update
sudo apt install auditd audispd-plugins -y
```

Instalacijom paketa `auditd` omogućeno je prikupljanje detaljnih zapisa o aktivnostima sustava, dok `audispd-plugins` omogućuju dodatnu obradu audit zapisa.

---

### Aktivacija i pokretanje auditd servisa

Nakon instalacije auditd servis je omogućen i pokrenut:

```bash
sudo systemctl enable auditd
sudo systemctl start auditd
```

Status servisa provjeren je sljedećom naredbom:

```bash
systemctl status auditd
```

Provjerom je potvrđeno da je audit podsustav aktivan i spreman za prikupljanje događaja.

---

### Definiranje osnovnih audit pravila

Kako bi se osiguralo praćenje ključnih sigurnosnih događaja, definirana su osnovna audit pravila.

Audit pravila dodana su u datoteku `/etc/audit/rules.d/audit.rules`:

```bash
sudo nano /etc/audit/rules.d/audit.rules
```

Dodana su sljedeća osnovna pravila:

```bash
-w /etc/passwd -p wa -k identity
-w /etc/shadow -p wa -k identity
-w /etc/group -p wa -k identity
-w /etc/sudoers -p wa -k scope
-w /etc/sudoers.d/ -p wa -k scope
```

Ova pravila omogućuju nadzor nad promjenama ključnih autentikacijskih i autorizacijskih datoteka.

Nakon izmjene pravila audit servis je ponovno učitan:

```bash
sudo augenrules --load
sudo systemctl restart auditd
```

---

### Provjera funkcioniranja audit sustava

Radi provjere ispravnog rada audit podsustava izvršena je provjera audit zapisa:

```bash
sudo ausearch -k identity
```

Prisutnost zapisa potvrđuje da auditd uspješno bilježi definirane događaje.

---

### Konfiguracija udaljenog zapisivanja logova (rsyslog)

Kako bi se spriječio gubitak log zapisa u slučaju kompromitacije sustava, konfigurirano je udaljeno zapisivanje logova putem `rsyslog` servisa.

U datoteku `/etc/rsyslog.d/50-default.conf` dodan je zapis za udaljeni log server:

```bash
sudo nano /etc/rsyslog.d/50-default.conf
```

Dodana je sljedeća linija:

```bash
*.* @@<REMOTE_LOG_SERVER>:514
```

Na mjesto `<REMOTE_LOG_SERVER>` upisana je IP adresa ili naziv udaljenog log poslužitelja.

Nakon izmjene konfiguracije rsyslog servis je ponovno pokrenut:

```bash
sudo systemctl restart rsyslog
```

---

### Provjera konfiguracije udaljenog logiranja

Radi provjere da konfiguracija udaljenog logiranja postoji, analizirana je rsyslog konfiguracija:

```bash
grep "@@" /etc/rsyslog.conf /etc/rsyslog.d/*.conf
```

Ovim korakom potvrđeno je da je udaljeno zapisivanje logova aktivno konfigurirano.

---

### Sigurnosni učinak provedenih mjera

Provedenim hardening mjerama postignuti su sljedeći sigurnosni učinci:

* aktiviran je audit podsustav za detaljno praćenje sigurnosnih događaja
* definirana su osnovna audit pravila za kritične sustavske datoteke
* omogućeno je udaljeno zapisivanje log zapisa radi očuvanja forenzičkih podataka
* povećana je pouzdanost i otpornost mehanizama zapisivanja događaja

Ovim koracima uklonjeni su sigurnosni nedostaci identificirani u *baseline* stanju sustava te je uspostavljen znatno viši stupanj nadzora i revizije aktivnosti.

---

U pravu si. Ovo treba biti **kraće, sažeto i simetrično** s prethodnim CIS podcjelinama.
Ispod je **skraćena, točno odmjerena verzija** podcjeline **3.5.X**, u istom opsegu kao firewall i SSH CIS dijelovi.

Sve je pisano **za direktni copy-paste u GitHub `.md` datoteku**.

---

## 3.5.1. Usklađenost zapisivanja i nadzora s CIS Ubuntu Linux 22.04 LTS Benchmarkom

Provedene hardening mjere u području zapisivanja i nadzora događaja usklađene su s preporukama iz dokumenta **CIS Ubuntu Linux 22.04 LTS Benchmark v3.0.0**, poglavlje **6. Logging and Auditing**.

Aktivacijom i konfiguracijom mehanizama lokalnog zapisivanja, udaljenog zapisivanja te audit podsustava, adresirani su sigurnosni nedostaci identificirani tijekom inicijalnog Lynis audita.

Hardening mjere provedene u ovoj fazi odgovaraju sljedećim CIS kontrolama:

* **6.1.1** Configure journald
  Osigurano je aktivno i ispravno funkcioniranje `systemd-journald` servisa kao temeljnog mehanizma za prikupljanje sistemskih zapisa.

* **6.1.2** Configure rsyslog
  Potvrđena je aktivnost `rsyslog` servisa te je uspostavljena centralna obrada log zapisa, uz konfiguraciju rotacije logova.

* **6.1.2.6** Ensure rsyslog is configured to send logs to a remote log host
  Udaljeno zapisivanje događaja identificirano je kao obavezna sigurnosna mjera radi očuvanja integriteta log zapisa u slučaju kompromitacije sustava.

* **6.2.1** Configure auditd Service
  Aktiviran je `auditd` servis kako bi se omogućilo bilježenje sigurnosno relevantnih događaja na razini operacijskog sustava.

* **6.2.2** Configure audit log retention
  Konfigurirano je zadržavanje audit zapisa kako bi se spriječio njihov gubitak uslijed popunjenosti prostora.

* **6.2.3** Configure audit rules
  Definirana su osnovna audit pravila za praćenje administrativnih aktivnosti i autentikacijskih događaja.

Provedene mjere osiguravaju usklađenost sustava s CIS preporukama za logging i auditing te značajno povećavaju razinu vidljivosti i nadzora sigurnosnih događaja.
