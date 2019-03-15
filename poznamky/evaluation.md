# Evaluace metod

## MIREX

Soutěž MIREX (Music Information Retrieval Evaluation eXchange) probíhá již od roku 2005 a v MIR komunitě zastává hlavní postavení jakožto každoroční událost pro nezávislé, objektivní srovnání state-of-the-art metod a algoritmů pro řešení širokého spektra úloh souvisejících se zpracováním hudebních dat. Mezi tyto úlohy patří například _rozpoznání žánru_, _odhad tempa_, _odhad akordů_, _identifikace coveru_ a samozřejmě také _extrakce melodie_.

Na rozdíl od jiných úloh, kde debata o zvolení nejvhodnějších objektivních metrik pro porovnávání stále probíhá, metriky pro extrakci melodie se ustanovily již v prvním ročníku (na základě dřívějších zkušeností) a zůstaly neměnné dodnes \cite{Raffel2014}. Naopak data použitá pro testování se postupně kumulují a dnes soutěž probíhá již s řadou datasetů (ADC04, MIREX05, MIREX08, MIREX09, ORCHSET), které blíže popisuje kapitola o dostupných datech.

------

Výsledky soutěže jsou prezentovány na mezinárodní konferenci ISMIR, 

\cite{Downie2010}

## Trénovací, validační a testovací množina

Z dostupných dat, které pro úlohu máme k dispozici, musíme vyhradit množiny pro trénování, validaci a testování, aby byly metody porovnatelné jak mezi sebou, tak se stávajícími state-of-the-art metodami. Pro trénování se jeví jako nejvhodnější dataset MedleyDB, jednak pro svou délku a jednak pro žánrovou rozmanitost, proto je použit pro většinu popsaných experimentů. Rozdělení na tři části vychází z práce \cite{Bittner2017} a \cite{DBasaranSEssid2018}, aby byly metriky přímo porovnatelné s výsledky v uvedených článcích. Další výhodou použití stejného _splitu_ je možnost reprodukce výsledků, za použití popisované architektury, a tím pádem minimalizování možnosti nějaké velké implementační chyby v kódu. Pokud by se totiž výsledky nepodařilo reprodukovat se stejnými daty i architekturou, musela by být chyba jinde - tedy s největší určitostí v vyvinutém frameworku.

Dalším zdrojem dat je dataset _MDB-melody-synth_, který je přesyntetizován z vícestopých nahrávek _MedleyDB_, proto se nabízí použít stejné rozdělení dat, jaké se používá pro _MedleyDB_, ze stejných důvodů uvedených v předchozím odstavci. Jelikož dataset neobsahuje veškerá data, ale pouze jejich podmnožinu, i v experimentech používaný _split_ obsahuje pouze podmnožinu z původního _splitu_ datasetu _MedleyDB_. 

Posledním velkým datasetem, používaným pro trénování, je _Weimar Jazz Database_. Zde žádný doporučený postup ani výběr rozdělení dataestu v relevatní literatuře neexistuje, proto jsem dataset rozdělil podle metody \cite{Bittner2017} na tři části (v celkové délce nahrávek na části velikosti 63%, 14% a 23%). Skladby jsou rozděleny do částí podle interpretů tak, aby se každý interpret vyskytoval právě v jedné části datasetu. Toto omezení na podmnožiny \cite{Bittner2017} nediskutuje, lze však doložit (práce \cite{Sturm2013}), že pro úlohu _rozpoznání žánru_ metody založené na strojovém učení vykazují po trénování a validaci na datech bez tohoto filtru výrazně lepší výsledky než stejné metody spuštěné na roztříděných datech, takové zlepšení výkonu je ale jistě umělým důsledkem špatné volby dat. 

Ostatní datasety (ADC04, MIREX05, ORCHSET) jsou v práci použity pouze jako testovací data, díky tomu lze korektně výsledky přímo srovnávat s žebříčky úlohy Melody Extraction v soutěži MIREX.

------

- MatthewEntwistle_FairerHopes
obsahuje harfu, ale trénovací data ji neobsahují, chce to ale víc prozkoumat, jelikož trénovací data neobsahují víc nástrojů, tak zjistit přesně přesnost anotace pro tyto nástroje

## Kvalitativní příklady

Pro lepší porozumění hranic testovaných metod je vhodné studovat také výsledky na kvalitativních ukázkách. Modely byly při práci vyhodnocovány na několikaminutových množinách výňatků z validačních a testovacích dat. Metodika výběru spočívala v poslechu nahrávek a ručním hledáním zajímavých hudebních jevů a také v seřazení nahrávek podle úspěšnosti přepisu stávajícími metodami a výběrem výňatků právě z těchto nejproblematičtějších příkladů.

Omezení plynoucí z potřeby zkrátit výňatky na minimum,


## Metriky


po algoritmu chceme:
- správnou výšku tónů melodie (pitch estimation)
- jestli melodie zní nebo ne (voicing detection)

- (mirex formát) - 10 ms hop, seznam časů a frekvencí, 0 = žádná melodie

- per-frame comparison (MIREX 2005)
    - Voicing Recall Rate: The proportion of frames labeled as melody frames in the ground truth that are estimated as melody frames by the algorithm.
    - Voicing False Alarm Rate: The proportion of frames labeled as non-melody in the ground truth that are mis- takenly estimated as melody frames by the algorithm.
    - Raw pitch accuracy: The proportion of melody frames in the ground truth for which fτ is considered correct
    - Raw Chroma Accuracy: As raw pitch accuracy, except that both the estimated and ground truth f0 sequences are mapped onto a single octave.
    - Overall Accuracy: this measure combines the perfor- mance of the pitch estimation and voicing detection tasks to give an overall performance score for the system.

    - upozornit, že voicing metriky a pitch metriky jsou na sobě nezávislé (algoritmy mohou udávat pitch v záporných hodnotách). 
    - opravil jsem chroma accuracy v mir_eval

- limitace jsou předvedeny v onsets+frames

- Bosch metrics \cite{Bosch2016}
    - Weighted Raw Chroma accuracy - počítá vzdálenost v oktávách
    - Octave Jumps - vyjadřuje skokovitost o oktávy v po sobě následujících framech v rámci správných chroma odhadů
    - Chroma continuity - 

- moje
    - harmonic accuracy
    - confusion matrix
    - estimation distance histogram
    - pitch accuracy per note

# Kvalitativní
- popsat můj small_validation