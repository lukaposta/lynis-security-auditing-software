# 1. Uvod

Sigurnost informacijskih sustava predstavlja jedan od ključnih izazova u suvremenim računalnim okruženjima. Linux poslužitelji široko se koriste za pružanje mrežnih usluga, pohranu podataka i izvršavanje aplikacija, zbog čega često postaju meta različitih sigurnosnih prijetnji. Iako Linux operacijski sustavi nude visoku razinu stabilnosti i pouzdanosti, njihova stvarna sigurnost uvelike ovisi o pravilnoj konfiguraciji i kontinuiranom nadzoru.

Svježe instalirani Linux sustavi najčešće koriste zadane postavke koje su usmjerene na funkcionalnost i jednostavnost korištenja, a ne na maksimalnu sigurnost. Takav pristup povećava rizik od pogrešnih konfiguracija i izlaganja sustava potencijalnim napadima. Zbog toga su sigurnosni audit i hardening nužni koraci u uspostavi pouzdanog i sigurnog poslužiteljskog okruženja.

U ovom projektu koristi se alat Lynis, open-source rješenje za sigurnosni audit UNIX i Linux sustava. Lynis provodi sustavnu analizu konfiguracije operacijskog sustava, identificira potencijalne sigurnosne slabosti te daje preporuke za njihovo ublažavanje. Alat je primjenjiv u različitim okruženjima, uključujući edukativne i manje sustave, što ga čini prikladnim za praktičnu primjenu u okviru ovog rada.

Cilj projekta je provesti sigurnosni audit svježe instaliranog Ubuntu Server 22.04 LTS sustava korištenjem alata Lynis, identificirati odabrane sigurnosne nedostatke te primijeniti dio preporučenih hardening mjera. Nakon provedbe hardeninga, audit se ponovno izvršava kako bi se usporedilo početno i završno stanje sustava. Rezultati se analiziraju i interpretiraju uz korištenje CIS Benchmarks kao referentnog sigurnosnog standarda.

Projekt je koncipiran kao praktični rad bez razvoja vlastitog aplikacijskog koda. Naglasak je stavljen na razumijevanje sigurnosnih nalaza, selektivnu primjenu hardening mjera i analizu njihovog utjecaja na ukupnu sigurnosnu razinu sustava. Na taj način rad povezuje teorijske smjernice iz područja sigurnosti informacijskih sustava s konkretnom primjenom u stvarnom Linux poslužiteljskom okruženju.
