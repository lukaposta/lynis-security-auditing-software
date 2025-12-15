# 3. Vlastiti praktični dio

Ova cjelina opisuje provedbu vlastitog praktičnog dijela rada, s naglaskom na primjenu sigurnosnog audita i hardening mjera nad Linux poslužiteljskim sustavom. Praktični dio temelji se na stvarnom radnom okruženju i slijedi fazni pristup koji uključuje inicijalni audit, odabir i primjenu hardening mjera te ponovnu evaluaciju sigurnosnog stanja. U ovoj cjelini naglasak je na opisu postupaka i odluka, bez ulaska u tehničke detalje, naredbe ili ispise alata, koji se obrađuju u zasebnim dijelovima projekta.

## 3.1 Opis radnog okruženja

Praktični dio rada proveden je u virtualnom okruženju korištenjem alata Oracle VirtualBox. U sklopu virtualnog okruženja korišten je jedan virtualni stroj koji simulira realističan Linux poslužiteljski sustav, što omogućuje kontrolirane uvjete rada i ponovljivost postupaka.

Na virtualni stroj instaliran je Ubuntu Server 22.04 LTS, bez grafičkog korisničkog sučelja. Odabir serverske verzije operacijskog sustava omogućuje fokus na sigurnosne aspekte tipične za poslužiteljska okruženja, poput rada kroz terminal, upravljanja servisima i konfiguracijskih datoteka. Sav rad na sustavu provodi se isključivo putem komandne linije, što odgovara uobičajenoj praksi administracije Linux poslužitelja.

Virtualno okruženje korišteno je kao izolirana platforma za provođenje sigurnosnog audita i hardening mjera, bez utjecaja na produkcijske sustave. Takav pristup omogućuje sigurno eksperimentiranje s konfiguracijama, jasno praćenje promjena i analizu njihovog utjecaja na sigurnosno stanje sustava.

## 3.2 Korišteni alati i referentni standardi

U provedbi praktičnog dijela rada korištena je kombinacija alata i sigurnosnih standarda koji zajedno omogućuju strukturiran, ponovljiv i metodološki opravdan pristup sigurnosnom auditu i hardeningu Linux poslužiteljskog sustava. Odabrani alati i standardi imaju jasno razgraničene uloge te se ne koriste kao međusobne zamjene, već kao komplementarni elementi istog procesa.

Kao osnovni alat za sigurnosni audit korišten je Lynis. Lynis omogućuje sustavnu analizu sigurnosne konfiguracije Linux sustava kroz niz provjera koje obuhvaćaju operacijski sustav, aktivne servise, mrežne postavke, autentikaciju, kontrolu pristupa i zapisivanje događaja. U ovom radu Lynis se koristi isključivo kao alat za procjenu sigurnosnog stanja sustava i identifikaciju potencijalnih slabosti. Alat ne provodi automatske promjene konfiguracije, već generira preporuke koje služe kao polazište za daljnje sigurnosne odluke.

CIS Benchmarks koriste se kao referentni sigurnosni standard za interpretaciju nalaza sigurnosnog audita. Benchmark za Ubuntu Server 22.04 LTS pruža strukturirane smjernice za sigurnu konfiguraciju sustava, raspoređene po jasno definiranim sigurnosnim domenama. U ovom praktičnom dijelu CIS Benchmarks ne koriste se kao stroga kontrolna lista, već kao okvir za procjenu relevantnosti i prioriteta pojedinih hardening mjera. Time se omogućuje selektivan pristup hardeningu, pri kojem se uzimaju u obzir namjena sustava i potencijalni utjecaj promjena na njegovu funkcionalnost.

Teorijsku i metodološku podlogu praktičnog dijela rada čine smjernice iz dokumenta NIST Special Publication 800-123. NIST pruža opći okvir za sigurnu konfiguraciju i održavanje Linux sustava, s naglaskom na kontinuirani sigurnosni proces koji uključuje audit, primjenu sigurnosnih mjera i ponovnu evaluaciju. U ovom radu NIST smjernice koriste se za opravdanje faznog pristupa praktičnom dijelu, kao i za objašnjenje zašto sigurnosni hardening nije jednokratna aktivnost, već iterativni proces.

Kombinacijom Lynisa, CIS Benchmarks i NIST smjernica ostvaruje se jasan odnos između alata, standarda i metodologije. Lynis služi za identifikaciju sigurnosnih slabosti, CIS Benchmarks za vrednovanje i kontekstualizaciju preporučenih mjera, dok NIST osigurava teorijski okvir koji povezuje audit i hardening u koherentan sigurnosni proces. Takav pristup omogućuje provedbu praktičnog dijela rada na strukturiran i akademski utemeljen način, bez oslanjanja na automatizirane ili nekritički primijenjene konfiguracijske promjene.

### 3.3 Provedba inicijalnog sigurnosnog audita

Nakon uspostave radnog okruženja proveden je inicijalni sigurnosni audit sustava s ciljem utvrđivanja početnog sigurnosnog stanja svježe instaliranog Ubuntu Server sustava. Inicijalni audit predstavlja polaznu točku praktičnog dijela rada i služi za identifikaciju potencijalnih sigurnosnih slabosti prije primjene bilo kakvih hardening mjera.

Sigurnosni audit proveden je korištenjem alata Lynis, koji analizira konfiguraciju operacijskog sustava kroz niz unaprijed definiranih provjera. Audit obuhvaća različite sigurnosne domene, uključujući osnovne postavke sustava, aktivne servise, mrežnu konfiguraciju, autentikaciju korisnika, kontrolu privilegija te zapisivanje i nadzor sigurnosnih događaja. Cilj ove faze nije trenutačna korekcija uočenih problema, već dobivanje objektivnog pregleda sigurnosnog stanja sustava u njegovoj početnoj konfiguraciji.

Rezultati inicijalnog audita koriste se kao temelj za daljnje odluke u praktičnom dijelu rada. Umjesto nekritičkog prihvaćanja svih preporuka, nalazi audita analizirani su kako bi se utvrdilo koje su sigurnosne slabosti relevantne za ciljani poslužiteljski sustav. Na taj način audit ne služi samo kao tehnička provjera konfiguracije, već kao alat za usmjeravanje procesa hardeninga.

Provedba inicijalnog sigurnosnog audita ujedno omogućuje kasniju usporedbu stanja sustava prije i nakon primjene hardening mjera. Takav pristup u skladu je s preporukama sigurnosnih smjernica koje naglašavaju važnost početne procjene kao preduvjeta za smisleno i mjerljivo poboljšanje sigurnosne razine sustava.

## 3.4 Kriteriji za odabir hardening mjera

Odabir hardening mjera u ovom radu temelji se na svjesno selektivnom pristupu, a ne na potpunoj implementaciji svih preporuka koje proizlaze iz sigurnosnog audita. Takav pristup u skladu je s praktičnim smjernicama sigurnosnog hardeninga, koje naglašavaju potrebu prilagodbe sigurnosnih mjera stvarnoj namjeni sustava i kontekstu njegova korištenja.

Primarni kriterij pri odabiru hardening mjera bila je relevantnost preporuke za poslužiteljsko okruženje. Budući da se radi o Ubuntu Server sustavu bez grafičkog sučelja i bez dodatnih aplikacijskih servisa, prioritet su imale mjere koje se odnose na osnovne sigurnosne aspekte operacijskog sustava, poput upravljanja servisima, mrežnih postavki, autentikacije korisnika i zapisivanja sigurnosno relevantnih događaja. Preporuke koje nisu imale izravan utjecaj na sigurnost takvog okruženja nisu bile uključene u praktični dio.

Drugi važan kriterij bila je usklađenost s CIS Benchmarks smjernicama za Ubuntu Server 22.04 LTS. CIS preporuke korištene su kao referentni okvir za procjenu opravdanosti pojedinih hardening mjera, pri čemu se vodilo računa o razini preporuke i njezinoj namjeni. Poseban naglasak stavljen je na mjere koje se nalaze u okviru Level 1 profila, jer one donose značajan sigurnosni dobitak uz minimalan rizik za stabilnost i funkcionalnost sustava.

Treći kriterij odnosio se na potencijalni utjecaj hardening mjera na funkcionalnost i upravljivost sustava. Budući da je cilj praktičnog dijela bio demonstrirati poboljšanje sigurnosnog stanja bez narušavanja osnovne upotrebljivosti poslužitelja, izbjegnute su mjere koje bi mogle uzrokovati nepredvidivo ponašanje sustava ili zahtijevati dodatnu infrastrukturu. Ovakav pristup omogućuje jasnije praćenje učinka pojedinih promjena i smanjuje rizik od neželjenih posljedica.

Konačno, kriteriji odabira hardening mjera oslanjaju se i na smjernice iz NIST Special Publication 800-123, koje sigurnosni hardening promatraju kao iterativan proces. U skladu s tim, u praktičnom dijelu rada naglasak nije stavljen na količinu primijenjenih mjera, već na njihovu smislenost i povezanost s rezultatima sigurnosnog audita. Takav pristup omogućuje jasnu vezu između identificiranih slabosti, odabranih sigurnosnih mjera i njihove kasnije evaluacije.

## 3.5 Provedba hardening mjera

Nakon analize nalaza inicijalnog sigurnosnog audita pristupilo se provedbi odabranih hardening mjera. Hardening je proveden postupno i kontrolirano, u skladu s kriterijima definiranim u prethodnoj cjelini, s ciljem smanjenja napadne površine sustava bez narušavanja njegove osnovne funkcionalnosti.

Primjena hardening mjera obuhvatila je više sigurnosnih područja operacijskog sustava. Posebna pozornost posvećena je konfiguraciji servisa, pri čemu su razmatrane samo one promjene koje su relevantne za poslužiteljsko okruženje i koje ne zahtijevaju dodatne aplikacijske ovisnosti. Time se osigurava da sustav ostane stabilan i predvidiv, uz istovremeno povećanje sigurnosne razine.

Dio hardening mjera odnosio se na mrežne i komunikacijske postavke sustava. U ovom kontekstu naglasak je bio na smanjenju izloženosti sustava nepotrebnim mrežnim funkcionalnostima te na jačanju osnovnih sigurnosnih postavki koje utječu na način komunikacije sustava s okolinom. Takav pristup omogućuje smanjenje rizika od neovlaštenog pristupa bez uvođenja složenih mrežnih rješenja.

Značajan dio hardeninga obuhvatio je i područja autentikacije i kontrole pristupa. Mjere u tom području usmjerene su na jačanje osnovnih sigurnosnih mehanizama operacijskog sustava, s ciljem ograničavanja mogućnosti zloupotrebe korisničkih računa i privilegija. Time se povećava otpornost sustava na napade koji se oslanjaju na eskalaciju privilegija ili kompromitaciju korisničkih identiteta.

Osim navedenog, primijenjene su i mjere povezane sa zapisivanjem i nadzorom sigurnosno relevantnih događaja. Iako ove mjere ne sprječavaju izravno napade, one imaju ključnu ulogu u ranom otkrivanju sumnjivih aktivnosti i u kasnijoj analizi sigurnosnih incidenata. Njihova primjena doprinosi cjelovitijem sigurnosnom profilu sustava.

Sve hardening mjere provedene su ručno, uz svjesno izbjegavanje automatiziranih alata za masovnu primjenu konfiguracija. Takav pristup omogućuje bolju kontrolu nad svakom promjenom, jasnije razumijevanje njezina učinka te lakšu interpretaciju rezultata u završnoj fazi evaluacije sigurnosnog stanja sustava.

## 3.6 Ponovna evaluacija sigurnosnog stanja

Nakon planirane i provedene primjene odabranih hardening mjera predviđena je ponovna evaluacija sigurnosnog stanja sustava. Svrha ove faze jest provjeriti učinak primijenjenog hardeninga i utvrditi u kojoj je mjeri sigurnosna konfiguracija sustava unaprijeđena u odnosu na početno stanje.

Ponovna evaluacija provodi se korištenjem istog alata i istog postupka kao i inicijalni sigurnosni audit. Time se osigurava dosljednost u procjeni sigurnosnog stanja te omogućuje usporedivost rezultata prije i nakon primjene hardening mjera. Korištenje istog alata uklanja utjecaj metodoloških razlika i omogućuje fokus na promjene nastale isključivo kao posljedica provedenog hardeninga.

Rezultati ponovne evaluacije služe za konceptualnu usporedbu sigurnosnog stanja sustava prije i nakon intervencija. U ovoj fazi naglasak nije na pojedinačnim nalazima ili numeričkim vrijednostima, već na općem trendu promjena i identificiranju područja u kojima je došlo do poboljšanja sigurnosne konfiguracije. Takav pristup omogućuje objektivnu procjenu učinka hardening mjera bez ulaska u tehničke detalje.

Provedba ponovne evaluacije ujedno zatvara ciklus sigurnosnog procesa opisanog u ovom radu, koji obuhvaća inicijalni audit, primjenu sigurnosnih mjera i njihovu evaluaciju. Time se uspostavlja jasna poveznica između praktičnog dijela rada i kasnije usporedbe s postojećim rješenjima, u kojoj se rezultati ove faze koriste kao temelj za analizu učinkovitosti odabranog pristupa.

## 3.7 Sažetak praktičnog dijela

U praktičnom dijelu rada prikazan je strukturiran pristup sigurnosnom auditu i hardeningu Linux poslužiteljskog sustava, proveden u kontroliranom virtualnom okruženju. Praktični rad organiziran je kroz jasno definirane faze koje slijede preporučeni sigurnosni ciklus, od inicijalne procjene stanja do ponovne evaluacije sigurnosti.

Najprije je uspostavljeno radno okruženje temeljeno na Ubuntu Server 22.04 LTS, nakon čega je proveden inicijalni sigurnosni audit s ciljem dobivanja objektivnog uvida u početno sigurnosno stanje sustava. Na temelju nalaza audita definirani su kriteriji za odabir hardening mjera, pri čemu je primijenjen selektivan pristup usklađen s CIS Benchmarks smjernicama i NIST preporukama.

Planirana i provedena primjena hardening mjera usmjerena je na ključna sigurnosna područja poslužiteljskog sustava, uz naglasak na kontrolirani opseg promjena i očuvanje funkcionalnosti sustava. Završna faza praktičnog dijela obuhvaća ponovnu evaluaciju sigurnosnog stanja, kojom se zatvara ciklus audita i hardeninga te stvara temelj za analizu učinka odabranog pristupa.

Ovim praktičnim dijelom uspostavljena je jasna veza između teorijskih smjernica i njihove primjene u stvarnom sustavu. Rezultati i iskustva stečena kroz opisani proces koriste se u sljedećoj cjelini rada, u kojoj se provedeni pristup uspoređuje s postojećim rješenjima i istraživanjima iz literature.
