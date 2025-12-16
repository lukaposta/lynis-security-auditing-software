# 4. Usporedba vlastitog sa postojećim radovima/rješenjima

U ovoj cjelini provodi se usporedba vlastitog pristupa sigurnosnom auditu i hardeningu Linux poslužiteljskog sustava s odabranim rješenjima i istraživanjima iz literature. Cilj usporedbe nije rangiranje ili vrednovanje pojedinih radova, već analiza sličnosti i razlika u metodološkim pristupima, korištenim alatima, razini automatizacije i načinu evaluacije sigurnosnog stanja.

Usporedba se temelji na radovima koji su prethodno analizirani u pregledu srodnih radova te na teorijskim smjernicama i standardima koji se koriste kao referentni okvir. Poseban naglasak stavljen je na opravdanje vlastitog pristupa u kontekstu postojećih rješenja, uz jasno isticanje njegovih prednosti i ograničenja. Time se omogućuje smještanje ovog projekta u širi kontekst postojećih praksi sigurnosnog audita i hardeninga Linux sustava.

## 4.1. Kriteriji usporedbe

Kako bi usporedba bila sustavna i metodološki utemeljena, definirani su jasni kriteriji prema kojima se vlastiti pristup uspoređuje s postojećim rješenjima iz literature. Odabrani kriteriji proizlaze iz ciljeva rada i karakteristika analiziranih radova, a omogućuju objektivnu analizu različitih pristupa sigurnosnom auditu i hardeningu.

Prvi kriterij odnosi se na opseg sustava koji se analizira. Uspoređuje se pristup koji se fokusira na jedan operacijski sustav s radovima koji obuhvaćaju više distribucija ili različita okruženja. Time se razmatra utjecaj širine opsega na dubinu analize i složenost provedbe.

Drugi kriterij odnosi se na korištene alate i standarde. Analizira se uporaba postojećih alata za sigurnosni audit u odnosu na razvoj vlastitih ili prilagođenih rješenja, kao i uloga sigurnosnih standarda poput CIS Benchmarks u procesu audita i hardeninga. Posebno se razmatra razlika između korištenja standarda kao referentnog okvira i njihove primjene kao stroge mjere usklađenosti.

Treći kriterij obuhvaća razinu automatizacije. Uspoređuju se radovi koji se oslanjaju na automatizirane alate za audit i hardening s pristupima koji naglašavaju ručnu primjenu sigurnosnih mjera. Ovaj kriterij omogućuje analizu odnosa između složenosti rješenja, fleksibilnosti i razine kontrole nad promjenama konfiguracije.

Četvrti kriterij odnosi se na pristup hardeningu. Uspoređuje se selektivan pristup primjeni sigurnosnih mjera s radovima koji implementiraju cjelovite ili automatizirane hardening postupke. Time se razmatra utjecaj opsega hardeninga na funkcionalnost sustava i interpretaciju rezultata.

Konačno, kriteriji usporedbe uključuju i način evaluacije sigurnosnog stanja. Analizira se korištenje inicijalnog i ponovnog audita, sigurnosnih ocjena, compliance postotaka ili kvalitativne procjene kao načina mjerenja učinka primijenjenih sigurnosnih mjera. Ovaj kriterij omogućuje usporedbu mjerljivosti i interpretabilnosti rezultata u različitim pristupima.

Definirani kriteriji služe kao temelj za daljnje podcjeline ove usporedbe, u kojima se vlastiti pristup sustavno analizira u odnosu na pojedine skupine postojećih rješenja.

## 4.2. Usporedba metodološkog pristupa

Metodološki pristup vlastitog rada temelji se na faznom procesu koji uključuje inicijalni sigurnosni audit, selektivnu primjenu hardening mjera i ponovnu evaluaciju sigurnosnog stanja. Takav pristup omogućuje jasno praćenje promjena i njihovo povezivanje s identificiranim sigurnosnim slabostima, uz očuvanje preglednosti i kontrole nad opsegom zahvata.

U usporedbi s tim, Disi (2022) primjenjuje sličan fazni pristup koji također uključuje audit, hardening i ponovni audit, ali u širem eksperimentalnom kontekstu. Njegov rad obuhvaća više alata i distribucija te naglasak stavlja na kvantitativnu procjenu poboljšanja sigurnosnog stanja kroz compliance metrike. Za razliku od toga, vlastiti rad svjesno se ograničava na jedan operacijski sustav i jedan alat, čime se postiže veća jasnoća metodološkog tijeka i lakša interpretacija postupaka.

Akporhuarho (2025) primjenjuje drugačiji metodološki pristup, usmjeren na visoku razinu automatizacije. U tom radu audit i hardening dio su automatiziranog sustava koji provodi sigurnosne mjere uz minimalnu ručnu intervenciju. Takav pristup pogodan je za razvojna okruženja i mala poduzeća, ali smanjuje razinu izravne kontrole nad pojedinačnim konfiguracijskim promjenama. Nasuprot tome, vlastiti rad naglašava ručni i interpretativni pristup, koji omogućuje detaljnije razumijevanje uzroka i posljedica sigurnosnih promjena.

Radovi koji se fokusiraju isključivo na sigurnosni audit, poput Sedano i Salman (2021) te Sholihin i Salman (2025), imaju metodologiju ograničenu na procjenu postojećeg stanja sustava. U tim radovima ne provodi se hardening niti ponovna evaluacija, zbog čega metodološki ciklus ostaje nepotpun u kontekstu praktične primjene sigurnosnih mjera. Vlastiti rad razlikuje se upravo po tome što audit ne promatra kao cilj sam po sebi, već kao polaznu točku za daljnje sigurnosne intervencije.

Specijalizirani pristup prikazan u radu Ansong i sur. (2025) metodološki je usmjeren na jednu sigurnosnu domenu, odnosno privilege escalation ranjivosti. Iako se taj rad oslanja na Lynis kao temeljni alat, njegov metodološki fokus znatno je uži u odnosu na vlastiti pristup, koji obuhvaća širi skup sigurnosnih domena tipičnih za Linux poslužiteljski sustav.

U cjelini, metodološki pristup vlastitog rada može se opisati kao umjeren i uravnotežen. On ne teži potpunoj automatizaciji niti eksperimentalnoj širini, već jasno strukturiranom i razumljivom procesu koji povezuje teorijske smjernice, sigurnosni audit i selektivni hardening u koherentnu cjelinu. Takav pristup omogućuje jasnu usporedbu s postojećim rješenjima te predstavlja razumnu sredinu između složenih automatiziranih sustava i isključivo auditno orijentiranih istraživanja.

## 4.3. Usporedba korištenih alata i standarda

U vlastitom radu korišten je jedan alat za sigurnosni audit, Lynis, uz oslanjanje na CIS Benchmarks kao referentni sigurnosni standard. Takva kombinacija omogućuje jasan razdor između alata koji provodi tehničku analizu sustava i standarda koji služi kao okvir za interpretaciju nalaza i odabir hardening mjera. Time se izbjegava vezivanje audita uz strogu provjeru usklađenosti, a naglasak se stavlja na razumijevanje sigurnosnog stanja sustava.

Uspoređujući ovaj pristup s radovima iz literature, uočavaju se značajne razlike u načinu korištenja alata i standarda. Sedano i Salman (2021) koriste CIS Benchmarks kao primarni temelj audita, pri čemu se provjera sigurnosnog stanja provodi pomoću alata Chef InSpec. U tom slučaju CIS preporuke služe kao izravni kriterij uspješnosti, a rezultati se izražavaju kroz razinu usklađenosti s definiranim kontrolama. Nasuprot tome, u vlastitom radu CIS Benchmarks ne predstavljaju ciljnu metriku, već referentni okvir za vrednovanje nalaza dobivenih alatom Lynis.

Sličan compliance-orijentirani pristup prisutan je i u radu Sholihin i Salman (2025), koji razvijaju alat OSCAT za automatiziranu provjeru usklađenosti s CIS Benchmarks standardima. Njihov alat usmjeren je na formaliziranu i skalabilnu procjenu konfiguracije sustava, pri čemu se sigurnost promatra kroz prizmu prolaska ili pada definiranih kontrola. U usporedbi s tim, vlastiti rad koristi postojeći audit alat bez dodatnih prilagodbi, a standarde promatra fleksibilnije, s naglaskom na interpretaciju i kontekst.

Rad Disi (2022) koristi kombinaciju više alata i sigurnosnih standarda, uključujući CIS Benchmarks, kako bi se postigla sveobuhvatna analiza sigurnosnog stanja različitih sustava. Takav pristup povećava širinu analize, ali ujedno uvodi veću složenost u metodologiju i interpretaciju rezultata. Vlastiti rad, ograničen na jedan alat i jedan standard, namjerno smanjuje tu složenost kako bi se osigurala preglednost i jasnoća postupka.

Specijalizirani rad Ansong i sur. (2025) također koristi Lynis, ali ga proširuje dodatnim mehanizmima za detekciju privilege escalation ranjivosti. U tom slučaju Lynis služi kao temelj za razvoj novog alata, dok u vlastitom radu ostaje u izvornom obliku. Ta razlika dodatno naglašava da se vlastiti pristup ne oslanja na prilagodbu ili razvoj alata, već na njegovu primjenu u standardnom obliku.

U konačnici, usporedba pokazuje da se vlastiti rad razlikuje po jasnoj podjeli uloga između alata i standarda. Lynis se koristi za tehničku procjenu sigurnosnog stanja, dok CIS Benchmarks služe kao referentna smjernica, a ne kao stroga mjera usklađenosti. Takav pristup omogućuje veću fleksibilnost u odabiru i primjeni hardening mjera te olakšava razumijevanje odnosa između nalaza audita i sigurnosnih preporuka.

## 4.4. Razina automatizacije i složenost rješenja

Razina automatizacije predstavlja jedan od ključnih aspekata razlikovanja vlastitog pristupa u odnosu na postojeća rješenja iz literature. U ovom radu sigurnosni audit i hardening provode se ručno, uz korištenje postojećeg audit alata, bez razvoja dodatnih skripti ili automatiziranih mehanizama za primjenu sigurnosnih mjera. Takav pristup smanjuje tehničku složenost rješenja i omogućuje potpunu kontrolu nad svakom promjenom konfiguracije.

Suprotan pristup vidljiv je u radu Akporhuarho (2025), koji razvija automatizirano rješenje za audit i hardening Linux sustava. U tom radu sigurnosne provjere i remedijacija provode se gotovo u potpunosti automatski, s ciljem smanjenja administrativnog opterećenja i ubrzavanja procesa osiguravanja sustava. Iako takva automatizacija povećava učinkovitost i skalabilnost, ona ujedno povećava složenost rješenja i smanjuje transparentnost pojedinačnih konfiguracijskih promjena.

Visoka razina automatizacije prisutna je i u radu Sholihin i Salman (2025), gdje je razvijen alat OSCAT za automatiziranu provjeru usklađenosti s CIS Benchmarks standardima. Njihovo rješenje namijenjeno je velikim okruženjima u kojima je potrebno brzo i konzistentno provjeriti velik broj sustava. U tom kontekstu naglasak je na formaliziranom auditu, dok se ručna interpretacija nalaza svodi na minimum.

Nasuprot tome, vlastiti rad svjesno izbjegava razvoj složenih automatiziranih rješenja i dodatne infrastrukture. Ograničavanjem na jedan sustav i ručni pristup, omogućuje se detaljnije razumijevanje odnosa između sigurnosnog audita, identificiranih slabosti i odabranih hardening mjera. Takav pristup posebno je prikladan u edukativnom i analitičkom kontekstu, gdje je cilj razumijevanje procesa, a ne masovna primjena sigurnosnih politika.

Usporedba razine automatizacije pokazuje da se vlastiti rad nalazi na suprotnoj strani spektra u odnosu na automatizirana i skalabilna rješenja iz literature. Dok automatizacija donosi prednosti u velikim i složenim okruženjima, ručni i jednostavniji pristup primijenjen u ovom radu omogućuje veću transparentnost, lakšu interpretaciju rezultata i jasnije povezivanje teorijskih smjernica s praktičnom primjenom sigurnosnih mjera.

## 4.5. Fokus analize i sigurnosni opseg

Fokus analize u vlastitom radu usmjeren je na cjelovitu procjenu sigurnosnog stanja Linux poslužiteljskog sustava, uz obuhvaćanje više sigurnosnih domena koje su tipične za serverska okruženja. Audit i hardening promatraju se kao opći procesi koji zahvaćaju konfiguraciju operacijskog sustava, mrežne postavke, autentikaciju, kontrolu pristupa te zapisivanje i nadzor sigurnosnih događaja. Takav široki sigurnosni opseg omogućuje dobivanje uravnotežene slike ukupne sigurnosne konfiguracije sustava.

U literaturi se, međutim, pojavljuju i radovi s užim, specijaliziranim fokusom. Primjer takvog pristupa je rad Ansong i sur. (2025), koji se usmjerava isključivo na detekciju i ublažavanje privilege escalation ranjivosti. Iako taj rad koristi Lynis kao temeljni alat, sigurnosni opseg analize ograničen je na jednu specifičnu klasu ranjivosti. Takav pristup omogućuje dublju analizu pojedinog sigurnosnog problema, ali ne pruža cjeloviti uvid u ukupno sigurnosno stanje sustava.

S druge strane, radovi poput onih Sedano i Salman (2021) te Sholihin i Salman (2025) obuhvaćaju širi sigurnosni opseg, ali isključivo kroz audit konfiguracije sustava. U tim radovima sigurnost se promatra kroz razinu usklađenosti s definiranim standardima, bez provedbe sigurnosnih mjera koje bi mogle utjecati na različite sigurnosne domene u praksi. Time se fokus analize zadržava na identifikaciji problema, a ne na njihovom rješavanju.

Vlastiti rad zauzima srednju poziciju između ovih pristupa. On ne ulazi u dubinsku analizu pojedinačnih vrsta ranjivosti, ali ni ne ostaje isključivo na razini formalne provjere konfiguracije. Umjesto toga, sigurnosni opseg definiran je tako da obuhvati najvažnije sigurnosne domene Linux poslužiteljskog sustava, uz selektivnu primjenu hardening mjera koje proizlaze iz rezultata audita.

Takav uravnotežen fokus omogućuje povezivanje teorijskih smjernica i praktične primjene sigurnosnih mjera, uz zadržavanje jasne strukture i preglednosti analize. U kontekstu usporedbe s postojećim rješenjima, vlastiti pristup ističe se kao općenit i primjenjiv na tipične serverske scenarije, bez potrebe za specijalizacijom ili složenim prilagodbama.

## 4.6. Evaluacija rezultata i mjerljivost

Način evaluacije sigurnosnog stanja predstavlja važan element usporedbe između vlastitog rada i postojećih rješenja iz literature. U vlastitom radu evaluacija se temelji na usporedbi početnog i završnog sigurnosnog stanja sustava, pri čemu se koristi isti audit alat i isti postupak u obje faze. Takav pristup omogućuje dosljednu i usporedivu procjenu učinka primijenjenih hardening mjera, bez utjecaja metodoloških razlika.

U radu Disi (2022) evaluacija rezultata provodi se kroz kvantitativne metrike koje izražavaju razinu sigurnosti i usklađenosti sustava prije i nakon primjene hardening mjera. Naglasak je na mjerljivom poboljšanju sigurnosnog stanja, što omogućuje jasnu demonstraciju učinka provedenih intervencija. Sličan pristup, ali bez faze hardeninga, prisutan je u radu Sedano i Salman (2021), gdje se rezultati audita izražavaju kroz postotak usklađenosti s CIS Benchmarks preporukama.

Radovi koji se fokusiraju na automatizirani audit, poput Sholihin i Salman (2025), također koriste kvantitativne pokazatelje za evaluaciju sigurnosnog stanja. U tim slučajevima mjerljivost se postiže kroz klasifikaciju kontrola i agregiranje rezultata, čime se dobiva formalizirana slika sigurnosti sustava. Međutim, takva evaluacija ostaje ograničena na stanje sustava u trenutku audita, bez analize promjena koje bi nastale primjenom sigurnosnih mjera.

U vlastitom radu mjerljivost se promatra u širem smislu, kroz konceptualnu usporedbu stanja prije i nakon hardeninga. Iako se koriste numerički pokazatelji koje pruža audit alat, naglasak nije isključivo na apsolutnim vrijednostima, već na smjeru promjena i poboljšanju sigurnosne konfiguracije u relevantnim domenama. Takav pristup omogućuje interpretaciju rezultata u kontekstu primijenjenih mjera i ciljeva rada.

Usporedba pokazuje da se vlastiti pristup nalazi između strogo kvantitativnih compliance-orijentiranih evaluacija i isključivo kvalitativnih procjena. Time se postiže ravnoteža između mjerljivosti i interpretabilnosti rezultata, što je posebno prikladno za analitičke i edukativne projekte u području sigurnosti Linux poslužiteljskih sustava.

## 4.7. Prednosti i ograničenja vlastitog pristupa

Usporedba s postojećim rješenjima omogućuje jasnije sagledavanje prednosti i ograničenja vlastitog pristupa sigurnosnom auditu i hardeningu Linux poslužiteljskog sustava. Jedna od glavnih prednosti vlastitog rada jest jednostavnost metodološkog okvira. Fokusiranjem na jedan operacijski sustav i jedan audit alat postiže se visoka razina preglednosti, što olakšava razumijevanje cijelog procesa i interpretaciju rezultata.

Dodatna prednost vlastitog pristupa jest transparentnost. Ručna primjena hardening mjera omogućuje potpunu kontrolu nad promjenama konfiguracije i jasnu povezanost između nalaza sigurnosnog audita i primijenjenih mjera. Takav pristup posebno je koristan u edukativnom i analitičkom kontekstu, gdje je razumijevanje uzročno-posljedičnih veza važnije od brzine ili skalabilnosti rješenja.

Korištenje CIS Benchmarks kao referentnog okvira, a ne kao stroge mjere usklađenosti, predstavlja dodatnu prednost vlastitog rada. Time se omogućuje fleksibilan i kontekstualiziran odabir hardening mjera, uz istodobno zadržavanje veze s priznatim sigurnosnim standardima. Ovakav pristup razlikuje se od compliance-orijentiranih rješenja iz literature i omogućuje prilagodbu sigurnosnih odluka stvarnim potrebama sustava.

S druge strane, vlastiti pristup ima i jasno definirana ograničenja. Analiza je ograničena na jedan operacijski sustav i jedno okruženje, što smanjuje opću primjenjivost rezultata u širem kontekstu. Za razliku od nekih radova iz literature, u ovom radu ne razmatra se skalabilnost rješenja niti primjena sigurnosnih mjera u velikim ili heterogenim okruženjima.

Ograničenje predstavlja i izostanak visoke razine automatizacije. Iako ručni pristup donosi prednosti u pogledu kontrole i razumijevanja, on nije prikladan za okruženja s velikim brojem sustava ili za situacije u kojima je potrebna brza i ponovljiva primjena sigurnosnih politika. U tom smislu, vlastiti rad svjesno žrtvuje skalabilnost u korist jednostavnosti i jasnoće.

## 4.8. Sažetak usporedbe

Usporedba vlastitog pristupa s postojećim rješenjima iz literature pokazuje da se ovaj rad smješta između teorijskih i visoko automatiziranih praktičnih rješenja. Za razliku od radova koji se fokusiraju isključivo na audit ili na automatizirani hardening, vlastiti pristup kombinira sigurnosni audit i selektivnu primjenu hardening mjera u jasno strukturiranom i razumljivom procesu.

Analiza metodološkog pristupa, korištenih alata, razine automatizacije, sigurnosnog opsega i načina evaluacije rezultata potvrđuje da je vlastiti rad metodološki utemeljen i u skladu s postojećim sigurnosnim praksama. Istodobno, jasno su identificirana i njegova ograničenja, koja proizlaze iz namjernog pojednostavljenja okruženja i izostanka automatizacije.

Takav pristup pokazuje se prikladnim za analitičke i edukativne projekte, u kojima je cilj razumijevanje procesa sigurnosnog audita i hardeninga, a ne razvoj skalabilnih ili produkcijskih rješenja. Time se ovaj rad smisleno uklapa u postojeći korpus istraživanja te pruža jasnu osnovu za završnu evaluaciju i zaključke koji slijede u sljedećem poglavlju.
