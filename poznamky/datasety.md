# Datasety


## MedleyDB \cite{Bittner2014}
- melody f0 annotations
- instrument activations
- multitrack
    - rozmanité žánry
- částečně automatizováno, spuštěním pYINu na stemy
    - multitrack usnadnuje anotaci
    - problém - tracky můžou mít bleed
        - v paperu tracky source-separovány s ručně doladěnými parametry

- problémem většiny existujících datasetů je buď žánrová homogenita a nebo celkově krátká délka
    - sem spadají všechny datasety používané v soutěži MIREX, tedy ADC2004, MIREX05, MIREX09, MIR1K, ORCHSET

- kvůli nepřesnostem v anotacích, krátké délky jednotlivých excerptů a malému množství nahrávek nemusí MIREX datasety dobře reflektovat opravdový výkon testovaných algoritmů.
    - jediný dataset, který je dostatečně velký (MIREX09) je velmi homogenní (čínský pop). To samé platí i o MIR1K a RWC

- definice melodie:
    1. The f0 curve of the predominant melodic line drawn from a single source.
    2. The f0 curve of the predominant melodic line drawn from multiple sources.
    3. The f0 curves of all melodic lines drawn from multiple sources.

    - definice 1. je shodná s definicí používanou v ostatních datasetech vyhodnocovaných v MIREXu
        "pitch is expressed as the fundamental frequency of the main melodic voice, and is reported in a frame-based manner on an evenly-spaced time- grid."
- The annotations were created by five annotators, all of which were musicians and had at least a bachelor’s degree in music. Each annotation was evaluated by one annotator and validated by another. The annotator/validator pairs were randomized to make the final annotations as unbiased as possible.

## MDB-synth
In this paper we propose a frame- work for automatically generating continuous f0 annota- tions without requiring manual refinement: the estimate of a pitch tracker is used to drive an analysis/synthesis pipeline which produces a synthesized version of the track
- potenciálně řeší problém s datasety automatickým generováním dat z multitracků
    - nicméně není zveřejněný kód
    - syntetické stemy zní divně
        - onsety umí ujet
            - vokály s frikativy - sinusoidal modeling umí zachytit pouze harmonický signál, chybí šum
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
- umí resyntetizovat pouze monofonní nástroje, což je mírně omezující pro AME (přepisovaný bude vždy monofonní, ostatní mohou být polyfonní) a dost omezující pro multi-f0 (mix je složen jen z monofonních nástrojů a perkusí)
    - demonstruje to filtrování při výrobě mdb-melody-synth - musely se vyhodit písně, kde je hlavní nástroj kytara
- ve výsledku tedy máme nový dataset s menší chybou transkripce, který je ale založený na známých datech z MedleyDB
    - mdb-melody-synth má 65 tracků
    - změřeny podobné výsledky z různých AME metod => reprezentuje to reálná data, snad

## Orchset

- vycházejí z rozšíření definice MedleyDB, 
    Such definitions are more useful in the context of symphonic music, which presents further challenges, since melodies are played by alternating instruments or instrument sections *(playing in unison, octave relation, or with harmonised melodic lines)*, and which might not be energetically predominant.
- a z původní definice (Poliner 2007) - dataset vznikl přezpíváváním a zpracováním těchto nahrávek
- neexistuje obecně přijímaná metodologie přepisu melodie pro nahrávky s více nástroji hrajícími melodickou linku

popis a statistika:
- dataset orientovaný na orchestrální repertoár
- z většiny romantismus, obsahuje ale i další skladby, včetně 20. století
- výběr byl omezený na výňatky, které potenciálně obsahují hlavní melodii
    - což bylo ověřeno (a anotace získána) pomocí nahrávek zpívání od anotátorů
    - celkem 64 nahrávek, z původních 86. 22 jich bylo zahozeno na základě neshody nahrávek od účastníků

    - pouze v jedné nahrávce hraje melodii jediný nástroj
    - In the rest of the dataset, the melody is played by several instruments from an instrument section, or a combination of sections, or even alternating sections within the same excerpt.
        - díky analýzy pomocí informed source separation autoři potvrdili, že melodie nemusí být energeticky nejsilnější zdroj.
        
    - After selecting the excerpts, we manually transcribed the notes sung by the participants, adjusting onsets and offsets to the audio.
    - Since vocal pitch range is different to the range of the instruments playing the main melody, notes were transposed to match the audio.
    - For excerpts in which melody notes are simultaneously played by several instruments in different octaves, we resolved the ambiguity by maximising the melodic contour smoothness (minimising jumps between notes)
    - The length of the excerpts ranges from 10 to 32 seconds. 93.69% of the frames of the dataset are labelled as voiced while 6.31% are unvoiced (in which case the pitch is set to be 0).


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
    - automatic f0 estimation z monofonních stemů + ruční kontrola
        - MedleyDB
        - time consuming and labor intensive
            - For example, manual corrections for Med- leyDB (108 songs, most 3–5 minutes long) required ap- proximately 50 hours of effort across annotators [7,29]
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

přidat:
    - poměr voiced/unvoiced

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

\section{Datasety s hlavní melodií}

- MedleyDB
Rozmanitý a kvalitní dataset s jednotlivými audiostopami, celkem 7 hodin hudby. Anotace byly generovány z oddělených audiostop a poté prošly ruční korekcí. Obsahuje tři druhy anotací, podle různých definic melodie. Jako jediný ze zmíněných nemá frekvenci melodie zaokrouhlenou na nejbližší tón. U jednotlivých stop je informace o nástroji.
Dalším specifikem je, že krom hlavní melodie také obsahuje anotace ostatních melodických linek, autoři v doprovodném článku považují zapsání všech melodií v daném časovém úseku za nejobecnější definici melodie.

Citovaný články: \url{https://scholar.google.cz/scholar?um=1&ie=UTF-8&lr&cites=14156318070025785121}


- ORCHSET \\
Created within the PHENICX project, it contains 64 audio excerpts from symphonies, symphonic poems, ballets suites and other musical forms interpreted by symphonic orchestras. The ground truth melody pitch is human-labeled with semitone quantisation and a hop size of 10 ms. The length of the excerpts ranges from 10 to 32 seconds.

článek: \url{https://www.upf.edu/web/mdm-dtic/-/-audio-orchset-a-dataset-for-melody-extraction-in-symphonic-music-recordings?inheritRedirect=true}
používají: \url{https://scholar.google.cz/scholar?um=1&ie=UTF-8&lr&cites=9877761979300687466}

Celkově 23 minut hudby.

Ostatní, používané při evaluaci v MIREXu:

MIREX09 database: 374 Karaoke recordings of Chinese songs. Each recording is mixed at three different levels of Signal-to-Accompaniment Ratio \{-5dB, 0dB, +5 dB\} for a total of 1122 audio clips. Instruments: singing voice (male, female), synthetic accompaniment. The groundtruth pitch of each clip is human labeled, with a frame size of 40ms, a hop size of 20 ms. Note that the center of the first frame is located at 20ms starting from the very beginning of a clip. The human labeled pitch is then interpolated to have a hop size of 10ms. Thus the time sequence of the pitch vector are 20ms, 30ms, 40ms, 50ms, and so on.

MIREX08 database: 4 excerpts of 1 min. from "north Indian classical vocal performances", instruments: singing voice (male, female), tanpura (Indian instrument, perpetual background drone), harmonium (secondary melodic instrument) and tablas (pitched percussions). There are two different mixtures of each of the 4 excerpts with differing amounts of accompaniment for a total of 8 audio clips.

MIREX05 database: 25 phrase excerpts of 10-40 sec from the following genres: Rock, R\&B, Pop, Jazz, Solo classical piano.

ADC04 database: Dataset from the 2004 Audio Description Contest. 20 excerpts of about 20s each.
manually annotated reference data (10 ms time grid)

\section{Použité datasety}

Pro \itext{mf0+audio->f0} baseline přichází v úvahu pouze dataset MedleyDB, protože má podle popisu obsahovat jednak anotaci melodie (f0) a jednak anotace všech dalších melodických linek ve skladbách. 