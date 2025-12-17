# 2. Rezultati i analiza sigurnosnog učvršćivanja (hardeninga)

U ovoj cjelini prikazani su rezultati sigurnosnog audita sustava provedenog nakon implementacije hardening mjera opisanih u implementacijskom dijelu rada. Cilj ove faze je kvantitativno i kvalitativno procijeniti učinak primijenjenih sigurnosnih mjera te usporediti stanje sustava prije i nakon hardeninga.

Post-hardening audit proveden je korištenjem istog alata, istih parametara i nad istim operacijskim okruženjem kao i inicijalni (*baseline*) audit, čime je osigurana usporedivost rezultata.


## 2.1. Post-hardening sigurnosni audit

Nakon provedbe svih planiranih hardening mjera ponovno je pokrenut sigurnosni audit alatom Lynis:

```bash
sudo lynis audit system
```

Audit je proveden nad sustavom Ubuntu Server 22.04.5 LTS u istom virtualizacijskom okruženju, bez dodatnih promjena koje nisu dio dokumentiranog hardening procesa.

Osnovni podaci post-hardening audita:

* Alat: Lynis
* Verzija: 3.0.7
* Način rada: lynis audit system
* Tests performed: 255
* Total tests available: 450

Na temelju provedenog audita izračunan je novi *hardening index* sustava:

**Hardening index: 72 / 100**

Poruka o razini sigurnosnog utvrđivanja sustava:

> System has been hardened, but could use additional hardening

Dobivena vrijednost ukazuje na značajno povećanje razine sigurnosti sustava u odnosu na inicijalno stanje te potvrđuje učinkovitost provedenih hardening mjera.


## 2.2. Usporedba baseline i post-hardening stanja

Usporedba rezultata inicijalnog i post-hardening audita pokazuje jasno poboljšanje sigurnosne konfiguracije sustava.

| Kategorija              | Baseline stanje                     | Post-hardening stanje               |
| ----------------------- | ----------------------------------- | ----------------------------------- |
| Hardening index         | 60                                  | 72                                  |
| Firewall konfiguracija  | Bez aktivnih pravila                | Aktivna restriktivna politika       |
| SSH konfiguracija       | Slabe zadane postavke               | Utvrđena i ograničena konfiguracija |
| Autentikacija i lozinke | Bez politike jačine i starosti      | Aktivne PAM i password politike     |
| Boot sigurnost          | GRUB bez lozinke                    | GRUB zaštićen lozinkom              |
| Logging i auditing      | auditd neaktivan, bez remote logova | Aktiviran auditd i poboljšan nadzor |

Najveće promjene vidljive su u područjima mrežne sigurnosti, autentikacije korisnika i nadzora sigurnosnih događaja, koja su u *baseline* stanju bila identificirana kao kritične sigurnosne slabosti.



## 2.3. Analiza uklonjenih sigurnosnih upozorenja

Post-hardening audit pokazuje da su sigurnosna upozorenja identificirana tijekom inicijalnog audita uklonjena ili svedena na razinu preporuka.

Uklonjena ili adresirana upozorenja uključuju:

* praznu firewall konfiguraciju i ne-restriktivne politike
* slabe SSH postavke povezane s forwardingom i kompresijom
* nepostojanje politike lozinki i PAM kontrole
* neaktivan audit podsustav
* nezaštićen GRUB bootloader

Uklanjanjem navedenih slabosti sustav postiže višu razinu otpornosti na neovlašteni pristup, eskalaciju privilegija i prikrivanje sigurnosnih incidenata.



## 2.4. Analiza učinkovitosti hardening mjera

Analiza rezultata pokazuje da su najveći doprinos povećanju *hardening indexa* imale sljedeće skupine mjera:

* uspostava aktivne firewall politike
* utvrđivanje SSH konfiguracije
* uvođenje politike jačine i starosti lozinki
* aktivacija auditd servisa i nadzora događaja
* zaštita procesa podizanja sustava

Provedene mjere izravno adresiraju nalaze inicijalnog Lynis audita i preporuke sigurnosnih smjernica te predstavljaju uravnotežen i metodološki opravdan pristup sigurnosnom hardeningu poslužiteljskog sustava.



## 2.5. Ograničenja analize

Rezultati sigurnosnog audita odnose se na sustav u kontroliranom virtualizacijskom okruženju i služe kao demonstracija metodologije sigurnosnog hardeninga. U produkcijskim okruženjima bilo bi potrebno dodatno prilagoditi sigurnosne mjere specifičnim zahtjevima i operativnim potrebama.

Unatoč tome, postignuti rezultati jasno potvrđuju da je sustav nakon hardeninga sigurniji, dosljednije konfiguriran i otporniji na uobičajene sigurnosne prijetnje.
