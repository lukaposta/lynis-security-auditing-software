# Pregled područja i postojećih radova/rješenja

U ovoj cjelini daje se pregled teorijskih i praktičnih aspekata sigurnosti Linux poslužiteljskih sustava. Prikazuju se temeljni pojmovi vezani uz sigurnosni audit i hardening, relevantni alati i standardi te postojeća istraživanja iz tog područja. Cilj pregleda područja je smjestiti ovaj projekt u širi kontekst postojećih praksi i istraživanja te opravdati odabrani pristup i korištene alate.

## 2.1 Sigurnost Linux poslužiteljskih sustava

Linux poslužiteljski sustavi čine temelj velikog broja informacijskih sustava, uključujući web poslužitelje, baze podataka, cloud infrastrukturu i mrežne servise. Njihova široka primjena proizlazi iz otvorenog koda, stabilnosti, fleksibilnosti i mogućnosti prilagodbe različitim okruženjima. Upravo zbog takve rasprostranjenosti Linux poslužitelji predstavljaju čestu metu sigurnosnih napada.

Sigurnost Linux sustava ne ovisi isključivo o samom operacijskom sustavu, već ponajprije o načinu njegove konfiguracije i održavanja. Zadane postavke često nisu prilagođene sigurnosno osjetljivim okruženjima te mogu uključivati nepotrebne servise, otvorene mrežne portove ili slabo definirane kontrole pristupa. Takve konfiguracije povećavaju napadnu površinu sustava i olakšavaju iskorištavanje potencijalnih ranjivosti.

U poslužiteljskim okruženjima dodatni rizik predstavlja činjenica da su sustavi često dostupni putem mreže, ponekad i izravno s interneta. Time su izloženi različitim vrstama prijetnji, uključujući neovlaštene pokušaje pristupa, zloupotrebu servisa, eskalaciju privilegija i kompromitaciju podataka. Zbog toga sigurnost Linux poslužitelja zahtijeva sustavan i planski pristup.

Smjernice iz dokumenta NIST Special Publication 800-123 naglašavaju da sigurnost Linux sustava ne smije biti promatrana kao statično stanje, već kao proces koji se razvija tijekom životnog ciklusa sustava. Početna konfiguracija predstavlja tek polazišnu točku, dok se stvarna sigurnost postiže kroz stalnu procjenu i prilagodbu postavki u skladu s promjenama okruženja i prijetnji.

Zbog toga je u poslužiteljskim okruženjima nužno periodično provjeravati sigurnosno stanje sustava te procjenjivati učinkovitost postojećih zaštitnih mjera. Takav pristup omogućuje pravovremeno uočavanje potencijalnih slabosti i postavlja temelje za strukturirane sigurnosne procese, poput audita i hardeninga, koji se detaljnije razmatraju u nastavku.

## 2.2 Sigurnosni audit i hardening Linux sustava

Sigurnosni audit predstavlja sustavan postupak procjene konfiguracije i stanja informacijskog sustava s ciljem identifikacije sigurnosnih slabosti, pogrešnih postavki i potencijalnih rizika. U kontekstu Linux poslužiteljskih sustava, audit obuhvaća provjeru operacijskog sustava, aktivnih servisa, korisničkih računa, mrežnih postavki, mehanizama autentikacije, kontrole pristupa i sustava zapisivanja događaja. Njegova svrha nije samo utvrditi trenutno stanje sustava, već i pružiti osnovu za donošenje odluka o daljnjim sigurnosnim mjerama.

Prema smjernicama iz dokumenta NIST Special Publication 800-123, sigurnosni audit predstavlja ključnu komponentu procesa osiguravanja Linux sustava. NIST naglašava da se audit treba provoditi redovito, jer se sigurnosni rizici mijenjaju s vremenom, razvojem sustava i pojavom novih prijetnji. Audit omogućuje administratorima da steknu uvid u stvarnu sigurnosnu razinu sustava, umjesto da se oslanjaju na pretpostavke o sigurnosti zadane konfiguracije.

Hardening Linux sustava označava proces smanjenja napadne površine sustava primjenom sigurnosnih mjera koje ograničavaju mogućnosti zloupotrebe. To uključuje uklanjanje ili onemogućavanje nepotrebnih servisa, strožu kontrolu korisničkih privilegija, sigurnu konfiguraciju mrežnih postavki, pravilno upravljanje zapisima sustava te redovito ažuriranje softvera. Cilj hardeninga nije postizanje apsolutne sigurnosti, već smanjenje vjerojatnosti uspješnog napada i ublažavanje mogućih posljedica.

NIST ističe da sigurnosni audit i hardening nisu odvojeni procesi, već međusobno povezane aktivnosti. Audit prethodi hardeningu jer omogućuje identifikaciju slabosti koje je potrebno adresirati, dok se hardening mjere nakon provedbe ponovno provjeravaju auditom kako bi se procijenio njihov učinak. Takav iterativni pristup omogućuje kontinuirano poboljšavanje sigurnosne konfiguracije sustava.

Važno je naglasiti da hardening Linux sustava nije jednokratna aktivnost provedena nakon instalacije operacijskog sustava. Promjene u softverskom okruženju, instalacija novih servisa i razvoj novih napadačkih tehnika zahtijevaju ponovnu procjenu sigurnosnog stanja. Zbog toga NIST preporučuje kontinuirani ciklus koji uključuje audit, primjenu sigurnosnih mjera, nadzor sustava i periodičnu ponovnu evaluaciju.

U literaturi se sigurnosni audit i hardening Linux sustava često navode kao temeljni preduvjeti za sigurno poslužiteljsko okruženje, neovisno o veličini ili namjeni sustava. Takav pristup omogućuje strukturirano upravljanje sigurnošću i smanjuje ovisnost o ad-hoc konfiguracijama, čime se povećava pouzdanost i otpornost sustava na sigurnosne incidente.

## 2.3 Alati za sigurnosni audit Linux sustava

Sigurnosni audit Linux sustava može se provoditi ručno ili uz pomoć specijaliziranih alata koji automatiziraju i strukturiraju postupak provjere konfiguracije. S obzirom na složenost suvremenih poslužiteljskih okruženja i velik broj sigurnosnih postavki koje je potrebno provjeriti, u praksi se često koriste alati za audit kako bi se osigurala dosljednost, ponovljivost i preglednost rezultata. Takvi alati ne zamjenjuju ulogu administratora, već služe kao potpora u identifikaciji sigurnosnih slabosti i planiranju daljnjih mjera.

### 2.3.1 Lynis

Prema službenoj dokumentaciji projekta, Lynis je open-source alat namijenjen sigurnosnom auditu UNIX i Linux sustava. Alat je razvijen s ciljem pružanja cjelovite analize sigurnosne konfiguracije operacijskog sustava te prepoznavanja potencijalnih slabosti koje mogu utjecati na sigurnost poslužitelja. Lynis se pokreće lokalno na sustavu koji se analizira i ne zahtijeva dodatnu infrastrukturu ili vanjske servise.

Lynis provodi niz provjera koje obuhvaćaju različite aspekte sustava, uključujući konfiguraciju operacijskog sustava, aktivne servise, upravljanje korisničkim računima, mehanizme autentikacije, mrežne postavke, zapisivanje događaja i opće sigurnosne postavke. Rezultati audita prezentiraju se u obliku upozorenja i preporuka, uz procjenu ukupne sigurnosne razine sustava.

Jedna od ključnih značajki Lynisa jest njegov pristup koji ne uključuje automatsku primjenu promjena. Umjesto toga, alat pruža informacije i smjernice na temelju kojih administrator samostalno donosi odluke o primjeni sigurnosnih mjera. Takav pristup omogućuje selektivnu primjenu preporuka i smanjuje rizik od negativnog utjecaja na funkcionalnost sustava.

Lynis je javno dostupan kao open-source projekt i aktivno se održava, što omogućuje transparentnost njegova rada te prilagodbu različitim okruženjima i potrebama korisnika. Zbog jednostavne uporabe, jasne strukture izvještaja i fleksibilnog pristupa, Lynis se često koristi u manjim i srednjim okruženjima, kao i u edukativne i analitičke svrhe, gdje je naglasak na razumijevanju sigurnosnih nalaza, a ne isključivo na automatiziranoj usklađenosti sa standardima.

### 2.3.2 Ostali alati za audit i hardening Linux sustava

Osim Lynisa, u praksi i literaturi koriste se i drugi alati za sigurnosni audit i hardening Linux sustava. Takvi alati često su usmjereni na provjeru usklađenosti sustava s unaprijed definiranim sigurnosnim standardima i namijenjeni su visokoj razini automatizacije, osobito u većim i složenijim okruženjima.

Alati poput CIS-CAT-a, OpenSCAP-a i Chef InSpec-a omogućuju sustavnu provjeru sigurnosnih postavki na temelju formaliziranih pravila i profila. Njihova primjena često uključuje automatizirano izvještavanje i integraciju s postojećim sustavima za upravljanje infrastrukturom. Takav pristup olakšava upravljanje sigurnošću u okruženjima s velikim brojem sustava, ali ujedno zahtijeva dodatnu konfiguraciju i pripremu.

Za razliku od alata koji su primarno usmjereni na automatiziranu provjeru usklađenosti, Lynis nudi fleksibilniji i interpretativni pristup auditu. Razlike između ovih alata ne predstavljaju inherentnu prednost ili nedostatak, već ovise o ciljevima analize, veličini okruženja i razini potrebne automatizacije.

U literaturi se često ističe da odabir alata za sigurnosni audit treba biti prilagođen konkretnom kontekstu primjene i namjeni analize. Dok automatizirani alati omogućuju brzu i dosljednu provjeru velikog broja sustava, alati poput Lynisa pružaju veću preglednost i kontrolu nad procesom hardeninga, što ih čini prikladnima za analitičke i edukativne projekte.

## 2.4 CIS Benchmarks i hardening za Ubuntu Server 22.04 LTS

CIS Benchmarks predstavljaju skup preskriptivnih preporuka za sigurnu konfiguraciju sustava. Fokus im je na tehničkim postavkama koje povećavaju sigurnost, a koriste se zajedno s praksama kao što su redovito zakrpavanje, zapisivanje i nadzor događaja te zaštita krajnjih točaka.

U ovom projektu CIS Ubuntu Linux 22.04 LTS Benchmark (v3.0.0) koristi se kao referentni standard za hardening odabrane Linux server instalacije. Benchmark je razvijen i testiran za Ubuntu 22.04 LTS na x86_64 platformi te je namijenjen administratorima i sigurnosnim ulogama koje provode procjenu i osiguravanje sustava.

### 2.4.1 Struktura i profili (Level 1 i Level 2)

Benchmark definira profile koji pomažu pri odabiru razine strogoće hardeninga u skladu s kontekstom primjene sustava.  
Level 1 (Server) obuhvaća praktične i razborite mjere koje donose jasan sigurnosni dobitak bez neprihvatljivog utjecaja na funkcionalnost sustava.  
Level 2 (Server) nadograđuje Level 1 i uključuje dodatne sigurnosne mjere namijenjene okruženjima u kojima je sigurnost prioritet, često kao dio višeslojne obrane, uz moguć negativan utjecaj na funkcionalnost ili performanse.

Ova podjela je značajna jer omogućuje selektivan pristup hardeningu i opravdava činjenicu da se u ovom projektu ne primjenjuju sve dostupne preporuke.

### 2.4.2 Struktura preporuka i način provjere

Svaka CIS preporuka sastoji se od opisa sigurnosne postavke, obrazloženja njezine svrhe, procjene mogućeg utjecaja na sustav te postupka provjere i provedbe. Benchmark razlikuje preporuke koje je moguće automatizirano provjeriti od onih koje zahtijevaju ručnu provjeru i kontekstualnu procjenu administratora.

Naglašava se i važnost korištenja odgovarajuće verzije benchmarka za ciljani operacijski sustav, jer primjena pogrešne verzije može dovesti do netočnih ili nepouzdanih rezultata sigurnosne procjene.

### 2.4.3 Područja hardeninga koja CIS pokriva na Ubuntu 22.04

CIS Benchmark za Ubuntu Linux 22.04 LTS organiziran je u tematske cjeline koje pokrivaju tipične sigurnosne domene poslužiteljskog sustava. Ta podjela omogućuje sustavan pristup hardeningu i jasno razgraničenje područja sigurnosne konfiguracije. Primjeri takvih cjelina uključuju:

- Initial Setup, uključujući konfiguraciju datotečnog sustava i particioniranje, primjerice korištenje zasebnih particija i odgovarajućih mount opcija za direktorije poput /tmp, /var, /var/log i /var/log/audit.
- Mandatory Access Control, kroz primjenu AppArmor mehanizama, uključujući instalaciju, omogućavanje i korištenje enforcing profila.
- Services, kroz onemogućavanje nepotrebnih servisa i ograničavanje onih koji slušaju na mrežnim sučeljima.
- Network konfiguraciju, uključujući provjeru IPv6 statusa, upravljanje mrežnim kernel modulima te postavljanje sysctl parametara za IPv4.
- Access Control, s naglaskom na hardening SSH servisa i kontrolu privilegija korištenjem sudo mehanizama.
- Logging i auditing, uključujući konfiguraciju auditd pravila i zaštitu audit konfiguracijskih datoteka.

Ovakva struktura benchmarka izravno je korisna za mapiranje nalaza sigurnosnog audita na odgovarajuće CIS preporuke u praktičnom dijelu projekta.


### 2.4.4 Procjena usklađenosti i alati

Benchmark podržava ručnu provjeru i ručnu provedbu preporuka, ali ujedno predviđa korištenje alata za procjenu usklađenosti u složenijim okruženjima. Naglašava se da se u sigurnosnim provjerama trebaju uzeti u obzir i automatizirane i ručne preporuke, a ne isključivo one koje je moguće jednostavno provjeriti.

U okviru ovog projekta CIS Benchmarks služe kao referentni okvir za opravdanje odabranih hardening mjera, dok se konkretna procjena stanja sustava i mjerenje poboljšanja sigurnosti provodi pomoću alata za sigurnosni audit. Takav pristup u skladu je sa smjernicama NIST-a koje sigurnosne standarde promatraju kao pomoć pri donošenju odluka, a ne kao rigidne kontrolne liste.

## 2.5 Pregled srodnih radova

U ovoj cjelini prikazuju se odabrani znanstveni i stručni radovi koji se bave sigurnosnim auditom i hardeningom Linux sustava. Radovi su grupirani prema pristupu i cilju istraživanja, kako bi se jasno istaknule razlike između auditno orijentiranih radova, eksperimentalnih istraživanja s hardeningom, automatiziranih rješenja, specijaliziranih proširenja alata te preglednih teorijskih radova. Takva podjela omogućuje jasnije pozicioniranje ovog projekta u odnosu na postojeća rješenja i istraživanja.

### 2.5.1 Radovi usmjereni na sigurnosni audit Linux sustava

Sedano i Salman (2021) provode sigurnosni audit Linux operacijskog sustava korištenjem CIS Benchmarks standarda uz pomoć alata Chef InSpec. Rad se fokusira na procjenu inicijalnog sigurnosnog stanja svježe instaliranog Ubuntu Server sustava, pri čemu se kvantitativno prikazuje razina usklađenosti s CIS preporukama. Istraživanje ne uključuje provedbu hardening mjera niti ponovnu evaluaciju sustava, već naglašava slabosti zadane konfiguracije i ograničenja automatizirane provjere.

Sholihin i Salman (2025) predstavljaju OSCAT, automatizirani alat za audit konfiguracije poslužiteljskih sustava temeljen na CIS Benchmarks standardima. Rad se isključivo bavi auditom i compliance provjerom, bez primjene hardening mjera. Poseban naglasak stavljen je na usporedbu automatiziranog i ručnog audita te na učinkovitost i točnost automatiziranog pristupa u enterprise okruženju.

### 2.5.2 Radovi koji kombiniraju audit i hardening

Disi (2022) provodi eksperimentalno istraživanje koje obuhvaća sigurnosni audit Linux sustava, primjenu hardening mjera i ponovnu evaluaciju sigurnosnog stanja. Rad koristi CIS Benchmarks i druge sigurnosne standarde kao referentni okvir te kvantitativno prikazuje poboljšanje sigurnosti kroz compliance score prije i nakon hardeninga. Istraživanje obuhvaća više distribucija i alata te naglašava razliku između automatiziranih i ručnih pristupa remedijaciji.

### 2.5.3 Automatizirani alati i okviri za sigurnosni audit i hardening

Akporhuarho (2025) razvija automatizirano rješenje za hardening Linux sustava namijenjeno razvojnim okruženjima i malim poduzećima. Rad kombinira audit i hardening kroz automatizirani tijek rada temeljen na OpenSCAP-u i CIS profilima. Naglasak je na pojednostavljivanju sigurnosnog hardeninga i smanjenju potrebe za ručnim intervencijama, uz kvantitativnu usporedbu sigurnosnog stanja prije i nakon primjene mjera.

### 2.5.4 Specijalizirani pristupi i proširenja audit alata

Ansong, Affum i Donkor (2025) predstavljaju PriviLynis, proširenje alata Lynis usmjereno na detekciju i ublažavanje privilege escalation ranjivosti. Rad se ne bavi općim hardeningom sustava, već specijaliziranom analizom lokalnih ranjivosti temeljenih na CVE zapisima. Istraživanje pokazuje da je moguće nadograditi postojeći audit alat dodatnim mehanizmima za specifične sigurnosne domene, uz minimalan utjecaj na performanse sustava.

### 2.5.5 Pregledni i teorijski radovi o Linux hardeningu

Aravindan (2025) donosi pregledni rad koji sintetizira postojeće tehnike hardeninga Linux sustava u enterprise okruženjima. Rad obuhvaća sigurnosne standarde, alate i koncepte te raspravlja o izazovima primjene hardening mjera, uključujući balans između sigurnosti i upotrebljivosti te zahtjeve usklađenosti s regulatornim okvirima. Istraživanje nema eksperimentalni dio niti mjerljive rezultate, već pruža teorijsku i kontekstualnu podlogu za praktične pristupe sigurnosti Linux sustava.

### 2.5.6 Pozicioniranje ovog projekta u odnosu na postojeće radove

U odnosu na prikazane radove, ovaj projekt zauzima srednju poziciju između teorijskih pregleda i složenih automatiziranih rješenja. Za razliku od radova koji se fokusiraju isključivo na audit ili isključivo na automatizirani hardening, projekt kombinira sigurnosni audit i selektivnu primjenu hardening mjera na jednom operacijskom sustavu. CIS Benchmarks koriste se kao referentni standard, dok se konkretna procjena sigurnosnog stanja i mjerenje poboljšanja provodi pomoću alata za sigurnosni audit. Time se projekt uklapa u postojeću praksu, ali zadržava jasan fokus na edukativnom i analitičkom pristupu sigurnosti Linux poslužiteljskog sustava.
