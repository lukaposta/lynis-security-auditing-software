# 1. Vlastito radno okruženje i inicijalna konfiguracija sustava

Radno okruženje za praktični dio rada postavljeno je s ciljem osiguravanja kontroliranih i ponovljivih uvjeta za provedbu sigurnosnog audita Linux poslužiteljskog sustava. U ovoj fazi dokumentira se isključivo inicijalno stanje sustava, bez primjene hardening mjera ili provođenja sigurnosnog audita.

## 1.1. Virtualizacijsko okruženje

Za potrebe praktičnog dijela rada korišteno je virtualizacijsko okruženje temeljeno na alatu Oracle VirtualBox. Virtualizacija omogućuje izolaciju sustava, jednostavno upravljanje konfiguracijom te potpunu kontrolu nad hardverskim i softverskim parametrima, što je posebno važno u kontekstu sigurnosne analize.

Oracle VirtualBox odabran je zbog svoje dostupnosti, stabilnosti i široke primjene u edukativnim i razvojnim okruženjima. Alat omogućuje precizno definiranje resursa virtualnog stroja, kao i jednostavno ponovno stvaranje istog okruženja, što doprinosi ponovljivosti eksperimenta.

Virtualni stroj korišten u ovom radu služi isključivo kao platforma za sigurnosnu analizu Linux poslužiteljskog sustava i nije povezan s produkcijskim okruženjima. Svi daljnji koraci projekta provedeni su unutar ovog virtualnog okruženja.

## 1.2. Konfiguracija virtualnog stroja

Prije instalacije operacijskog sustava konfiguriran je virtualni stroj u Oracle VirtualBoxu s unaprijed definiranim hardverskim parametrima. Cilj ove konfiguracije bio je osigurati stabilno i realistično poslužiteljsko okruženje, uz minimalan, ali dostatan skup resursa potreban za rad Ubuntu Server sustava i provođenje sigurnosne analize.

Virtualnom stroju dodijeljena su dva procesorska jezgra i 2048 MB radne memorije. Takva konfiguracija predstavlja tipično minimalno serversko okruženje te omogućuje nesmetan rad sustava, alata za audit i osnovnih servisa, bez uvođenja nepotrebnih varijabli koje bi mogle utjecati na rezultate analize.

Kao virtualni disk korišten je VDI format, s dinamičkom alokacijom prostora i ukupnim kapacitetom od 25 GB. Disk je konfiguriran kao jedan virtualni uređaj bez prethodne prealokacije pune veličine, što omogućuje učinkovitije korištenje resursa host sustava. EFI način pokretanja nije korišten, kako bi se zadržala standardna i jednostavna konfiguracija pokretanja sustava.

Mrežna konfiguracija virtualnog stroja postavljena je na NAT način rada. Takva postavka omogućuje sustavu pristup internetu bez dodatne konfiguracije mreže, što je nužno za dohvat paketa, ažuriranja i alata poput Lynisa, uz istovremeno zadržavanje izolacije od lokalne mreže.

Ovom konfiguracijom virtualnog stroja uspostavljeno je kontrolirano i ponovljivo okruženje koje predstavlja osnovu za sve daljnje faze praktičnog dijela rada.

## 1.3. Instalacija operacijskog sustava

Operacijski sustav instaliran je kao Ubuntu Server 22.04.5 LTS (jammy) za x86_64 (amd64) arhitekturu. Odabrana je LTS verzija radi stabilnosti, dugoročne podrške i usklađenosti s CIS Ubuntu Linux 22.04 LTS Benchmark (v3.0.0) dokumentom koji se koristi kao referentni standard u projektu.

Instalacija je provedena ručno, bez korištenja opcije Unattended Installation u Oracle VirtualBoxu. Ručna instalacija omogućuje potpunu kontrolu nad instalacijskim odlukama i transparentnu dokumentaciju svih postavki, što je važno za kasniju interpretaciju nalaza sigurnosnog audita.

Tijekom instalacije donesene su osnovne odluke koje utječu na sigurnost i kasniju analizu sustava:

- Odabrana je standardna opcija Ubuntu Server, bez minimized varijante, kako bi početno stanje sustava odgovaralo uobičajenoj serverskoj instalaciji i kako bi dostupnost osnovnih alata bila predvidiva.
- Dodatni snap paketi nisu odabrani, kako se ne bi proširivala napadna površina sustava prije početnog audita.
- Ubuntu Pro je preskočen, kako početno stanje sustava ne bi bilo dodatno "ojačano" izvan standardne LTS instalacije.
- Disk je konfiguriran korištenjem cijelog virtualnog diska uz LVM, bez LUKS enkripcije, kako bi konfiguracija ostala jednostavna i usporediva te kako bi se izbjegla dodatna složenost koja nije u fokusu ovog projekta.
- Mrežne postavke ostavljene su na zadanim vrijednostima (DHCP, bez proxyja, zadani mirror), kako bi početno stanje sustava bilo što bliže standardnoj instalaciji.

Nakon završetka instalacije sustav je ponovno pokrenut i uspješno podignut s virtualnog diska, čime je uspostavljena početna platforma za sljedeće korake projekta.

## 1.4. Osnovna konfiguracija sustava

Nakon uspješne instalacije operacijskog sustava provedena je osnovna konfiguracija sustava s ciljem potvrde ispravnog rada i pripreme okruženja za daljnje faze projekta. Ova konfiguracija ne uključuje sigurnosni hardening niti instalaciju alata za audit, već se zadržava na minimalnim provjerama i nužnim radnjama.

Prijava u sustav izvršena je korištenjem lokalnog korisničkog računa definiranog tijekom instalacije. Time je potvrđeno ispravno funkcioniranje autentikacije, korisničkog okruženja i dodjele administratorskih (sudo) ovlasti.

Nakon prijave provjerena je verzija operacijskog sustava kako bi se potvrdilo da je instalirana točna verzija Ubuntu Server 22.04 LTS. Ova provjera osigurava usklađenost sustava s CIS Benchmarks dokumentom i ostalim referentnim materijalima korištenima u projektu.

U ovoj fazi nisu mijenjane konfiguracijske datoteke, nisu dodavani korisnici niti su instalirani dodatni paketi osim onih uključenih u zadanu instalaciju. Sustav je namjerno ostavljen u stanju koje odgovara svježe instaliranom Ubuntu Server okruženju, kako bi se u sljedećim fazama moglo jasno razlikovati početno stanje sustava od promjena nastalih instalacijom alata za sigurnosni audit i primjenom hardening mjera.

## 1.5. Priprema sustava za sigurnosni audit

Prije instalacije alata za sigurnosni audit provedene su osnovne pripremne radnje kako bi sustav bio u konzistentnom i ažurnom stanju. Cilj ove faze nije promjena sigurnosne konfiguracije, već osiguravanje stabilne polazne točke za provođenje audita.

Kao prvi korak izvršeno je ažuriranje popisa paketa i instaliranih paketa korištenjem službenih Ubuntu repozitorija. Time se osigurava da je sustav u skladu s trenutačno dostupnim zakrpama i ispravcima grešaka, što je preporučena praksa prije provođenja sigurnosne analize.

Osim ažuriranja sustava, instaliran je osnovni skup pomoćnih alata potrebnih za daljnji rad i administraciju sustava. Ovi alati ne utječu izravno na sigurnosnu konfiguraciju, ali omogućuju lakši rad u terminalu i dohvat dodatnih resursa tijekom projekta.

U ovoj fazi nisu provođene nikakve sigurnosne preporuke, hardening mjere niti ručne izmjene konfiguracijskih datoteka. Sustav je zadržan u stanju koje odgovara standardnoj, ažuriranoj instalaciji Ubuntu Servera, čime je osigurana jasna razlika između pripreme sustava i kasnijih sigurnosnih intervencija.


## 1.6. Završno stanje početne konfiguracije

Nakon provedbe instalacije operacijskog sustava i osnovne pripreme okruženja uspostavljeno je stabilno početno stanje Linux poslužiteljskog sustava. Sustav se nalazi u stanju svježe instaliranog i ažuriranog Ubuntu Server 22.04 LTS okruženja, bez primjene sigurnosnog hardeninga ili dodatnih sigurnosnih alata.

U ovoj fazi sustav sadrži isključivo zadane konfiguracije operacijskog sustava i minimalni skup administrativnih alata potrebnih za daljnji rad. Nisu provedene izmjene sigurnosnih postavki, ograničenja servisa niti prilagodbe konfiguracijskih datoteka koje bi mogle utjecati na rezultate inicijalnog sigurnosnog audita.

Takvo početno stanje predstavlja referentnu točku za sljedeću fazu projekta, u kojoj se provodi instalacija alata Lynis i inicijalni sigurnosni audit sustava. Jasno definiranje ovog stanja omogućuje transparentnu usporedbu konfiguracije sustava prije i nakon primjene sigurnosnih mjera te osigurava metodološku konzistentnost praktičnog dijela rada.
