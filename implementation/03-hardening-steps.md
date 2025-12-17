# 3. Sigurnosni hardening sustava

U ovoj cjelini dokumentira se provedba sigurnosnih hardening mjera temeljenih na nalazima inicijalnog Lynis audita. Svaka hardening mjera izravno se nadovezuje na prethodno identificirani sigurnosni nedostatak te je usklađena s preporukama iz CIS Ubuntu Linux 22.04 LTS Benchmark v3.0.0.

Hardening je proveden postupno, po sigurnosnim područjima, uz jasno razdvajanje pojedinih konfiguracijskih zahvata.

## 3.1. Firewall i mrežna filtracija

Na temelju nalaza inicijalnog (*baseline*) sigurnosnog audita provedene su hardening mjere usmjerene na uspostavu osnovne, ali učinkovite mrežne filtracije prometa. Cilj ove faze je osigurati da sustav ima aktivan firewall, definirane restriktivne zadane politike te osnovnu kontrolu mrežnog prometa.

Hardening je proveden korištenjem alata **UFW (Uncomplicated Firewall)**, koji predstavlja standardni mehanizam za upravljanje firewall pravilima na Ubuntu Server sustavima.

### 3.1.1. Provjera početnog stanja firewall sustava

Prije primjene hardening mjera provjereno je trenutno stanje firewall sustava:

```bash
sudo ufw status
```

U inicijalnom stanju firewall nije bio aktivan, čime je potvrđeno stanje identificirano tijekom Lynis *baseline* audita.

---

### 3.1.2. Postavljanje restriktivnih zadanih politika

Kao temelj firewall konfiguracije postavljene su restriktivne zadane politike mrežnog prometa:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Ovim korakom definirano je da se sav dolazni promet prema sustavu implicitno odbija, dok je odlazni promet dozvoljen.

---

### 3.1.3. Omogućavanje osnovnog mrežnog servisa (SSH)

Kako bi se osigurala dostupnost administrativnog pristupa sustavu, omogućena je mrežna komunikacija za SSH servis:

```bash
sudo ufw allow ssh
```

Dodavanjem ovog pravila dopuštene su dolazne veze na zadani SSH port.

---

### 3.1.4. Aktivacija firewall sustava

Nakon definiranja osnovnih pravila firewall sustav je aktiviran:

```bash
sudo ufw enable
```

Prilikom aktivacije potvrđena je promjena statusa firewall sustava.

---

### 3.1.5. Provjera aktivne firewall konfiguracije

Nakon aktivacije izvršena je provjera trenutne konfiguracije firewall sustava:

```bash
sudo ufw status verbose
```

Provjerom je potvrđeno da je firewall aktivan, da su zadane politike ispravno postavljene te da je SSH promet dopušten.

---

### 3.1.6. Validacija primijenjenih hardening mjera

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

### 3.1.7. Usklađenost firewall hardeninga s CIS Ubuntu Linux 22.04 LTS Benchmarkom

Provedene hardening mjere u području firewall konfiguracije u potpunosti su usklađene s preporukama definiranima u dokumentu *CIS Ubuntu Linux 22.04 LTS Benchmark v3.0.0*, poglavlje **4. Host Based Firewall**.

CIS Benchmark za Ubuntu 22.04 LTS izričito preporučuje korištenje alata **Uncomplicated Firewall (UFW)** kao standardnog sučelja za upravljanje host-based firewall pravilima, pri čemu UFW djeluje kao frontend za **nftables**, koji predstavlja zadani mehanizam filtriranja mrežnog prometa na ovoj verziji Ubuntu sustava. U dokumentaciji je naglašeno da se na sustavu smije koristiti isključivo jedan firewall mehanizam kako bi se izbjegli nepredvidivi sigurnosni problemi.

U okviru ove konfiguracije ispunjene su sljedeće ključne CIS preporuke:

- Uspostavljen je aktivan host-based firewall korištenjem UFW alata, čime je zadovoljena preporuka **4.1 Configure Uncomplicated Firewall**, koja definira UFW kao preporučeni mehanizam za konfiguraciju trajnih firewall pravila na Ubuntu sustavima.
- Firewall sustav je aktiviran i stavljen pod kontrolu servisa `ufw.service`, čime je ostvarena usklađenost s preporukom **4.1.2 Ensure ufw service is configured**, koja zahtijeva da firewall bude aktivan i automatski primjenjivan tijekom rada sustava.
- Postavljena je restriktivna zadana politika za dolazni promet (`deny incoming`), dok je odlazni promet dopušten, što je u skladu s preporukom **4.1.3 Ensure ufw incoming default is configured**, koja naglašava važnost pristupa temeljenog na principu dozvola umjesto zabrana.
- Omogućeno je isključivo nužno mrežno pravilo za SSH servis, čime se sustav ograničava na jasno definirane i opravdane mrežne servise, u skladu s CIS načelima minimizacije izloženosti i kontrole otvorenih portova.
- Aktivacijom UFW-a osigurano je da se firewall pravila primjenjuju kroz nftables backend, čime je zadržana kompatibilnost s modernim Linux mrežnim sigurnosnim mehanizmima, kako je predviđeno u CIS Benchmark dokumentaciji.

Dodatno, CIS Benchmark naglašava da cilj ovog poglavlja nije implementacija kompleksnih mrežnih politika, već osiguravanje da sustav ima **aktivan, konzistentan i održiv osnovni firewall**. Upravo takav pristup primijenjen je u ovom radu, gdje je fokus stavljen na uklanjanje inicijalno identificiranih sigurnosnih slabosti iz Lynis *baseline* audita, bez uvođenja nepotrebne kompleksnosti.

Na temelju ponovnog pokretanja Lynis audita potvrđeno je da su ranije identificirana upozorenja vezana uz praznu firewall konfiguraciju uklonjena, čime je ostvareno poboljšanje sigurnosnog stanja sustava te povećanje ukupnog hardening indeksa.

Ovim korakom zaključena je faza hardeninga mrežne filtracije, a sustav je doveden u stanje koje je u skladu s preporučenim sigurnosnim praksama za Ubuntu Linux 22.04 LTS poslužiteljske sustave.


