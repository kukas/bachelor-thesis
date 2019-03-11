# Datasety

Nedostupnost dostatečného množství dat pro automatickou transkripci melodie představuje zejména pro metody strojového učení otevřený problém. Zatímco pro vzdáleně příbuznou úlohu automatického přepisu mluveného slova existuje tisíce hodin nahrávek (například dataset LibriSpeech, který vznikl na základě audioknih), největší dataset s přepsanou melodickou linkou MedleyDB má celkovou délku pod osm hodin. Do roku 2014, kdy MedleyDB vznikl, existovaly datasety, které byly buď rozmanité, ale příliš krátké (ADC04, MIREX05, INDIAN08) nebo naopak celkově větší, ale žánrově a hudebně homogenní (MIREX09, MIR1K, RWC). V roce 2015 byl vydán dataset Orchset, který obsahuje 23 minut výňatků z orchestrálních skladeb různých období. Za dataset pro extrakci melodie by se také dal považovat Weimar Jazz Database, který je sice primárně zaměřený na využití v muzikologii, nicméně obsahuje přes 450 přepsaných jazzových sól. Novinkou z roku 2017 je vydání datasetu MDB-melody-synth, který byl automaticky vygenerován základě vstupní vícestopé hudby (převzaté z MedleyDB), existuje tedy naděje, že současný korpus pro přepis melodie by se mohl v budoucnu rozšířit o velkou část veřejně dostupných vícestopých nahrávek.

Co se týče blízké úlohy transkripce hudby, velikost největších datasetů se pohybuje v řádu desítek hodin, tudíž jde stále o omezené kolekce. Mezi největší se řadí MusicNet (orchestrální, 34 hodin), MAPS (klavír, 18 hodin), MDB-mf0-synth (multižánrový, 4,7 hodin), GuitarSet (kytara, 3 hodiny) a URMP (komorní orchestr, 1,3 hodiny). I když jde o úlohu, která je lépe definovaná (na rozdíl od extrakce melodie v celkovém přepisu nehraje takovou roli subjektivita při volbě hlavního hlasu), s použitím polyfonních nástrojů vyvstává problém náročné ruční anotace.

Vytváření nových datasetů je obecně velmi pracné a nákladné. Obvyklý postup totiž zahrnuje buď kompletní ruční přepis nahrávky nebo alespoň ruční opravu výstupu automatického přepisu, přičemž tuto práci odvedou nejkvalitněji pouze zaškolení hudebníci. Každá vzniklá anotace by se také měla překontrolovat, a to nejlépe jiným hudebníkem. Dalším problémem je vůbec identifikace melodie - jelikož je určení hlavní melodické linie subjektivní, musí se na výsledné anotaci shodnout co nejvíce posluchačů, ve výsledku se proto vybírají takové nahrávky, které nejsou sporné. S tím souvisí také zavedení a pečlivé dodržování anotační politky u komplexnějších skladeb (zejména orchestrálních), kde může melodii nést více hlasů zároveň (současně či střídajíc se). Také množství výchozích dat pro vznik datasetů není velké, jednak musí být skladby šiřitelné, pokud má být dataset volně dostupný a jednak by k nim měly být dostupné audiostopy, ze kterých je smíchán finální mix, jelikož ruční anotace pouze s pomocí finálního mixu je mnohem náročnější než anotace stop.

Existence dostatenčně velkého množství dat je obecně vzato zásadním předpokladem pro využití metod strojového učení pomocí hlubokých neuronových sítí, zejména pak pro netriviální úlohy, jakou je například přepis melodie, jelikož dovoluje zvětšení celkové kapacity modelu, aniž by docházelo k overfittingu. Také pro evaluaci metod, například i v soutěži MIREX, jsou potřeba takové datasety, které dobře reprezentují reálná data, přitom MedleyDB vzniklo mimo jiné z důvodu, že stávající datasety nestačily ani pro tento účel. 

Možností řešení nastíněného probému nedostatku dat je více. Přímým řešením by byl návrh metody, která by celý proces vzniku datasetů výrazně ulehčila. O to se snaží článek \cite{Salamon2017} a princip této metody popisuje kapitola XXX. 

------------

nepřímé metody:
    - augumentace dat \cite{Thickstun2018}, \cite{Kum2016}
        - pitch, noise
        - remixování mdb-mf0-synth
    - syntetická data \cite{Bittner2018}
    - multitask learning \cite{Bittner2018}
    - crowdsourcing \cite{Tse2016}


## MedleyDB \cite{Bittner2014}
Multimodální, vícestopý dataset obsahující 122 nahrávek, k 108 z nich je dostupná anotace melodie. Kromě té obsahuje také metadata o všech písní s informacemi o žánru a instrumentalizaci. S celkovou délkou 7.3 hodiny jde o nejdelší dataset, který se zaměřuje na více hudebních žánrů. O rozmanitosti svědčí i to, že se v datasetu vyskytuje řada nástrojů mimoevropského původu, a že jen přibližně polovina písní obsahuje zpěv. Na rozdíl od ostatních datasetů jsou nahrávky ve většině případů celé písně, tedy nejde pouze o krátké výňatky, a ke každé jsou poskytnuty audiostopy, ze kterých je vytvořen výsledný mix.
Na základě diskuze, kterou shrnuji v kapitole o definici melodie, autoři poskytují tři verze anotací, na základě různě obecných definic:

1. The f0 curve of the predominant melodic line drawn from a single source.
    - Základní frekvence nejvýraznějšího melodického hlasu, jehož zdroj zůstává po dobu nahrávky neměnný. (Tato definice je shodná pro evaluační datasety používané v soutěži MIREX, s výjimkou Orchsetu)
2. The f0 curve of the predominant melodic line drawn from multiple sources.
    - Základní frekvence nejvýraznějšího melodického hlasu, jehož zdroje se mohou měnit.
3. The f0 curves of all melodic lines drawn from multiple sources.
    - Základní frekvence všech melodických hlasů, které mohou pocházet z více zdrojů.

Ačkoli třetí definice dovoluje, aby v anotaci znělo více melodických linek zároveň, nejedná se o kompletní přepis nahrávek, ten autoři neposkytují.

Dataset vznikl obvyklou cestou ruční anotace, ze shromážděného vícestopého materiálu byly vybrány stopy s potenciálním výskytem melodie, stopy s přeslechem byly filtrovány pomocí source-separation algoritmu s ručně doladěnými parametry pro každou jednotlivou stopu, následně byl na monofonní stopy spuštěn pitch-tracker pYIN a výsledné automaticky získané anotace opravilo a vzájemně zkontrolovalo pět anotátorů s hudebním vzděláním. 

------------

- definice 1. je shodná s definicí používanou v ostatních datasetech vyhodnocovaných v MIREXu
    "pitch is expressed as the fundamental frequency of the main melodic voice, and is reported in a frame-based manner on an evenly-spaced time- grid."

## MDB-synth \cite{Salamon2017}

Hlavním přínosem práce \cite{Salamon2017} je navržení způsobu anotace základní frekvence monofonních audiostop takovým způsobem, že výsledná dvojice zvukové stopy a anotace nevyžaduje další manuální kontrolu. Anotace stopy probíhá ve dvou krocích, nejprve získáme pomocí libovolného monopitch trackeru křivku základní frekvence a poté na základě této křivky, která může obsahovat chybné úseky, syntetizujeme novou stopu, která zachovává barvu nahrávky, ale výšku tónu určuje právě tato anotace. Díky tomu je pak přesnost anotace pro tuto novou, syntetickou nahrávku stoprocentní, přitom (v ideálním případě) neztrácí charakteristiky původní nahrávky.

Pro vytváření datasetu je toto významné zjednodušení, protože tím algoritmus odstraňuje časově nejnáročnější část práce - ruční kontrolu anotací audiostop. Pokud by se ukázalo, že syntéza významně neubírá na kvalitě dat, použitím navrhované metody by mohlo vzniknout velké množství nových dat (napříkad repozitář Open Multitrack Testbed obsahuje stovky vícestopých nahrávek, které by šlo využít). Autoři v článku provádí kvantitativní analýzu pomocí srovnání state-of-the-art algoritmů pro extrakci melodie a prokazují, že výsledky těchto metod na syntetických datech se významně neliší od výsledků na původních, tím je podle autorů potvrzená možnost použití dat jak pro trénování tak pro evaluaci nových metod.

Metoda má ale bohužel svá omezení, mezi ty zásadní patří, že se dá aplikovat pouze na stopy, které obsahují monofonní signál, vstupní data tedy nesmí obsahovat přeslech a nahrávaný nástroj může hrát pouze jednohlase, v důsledku nelze zpracovat klavír či kytara, které hrají zpravidla vícehlas. To nevadí tolik u generování datasetu pro přepis melodie, jelikož melodii často hraje jeden hlas a doprovod hrají ostatní, velkým nedostatkem je toto spíše pro generování multif0 datasetů.

Dále k článku není zveřejněná kompletní refereční implementace algoritmu, tudíž algoritmus nelze snadno spustit na nových datech. Ve výsledku je tudíž největším praktickým přínosem nová sada syntetických datasetů pro úlohy přepisu melodie, basy, monofonních stop a kompletní partitury, každý dataset obsahuje destíky nahrávek. Vícestopá data použitá pro syntézu byla převzata z MedleyDB, tudíž ve výsledku nové datasety nerozšiřují celkový hudební záběr, pouze zpřesňují ten již existující.

_TODO obrázek? Porovnání spektrogramů syntetické a původní nahrávky_

Z kvalitativního pohledu je na výstupních syntetických nahrávkách poznat, že jsou syntetické. Autoři sice prokazují, že současné metody na těchto datech dosahují stejných výsledků, nicméně v článku chybí diskuse o tom, zda-li v datech algoritmus nevytváří nové umělé artefakty, které by mohly zneužít metody strojového učení pro spolehlivější výsledky (které by však negeneralizovaly na reálná data). Při pohledu na spektrogram je například zřejmé, že syntetická nahrávka obsahuje mnohem více výrazných alikvótních frekvencí


------------

V článku ale chybí diskuze, zda-li syntetická data neobsahují artefakty, které by mohly využít metody strojového učení

- pohled na spektrogram:    
    - syntetická data mají mnohem výraznější alikvóty - jdou tam opravdu donekonečna
    - žádný šum
    - neobsahuje nic než harmonické frekvence + harmonické frekvence jsou nyní v naprosto přesných násobcích
        - wikipedie: While it is true that electronically produced periodic tones (e.g. square waves or other non-sinusoidal waves) have "harmonics" that are whole number multiples of the fundamental frequency, practical instruments do not all have this characteristic. For example, higher "harmonics"' of piano notes are not true harmonics but are "overtones" and can be very sharp, i.e. a higher frequency than given by a pure harmonic series. This is especially true of instruments other than stringed or brass/woodwind ones, e.g., xylophone, drums, bells etc., where not all the overtones have a simple whole number ratio with the fundamental frequency. The fundamental frequency is the reciprocal of the period of the periodic phenomenon.[5]
        https://en.wikipedia.org/wiki/Harmonic

- syntetické stemy zní divně
    - onsety umí ujet
        - vokály s frikativy - sinusoidal modeling umí zachytit pouze harmonický signál, chybí šum
            The synthesis used in this study is purely harmonic, which affects the quality of the synthesis and could potentially affect the perception of note onsets (e.g., vocals with fricatives).
- method
    1. pitch tracking
        - pomocí SAC = E. Gomez and J. Bonada. Towards computer-assisted flamenco transcription: An experimental comparison of automatic transcription algorithms from a cappella singing. Computer Music journal, 37(2):73–90, 2013.
        - nicméně je to nezávislé na 
    2. sinusoidal modeling
        - přesné zjistění harmonických parametrů
    3. synthesis
        - pomocí banky sinusových oscilátorů na základě parametrů zjištěných v 2.
    4. remixing
        - protože mix není triviální součet stemů, používá se vážená lineární kombinace s odhadem vah na základě původních stemů

## Orchset \cite{Bosch2016}

Dataset orientovaný na orchestrální repertoár pocházející z různých historických období včetně 20. století. Obsahuje 64 výňatků délky od 10 do 32 sekund. Výňatky byly vybírány tak, aby obsahovaly zřejmou melodii, dataset tedy obsahuje v porovnání málo pasáží bez melodie (6% z celkové délky). Vzhledem k komplexitě uvažovaných žánrů autoři vycházejí z kombinace rozšířené definice melodie podle \cite{Bittner2014} a definice \cite{Poliner2007}. Melodii ve výňatcích proto zpravidla nese více hudebních nástrojů (nebo celých sekcí), které se v průběhu střídají, případně mohou části hrát společně v rozdílných oktávách (nebo jiných intervalech, tvoříce tak harmonický doprovod). 

Pro zjištění melodie se v takto vrstveném materiálu autoři uchylují k úplnému základu definice melodie (Poliner) a nechávají si skupinou čtyř posluchačů výňatky přezpívávat. Tato hrubá data pak autoři sumarizují a odebírají z datasetu ty výňatky, na jejichž melodii se posluchači neshodli. Přezpívané tóny bylo nutné ručně opravit, aby načasováním přesně seděly na výňatek. Lidský hlas také samozřejmě nemá rozsah plného orchestru, proto bylo dalším krokem transponovat anotace tak, aby zněly ve správných oktávách. Zde se opět může vyskytnout problém subjektivity, pokud melodii hrají dva různé nástroje, pouze v jiných oktávách, pak je sporné, který nástroj označit jako hlavní (v některých případech taková otázka ani nedává příliš smysl.). Částečným řešením je zvolit libovolnou anotační politiku a tu konzistentně dodržovat (žádná společná v komunitě MIR neexistuje), v případě Orchsetu byla snaha minimalizovat skoky v melodické kontuře, což zároveň respektuje obecné pozorování, že v melodii se vyskytují mnohem častěji malé skoky (nejčastěji prima a malá/velká sekunda) než větší. Tedy například pokud pasáži hrané ve dvou různých oktávách předcházela pasáž hraná v jedné, anotace obou pasáží lze transponovat do společné oktávy tak, abychom na rozhraní minimalizovali skok v anotaci.

Dataset obsahuje pouze hrubé anotace tónů melodie, nikoli přesnou základní frekvenci nástroje, který v danou chvíli melodii hraje. Článek o tomto rozhodnutí příliš nediskutuje, vychází ale opět logicky z volby dat. U orchestrálních dat je tento abstraktnější pojem melodie mnohem méně sporný. Pokud hraje melodii sekce nástrojů v unisonu, přesná základní frekvence není dobře definovaná, jelikož se základní frekvence znějících hlasů vzájemně překrývají.

------------

* Melodic intervals generally lie in a relatively small range, according to the voice leading principle of pitch proximity (Huron 2001). The most common sequence of two notes is a perfect unison, followed by a major second, and then minor second either descending or ascending. Previous works obtained similar conclusions, such as Dressler (2012b) with a dataset of 6000 MIDI files from varied genres, or Friberg and Ahlb¨ack (2009) in a dataset of polyphonic ring tones. The

## Wjazzd \cite{Pfleiderer}

Weimar Jazz Database obsahuje přes 450 transkripcí jazzových sól ze všech období vývoje jazzu. Data původně zamýšlená pro muzikologické studie využívající statistické metody ale lze využít i pro potřeby extrakce melodie, jelikož uvažované nahrávky spadají zřejmě pod nejrestriktivnější definici melodie (definici používanou v soutěži MIREX) - melodii nese jistě právě jeden, sólový nástroj, a po celou dobu výňatku je jistě nejvýraznější. Výběr sólových nástrojů se omezuje pouze na jednohlasé, jelikož ruční anotace vícehlasých je příliš obtížná. Hlavním problémem při využívání je restriktivní licence, která platí na nahrávky, tudíž zdrojové audio, na základě kterého anotace vznikaly, není veřejně přístupné.
Jelikož pro data neexistují jednotlivé stopy, ruční anotace probíhala přímo z finální nahrávky, což je obtížný úkol - 


- monofonní sóla, transkripce obsahuje pouze hlavní nástroj, nikoli doprovod
    - "polyfonní by bylo obtížné anotovat"
    - tudíž se zde nemusí příliš řešit problém, jaký hlas je hlavní, dobře tu funguje původní MIREXová definice melodie
- manuální transkripce přímo z audia pomocí Sonic Visualizer

The twelve staff members involved in the transcription and annotation process were students of either musicology, music education or the jazz program at the Music University ‘Franz Liszt’ Weimar. They had various musical backgrounds but were in general familiar with jazz, mostly by both listening to and playing jazz. Despite a high level of expertise, the quality of the tran- scriptions inevitably varied according to the respective solos, transcribers and their form on the day

Moreover, the transcribed pitches are highly accurate since they are cross-checked several times by several persons. Nevertheless, the transcriptions still involve a moment of fuzziness due to several subjective factors and algorithmic short-comings in regard to the metrical beat grid, pitch notation and the annotation of phrases and midlevel units. One has to keep these aspects in mind whenever one explores the data.

Pitch transcription
In regard to pitches, uncertainties were rather scarce. At the most, it some- times turned out to be difficult to determine a definite pitch within fast lines, very low tones and glissandi, as well as in the case of slides, ambiguously intoned tones and appoggiaturas or grace notes. While the cross-checked pitch notation is, in general, very precise, one has to keep in mind that every tone—even a very short appoggiatura or the many tones within a longer glissando—is notated in the same way an as, e. g., a long tone played over a whole bar. Slides at the beginning of a tone, ‘bends’ (raising or lowering the pitch within a tone) and ‘fall-offs’ at its ending were either notated as two (or more) separate tones or as one tone with an additional note (‘slide’, ‘bend’, ‘fall-off’) in the annotation text layer (see the glossary for further explanation).

the transcribed part of the original audio recording is separated into a backing track, which includes the accompanying instruments, and a solo track, which includes the improvising soloist (cf. p. 101). Then, the underlying tuning frequency is estimated from the backing track (cf. p. 101). In the next step, we estimate tone-wise fundamental frequency contours and intensity contours from the isolated solo instrument track (cf. p. 104 and p. 109). Finally, we describe each contour using more abstract features such as the fundamental frequency modulation range or the median tone intensity (cf. p. 104).

- ladění
    - první nahrávky jsou z dvacátých let, přitom A4=440 bylo definováno 1955
    - piana se postupně rozlaďují
    - artefakty nahrávek - zrychlování a zpomalování ovlivňuje ladění
    -> proto se musí z doprovodu před extrakcí f0 získat ladění
        - piano a kontrabas jsou pro odhad f0 spolehlivější než sólové nástroje, které si s laděním v průběhu sóla hrají
- fundamental frequency contour tracking
    - dvě možnosti:
        - sledovat nejbližší peak na spektrogramu
        - spustit pYIN na hlavní nástroj

## Obecně
- oproti jiným MIR odvětvím, speech recognition nebo image recognition je to málo dat \cite{Salamon2017}
    - možné zlepšení:
        - augumentace
            - pokud je ale málo dat, augumentace neudělá zázraky
        - syntéza
        - multitask learning (cite Bittnerová)
- co vlastně od datasetu chci:
    - přesnou anotaci
        - frame-level annotation
        - note-level annotation
    - bez chyb
    - objektivní
- typy datasetů
    - multitrack
    - melody midi/f0
- vznik datasetů
    - přezpívání a manuální korekce
        - Orchset
    - automatic f0 estimation z monofonních stemů + ruční kontrola
        - ADC2004, zdroj: http://ismir2004.ismir.net/melody_contest/results.html
        - MIREX05, zdroj
        - MedleyDB
        - time consuming and labor intensive
            - For example, manual corrections for MedleyDB (108 songs, most 3–5 minutes long) required approximately 50 hours of effort across annotators [7,29]
    - audio to MIDI alignment
        - omezeno robustností alignment algoritmu a dostupností kvalitních MIDI dat
        - musicnet
    - MIDI klavír + partitura + klavírista, který hraje přes nahrávku
        - nicméně negeneruje f0, ale noty
    - hudební nástroje se senzory, které generují anotace
        - ale logicky omezeno na jeden druh nástroje
        - GuitarSet
    - syntéza
    - jen drobně zmínit: crowdsourcing - https://s18798.pcdn.co/ismir2016/wp-content/uploads/sites/2294/2016/07/072_Paper.pdf
- srovnání dostupných datasetů:
    - MedleyDB
        - obsahuje pouze melodická data, tedy nemá multif0
    - MDB-synth
        - kvantitativně posouzené v článku
        - kvalitativně u některých nástrojů zní dataset divně - vadí to hlavně u melodických dat
    - tabulka

|                 | MedleyDB | Orchset | MIREX    | MusicNet | URMP | MDB-synth | WJAZZD? | DSD100 |
|-----------------|----------|---------|----------|----------|------|-----------|---------|--------|
| mix audio       | Y        | Y       | Y        | Y        | Y    | Y         | Y       | Y      |
| stem audio      | Y **     | N       | N        | N        | Y    | Y         | N       | Y      |
| Multi f0        | *        | N       | N        | N        | Y    | Y         | N       | N      |
| MIDI            | *        | N       | N        | Y        | Y    | Y         | N       | N      |
| Stem MIDI       | Y        | N       | N        | Y        | Y    | Y         | N       | N      |
| Stem priority   | Y        | N       | N        | N        | N    | Y         | N       | N      |
| Melody f0       | Y        | N       | N        | N        | N    | Y         | Y       | N      |
| Melody MIDI     | Y        | Y       | Y        | N        | N    | Y         | Y       | N      |
| Hours of audio  | 7.3      | 23min   | N        | 34       | 1.3  | 4.65      | ? >10 ? | 7      |

**přidat další řádek:**
    - poměr voiced/unvoiced framů

* pouze monofonické nástroje, pouze melodie (tj. doprovod není anotován)
** část stemů s větším či menším bleedem (toto je anotované)

pěkný podobný seznam datasetů: https://arxiv.org/pdf/1612.08727.pdf


#### Nové datasety
- URMP
    - délka 1,3h 
    - klasika
    - kompletní anotace a multitrack

- WEIMAR JAZZ DATABASE (WJAZZD)
    - jazzová sóla

Synth datasets: http://synthdatasets.weebly.com/
https://ismir2017.smcnus.org/wp-content/uploads/2017/10/164_Paper.pdf
AN ANALYSIS/SYNTHESIS FRAMEWORK FOR AUTOMATIC F0R ANNOTATION OF MULTITRACK DATASETS
- ​The datasets are intended for research on monophonic, melody, bass, and multiple f0 estimation (pitch tracking), and include:

- MDB-melody-synth
- 65 songs from the MedleyDB dataset in which the melody track has been resynthesized to obtain a perfect melody f0 annotation using the analysis/synthesis method described in the paper.

- MDB-bass-synth
- 71 songs from the MedleyDB dataset in which the bass track has been resynthesized to obtain a perfect bass f0 annotation using the analysis/synthesis method described in the paper.

- MDB-mf0-synth
- 85 songs from the MedleyDB dataset in which polyphonic pitched instruments (such as piano and guitar) have been removed and all monophonic pitched instruments (such as bass and voice) have been resynthesized to obtain perfect f0 annotations using the analysis/synthesis method described in the paper.

- MDB-stem-synth
- 230 solo stems (tracks) from the MedleyDB dataset spanning a variety of musical instruments and voices, which have been resynthesized to obtain a perfect f0 annotation using the analysis/synthesis method described in the paper.

- Bach10-mf0-synth
- 10 classical music pieces (four-part J.S. Bach chorales) from the Bach10 dataset where each instrument (bassoon, clarinet, saxophone and violin) has been resynthesized to obtain perfect f0 annotations using the analysis/synthesis method described in the paper.


- MedleyDB
    - Přidali nové multitracky

ISMIR2018:
DALI - vocal MIDI
GuitarSet - polyphonic guitar

#### Staré datasety

- MusicNet
    - 34 hodin
    - MIDI všech melodických nástrojů (rozlišené nástroje)
    - způsob: Dynamic time warp MIDI souborů k nahrávkám
    - bez vyznačení hlavní melodie
- Li Su
    - multif0, 10 mixů
    - 5 min
- Bach10
    - multif0, 10 chorálů
    - 5.5 min
- MAPS
    - 18.6h !
    - ale jenom piano

- Orchset
    - 23 minut hudby
    - MIDI hlavní melodie



Track : Beethoven-S7-II-ex2
- použít jako příklad pro složitost přepisu - chyba o ooktávu flétny+ dřeva
Track : Beethoven-S5-II-ex3
- podobný příklad, akorát porovnat oktávy - zjistit anotačn politiku (každopádně to ukazuje složitost anotace)

Track : Beethoven-S5-II-ex1
- nejdřív orchestr společně, pak se rozdělí do akordu. Pak v tercii


#### Syntéza
- The Lakh MIDI Dataset v0.1

The Lakh MIDI dataset is a collection of 176,581 unique MIDI files, 45,129 of which have been matched and aligned to entries in the Million Song Dataset. Its goal is to facilitate large-scale music information retrieval, both symbolic (using the MIDI files alone) and audio content-based (using information extracted from the MIDI files as annotations for the matched audio files).

- https://www.classicalarchives.com/

\chapter{Dostupné datasety k úloze}
Obecně je vytváření kvalitních datasetů otevřený problém, kvalitních dat je oproti jiným oborům, kde se využívá strojové učení, poměrně málo. Neexistuje spolehlivá, levná a rychlá metoda pro jejich vytváření, nejblíže se takové zatím blíží nová publikovaná metoda \citep{Salamon2017}. 

Podle \cite{su2015escaping} lze metody vytváření datasetů kategorizovat do překrývajících se množin:
1) manuální metody, 2) metody využívající více audiostop, 3) metody zarovnávající nahrávku a partiturou, 4) metody s transkripcí pomocí piana, 5) syntéza.


\section{Datasety s více melodickými linkami}
- MusicNet \\
Nový, velký dataset, koncipovaný jako základ pro strojové učení. Žánrově se jedná o klasickou hudbu, celkově 34 hodin anotovaných nahrávek. Anotace obsahují i informaci o nástroji. V doprovodném článku \citep{thickstun2016learning} jsou nastíněné tři jednoduché architektury sítí.
Vytvořený automaticky, pomocí zarovnávání MIDI souborů k volně dostupným nahrávkám koncertů (metoda Dynamic time warping), nicméně ručně kontrolovaný.

- MedleyDB \\
viz níže.

- The Lakh MIDI Dataset v0.1 \\
Přes 45000 MIDI souborů automaticky přiřazených a zarovnaných k odpovídajícím částím písní z Million Song Datasetu. Přesnost přepisu melodie ale nebyla v doprovodném článku \citep{datasetRaffel16} ověřována. V článku \citet{Bittner:DeepSalience:ISMIR:17} se o datasetu autoři zmiňují jako o "weakly-labeled data".

- Li Su dataset, MIREX MultiF0 Development Set \\
Malé datasety používané při evaluaci v soutěži MIREX.

Li Su přichází s nápadem použít MIDI klavír + celou partituru + zkušeného hudebníka, který hraje jednotlivé party všech nástrojů, tedy midi klavír anotuje i jiné hudební nástroje. Problémem je glissando a jiné prvky, které klávesy nezachytí.

Je zde také vyhlídka na nový, velký dataset. Framework pro vytváření kvalitních dat z vícestopých nahrávek byl prezentován na ISMIR 2017 \citep{Salamon2017}. Využívá podobného postupu jako MedleyDB, ale řeší problém náročnosti ruční kontroly. Z původní nahrávky vytvoří novou, syntezovanou verzi, která melodicky odpovídá přesně automaticky získané anotaci, díky tomu je celková chybovost datasetu nulová. Podobně jako u Medley, i zde pravděpodobně bude spojitá informace o frekvenci melodie.

Ostatní:

- Bach10 \\
Malý dataset složený z deseti Bachových chorálů.

- MAPS \\
Pouze klavír, 31GB nahrávek, část syntetická, část hraná disklavírem.

- RWC dataset \\
100 populárních písní, 50 klasických skladeb, 50 jazzových nahrávek, přístup k datasetu je ale zpoplatněn.
pouze MIDI, nikoli f0

\section{Datasety s hlavní melodií}

- MedleyDB
Rozmanitý a kvalitní dataset s jednotlivými audiostopami, celkem 7 hodin hudby. Anotace byly generovány z oddělených audiostop a poté prošly ruční korekcí. Obsahuje tři druhy anotací, podle různých definic melodie. Jako jediný ze zmíněných nemá frekvenci melodie zaokrouhlenou na nejbližší tón. U jednotlivých stop je informace o nástroji.
Dalším specifikem je, že krom hlavní melodie také obsahuje anotace ostatních melodických linek, autoři v doprovodném článku považují zapsání všech melodií v daném časovém úseku za nejobecnější definici melodie.

- ORCHSET \\
Created within the PHENICX project, it contains 64 audio excerpts from symphonies, symphonic poems, ballets suites and other musical forms interpreted by symphonic orchestras. The ground truth melody pitch is human-labeled with semitone quantisation and a hop size of 10 ms. The length of the excerpts ranges from 10 to 32 seconds.

Celkově 23 minut hudby.

Ostatní, používané při evaluaci v MIREXu:

MIREX09 database
- 374 Karaoke recordings of Chinese songs. Each recording is mixed at three different levels of Signal-to-Accompaniment Ratio \{-5dB, 0dB, +5 dB\} for a total of 1122 audio clips. Instruments: singing voice (male, female), synthetic accompaniment. The groundtruth pitch of each clip is human labeled, with a frame size of 40ms, a hop size of 20 ms. Note that the center of the first frame is located at 20ms starting from the very beginning of a clip. The human labeled pitch is then interpolated to have a hop size of 10ms. Thus the time sequence of the pitch vector are 20ms, 30ms, 40ms, 50ms, and so on.
- 374 Karaoke recordings of Chinese songs (i.e. recorded singing with karaoke accompaniment). Each recording is mixed at three different levels of signal-to-accompaniment ratio {-5dB, 0dB, +5dB} for a total of 1,122 audio clips. Total play time: 10,022s.
2)


INDIAN08 database
- 4 excerpts of 1 min. from "north Indian classical vocal performances", instruments: singing voice (male, female), tanpura (Indian instrument, perpetual background drone), harmonium (secondary melodic instrument) and tablas (pitched percussions). There are two different mixtures of each of the 4 excerpts with differing amounts of accompaniment for a total of 8 audio clips.
- Four 1 minute long excerpts from north Indian classical vocal performances. There are two mixes per excerpt with differing amounts of accompaniment for a total of 8 audio clips. Total play time: 501s.

MIREX05 database:
- 25 phrase excerpts of 10-40 sec from the following genres: Rock, R\&B, Pop, Jazz, Solo classical piano.
- 25 excerpts of a 10-40s duration in the genres of rock, R&B, pop, jazz and solo classical piano. Includes real recordings and audio generated from MIDI files. Total play time: 686s.

ADC04 database: 
- Dataset from the 2004 Audio Description Contest. 20 excerpts of about 20s each.
- manually annotated reference data (10 ms time grid)
- 20 excerpts of roughly 20s in the genres of pop, jazz and opera. Includes real recordings, synthesized singing and audio generated from MIDI files. Total play time: 369s.

\section{Použité datasety}

Pro \itext{mf0+audio->f0} baseline přichází v úvahu pouze dataset MedleyDB, protože má podle popisu obsahovat jednak anotaci melodie (f0) a jednak anotace všech dalších melodických linek ve skladbách. 