# Datasety

nepřímé metody:
    - augumentace dat \cite{Thickstun2018}, \cite{Kum2016}
        - pitch, noise
        - remixování mdb-mf0-synth
    - syntetická data \cite{Bittner2018}
    - multitask learning \cite{Bittner2018}
    - crowdsourcing \cite{Tse2016}


## MedleyDB \cite{Bittner2014}

- definice 1. je shodná s definicí používanou v ostatních datasetech vyhodnocovaných v MIREXu
    "pitch is expressed as the fundamental frequency of the main melodic voice, and is reported in a frame-based manner on an evenly-spaced time- grid."

## MDB-synth \cite{Salamon2017}

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

* Melodic intervals generally lie in a relatively small range, according to the voice leading principle of pitch proximity (Huron 2001). The most common sequence of two notes is a perfect unison, followed by a major second, and then minor second either descending or ascending. Previous works obtained similar conclusions, such as Dressler (2012b) with a dataset of 6000 MIDI files from varied genres, or Friberg and Ahlb¨ack (2009) in a dataset of polyphonic ring tones. The

## Wjazzd \cite{Pfleiderer}

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
    - tabulka

|                 | MedleyDB | Orchset | ADC2004  | MIREX05  | MDB-synth | WJAZZD? | MusicNet | URMP | DSD100 |
|-----------------|----------|---------|----------|----------|-----------|---------|----------|------|--------|
| mix audio       | Y        | Y       | Y        | Y        | Y         | Y       | Y        | Y    | Y      |
| stem audio      | Y **     | N       | N        | N        | Y         | N       | N        | Y    | Y      |
| Multi f0        | *        | N       | N        | N        | Y         | N       | N        | Y    | N      |
| MIDI            | *        | N       | N        | N        | Y         | N       | Y        | Y    | N      |
| Stem MIDI       | Y        | N       | N        | N        | Y         | N       | Y        | Y    | N      |
| Stem priority   | Y        | N       | N        | N        | Y         | N       | N        | N    | N      |
| Melody f0       | Y        | N       | N        | N        | Y         | Y       | N        | N    | N      |
| Melody MIDI     | Y        | Y       | Y        | Y        | Y         | Y       | N        | N    | N      |
| Hours of audio  | 7.3***   | 23.4m   | 6.1m     | 6.5m     | 3.19      | 8.85    | 34       | 1.3  | 7      |
| Voiced frames   | 60.9%    | 93.69%  | 85.7%    | 63.1%    | 50.4%     | 62.8%   |          |      |        |
| Number of tracks| 122***   | 64      | 20       | 13       | 65        | 299     |          |      |        |

* pouze monofonické nástroje, pouze melodie (tj. doprovod není anotován)
** část stemů s větším či menším bleedem (toto je anotované)
*** 5.59h s anotací, 108 tracků

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