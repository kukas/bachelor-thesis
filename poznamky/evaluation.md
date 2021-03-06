# Evaluace metod

## MIREX

Soutěž MIREX (Music Information Retrieval Evaluation eXchange) probíhá již od roku 2005 a v MIR komunitě zastává hlavní postavení jakožto každoroční událost pro nezávislé, objektivní srovnání state-of-the-art metod a algoritmů pro řešení širokého spektra úloh souvisejících se zpracováním hudebních dat. Mezi tyto úlohy patří například _rozpoznání žánru_, _odhad tempa_, _odhad akordů_, _identifikace coveru_ a samozřejmě také _extrakce melodie_.

Na rozdíl od jiných úloh, kde debata o zvolení nejvhodnějších objektivních metrik pro porovnávání stále probíhá, metriky pro extrakci melodie se ustanovily již v prvním ročníku (na základě dřívějších zkušeností) a zůstaly neměnné dodnes \cite{Raffel2014}. Naopak data použitá pro testování se postupně kumulují a dnes soutěž probíhá již s řadou datasetů (ADC04, MIREX05, MIREX08, MIREX09, ORCHSET), které blíže popisuje kapitola o dostupných datech.

------

Výsledky soutěže jsou prezentovány na mezinárodní konferenci ISMIR, 

\cite{Downie2010}

## Trénovací, validační a testovací množina

Z dostupných dat, které pro úlohu máme k dispozici, musíme vyhradit množiny pro trénování, validaci a testování, aby byly metody porovnatelné jak mezi sebou, tak se stávajícími state-of-the-art metodami. Pro trénování se jeví jako nejvhodnější dataset MedleyDB, jednak pro svou délku a jednak pro žánrovou rozmanitost, proto je použit pro většinu popsaných experimentů. Rozdělení na tři části vychází z práce \cite{Bittner2017} a \cite{DBasaranSEssid2018}, aby byly metriky přímo porovnatelné s výsledky v uvedených článcích. Další výhodou použití stejného _splitu_ je možnost replikace výsledků, za použití popisované architektury, a tím pádem minimalizování možnosti nějaké velké implementační chyby v kódu. Pokud by se totiž výsledky nepodařilo replikovat se stejnými daty i architekturou, musela by být chyba jinde - tedy s největší určitostí v vyvinutém frameworku.

Dalším užitečným zdrojem dat je dataset _MDB-melody-synth_, který je přesyntetizován z vícestopých nahrávek _MedleyDB_, proto se nabízí použít stejné rozdělení dat, jaké se používá pro _MedleyDB_, ze stejných důvodů uvedených v předchozím odstavci. Jelikož dataset neobsahuje veškerá data, ale pouze jejich podmnožinu, i v experimentech používaný _split_ obsahuje pouze podmnožinu z původního _splitu_ datasetu _MedleyDB_. 

Posledním velkým datasetem, používaným pro trénování, je _Weimar Jazz Database_. Zde žádný doporučený postup ani výběr rozdělení dataestu v relevatní literatuře neexistuje, proto jsem dataset rozdělil podle metody \cite{Bittner2017} na tři části (v celkové délce nahrávek na části v poměrech 63%, 14% a 23%). Skladby jsou rozděleny do částí podle interpretů tak, aby se každý interpret vyskytoval právě v jedné části datasetu. Toto omezení na podmnožiny \cite{Bittner2017} nediskutuje, lze však doložit (práce \cite{Sturm2013}), že pro úlohu _rozpoznání žánru_ metody založené na strojovém učení vykazují po trénování a validaci na datech bez tohoto filtru výrazně lepší výsledky než stejné metody spuštěné na roztříděných datech, takové zlepšení výkonu je ale jistě umělým důsledkem špatné volby trénovací množiny. 

Ostatní datasety (ADC04, MIREX05, ORCHSET) jsou v práci použity pouze jako testovací data, díky tomu lze korektně výsledky přímo srovnávat s žebříčky úlohy Melody Extraction v soutěži MIREX.

------

- MatthewEntwistle_FairerHopes
obsahuje harfu, ale trénovací data ji neobsahují, chce to ale víc prozkoumat, jelikož trénovací data neobsahují víc nástrojů, tak zjistit přesně přesnost anotace pro tyto nástroje

## Kvalitativní příklady

Pro lepší porozumění hranic testovaných metod je vhodné studovat také výsledky na kvalitativních ukázkách. Modely byly při práci vyhodnocovány na několikaminutových množinách výňatků z validačních a testovacích dat. Metodika výběru spočívala v poslechu nahrávek a ručním hledáním zajímavých hudebních jevů a také v seřazení nahrávek podle úspěšnosti přepisu stávajícími metodami a výběrem výňatků právě z těchto nejproblematičtějších příkladů.

Omezení plynoucí z potřeby zkrátit výňatky na minimum,


## Metriky

Celkovou kvalitu metody pro extrakci melodie určuje její schopnost určit výšku tónu hrající melodie (_odhad výšky melodie_) a také rozpoznat části skladby, které melodii neobsahují (_detekce melodie_). Jelikož jsou tyto podúlohy na sobě nezávislé, standardní sada metrik zahrnuje jak celkové vyhodnocení přesnosti, tak dílčí vyhodnocení pro _odhad výšky_ a _detekci melodie_. 

-------

- můžu zmínit to, že je toto rozdělení důležité pro Orchset, který je z většiny voiced, a tedy overall accuracy může být zavádějící u algoritmů s přísným voicing detection.
- celkové skóre na datasetu je průměr všech písní

### Formát výstupu

Obvyklý formát výstupu algoritmů je CSV soubor se dvěma sloupci. První sloupec obsahuje pravidelné časové značky, druhý sloupec pak odhad základní frekvence melodie. Některé algoritmy uvádí i odhady výšky základní frekvence mimo detekovanou melodii (může jít například o doprovod, který zní i po hlavním melodickém hlasu). Aby tyto odhady byly odlišené od odhadů hlavní melodie, jsou uvedeny v záporných hodnotách. Díky tomu pak lze nezávisle vyhodnotit přesnost _odhadu výšky_ a _detekce melodie_. Odhad výšky se vyhodnocuje podle absolutní hodnoty frekvence ve všech časových oknech, ke kterým existuje anotace, detekce melodie pak na všech hodnotách vyšších než 0. 

### Definice metrik

Většina metrik je definována na základě porovnávání jednotlivých anotačních oken - tedy typicky srovnáním odhadovaných a pravdivých výšek melodie po konstantních časových skocích. Datasety používané pro vyhodnocování v soutěži MIREX používají časový skok délky 10 ms. V definicích budu vycházet ze značení v práci \cite{Salamon2014}. 

    Označme vektor odhadovaných základních frekvencí $\mathbf{f}$ a cílový vektor $\mathbf{f^*}$, složka $f_\tau$ je buď rovna hodnotě $f_0$ melodie nebo $0$, pokud v daném čase melodie nezní. Obdobně zaveďme vektor indikátorů $\mathbf{v}$, jehož prvek na pozici $\tau$ je roven $v_\tau=1$, pokud je v daném časovém okamžiku detekována melodie a $v_\tau = 0$ v opačném případě. Podobným způsobem zavedeme i vektor cílových indikátorů melodického hlasu $\mathbf{v^*}$ a také vektor indikátorů absence melodie $\bar{v}_\tau = 1 - v_\tau$. 

#### "Úplnost detekce" = Voicing Recall rate

Poměr počtu časových oken, které byly správně označené jakožto obsahující melodii, a počtu časových oken doopravdy obsahujících melodii podle anotace.

    $$\mathrm{VR}(\mathbf{v}, \mathbf{v^*}) = \frac{\sum_\tau{v_\tau v^*_\tau}}{\sum_\tau{v^*_\tau}}$$

------

The proportion of frames labeled as melody frames in the ground truth that are estimated as melody frames by the algorithm.

#### "Nesprávné detekce" = Voicing False Alarm rate

Poměr počtu časových oken, které byly nesprávně označené jako melodické, k počtu doopravdy nemelodických oken.

    $$\mathrm{FA}(\mathbf{v}, \mathbf{v^*}) = \frac{\sum_\tau{v_\tau \bar{v}^*_\tau}}{\sum_\tau{\bar{v}^*_\tau}}$$

-------

The proportion of frames labeled as non-melody in the ground truth that are mis- takenly estimated as melody frames by the algorithm.

#### "Přesnost odhadu tónu" = Raw Pitch Accuracy

Poměr správně odhadnutých tónů k celkovému počtu melodických oken. Výška správně určeného tónu se může lišit až o jeden půltón.


    $$\mathrm{RPA}(\mathbf{f}, \mathbf{f^*}) = \frac{\sum_\tau{v^*_\tau v_\tau \mathcal{T}[\mathcal{M}(f_\tau) - \mathcal{M}(f^*_\tau)}] }{\sum_\tau{v^*_\tau}}$$

kde $\mathcal{T}$ je prahová funkce

    \begin{equation*}
        \mathcal{T}[a] = \begin{cases}
                1 & \mathrm{pro} \lvert |a| \le 0.5 \\
                0 & \text{jinak}
                
            \end{cases}
    \end{equation*}

a $\mathcal{M}$ je funkce zobrazující frekvenci $f$ na reálné číslo počtu půltónů od nějakého referenčního tónu $f_{\mathrm{ref}}$ (například od 440 Hz, tedy komorního A4).

    $$\mathcal{M}(f) = 12 \log_2(\frac{f}{f_{\mathrm{ref}}})$$


-------

The proportion of melody frames in the ground truth for which f_τ is considered correct

Raw Pitch Accuracy: The proportion of melody frames in the ground truth for which fτ is considered correct
(i.e. within half a semitone of the ground truth f∗τ ).

#### "Přesnost odhadu tónu nezávisle na oktávě" = Raw Chroma Accuracy

Počítá se podobně jako _Přesnost odhadu tónu_, výstupní a cílové tóny jsou však mapovány na společnou oktávu. Metrika tedy ignoruje chyby odhadu způsobené špatným určením oktávy tónu.

    $$\mathrm{RCA}(\mathbf{f}, \mathbf{f^*}) = \frac{\sum_\tau{v^*_\tau v_\tau \mathcal{T}[\langle \mathcal{M}(f_\tau) - \mathcal{M}(f^*_\tau)} \rangle_{12}] }{\sum_\tau{v^*_\tau}}$$

Nezávislost na oktávě zajistíme pomocí zobrazení rozdílu cílového a výstupního tónu na společnou oktávu.

    $$\langle a \rangle_{12} = a - 12 \lfloor \frac{a}{12} + 0.5 \rfloor  $$

-------

As raw pitch accuracy, except that both the estimated and ground truth f0 sequences are mapped onto a single octave.


#### "Celková přesnost" = Overall Accuracy

Celková přesnost měří výkon algoritmu jak v odhadu melodie tak v detekci melodie. Počítá se jako podíl správně odhadnutých oken a celkového počtu oken.

    $$\mathrm{OA}(\mathbf{f}, \mathbf{f^*}) = \frac{\sum_\tau{v^*_\tau v_\tau \mathcal{T}[\mathcal{M}(f_\tau) - \mathcal{M}(f^*_\tau)}] + \bar{v}^*_\tau \bar{v}_\tau }{L}$$

---------

this measure combines the perfor- mance of the pitch estimation and voicing detection tasks to give an overall performance score for the system.

#### Poznámka k definicím metrik

Definice RPA, RCA a OA zde uvedené se mírně liší od výchozích v práci \cite{Salamon2014}, jejich přímá implementace podle vzorce totiž vede kvůli nedostatečně dobře zadefinovanému vektoru frekvencí $\mathbf{f}$ k chybě, která byla přítomna i v nejpoužívanější, veřejné implementaci MIR metrik _mir\_eval_. Tato chyba se týká zejména metriky RCA, která v původní definici chybně zahrnovala jako správné tóny ty, které algoritmus odhadl jako nulové (tedy neznějící) a zároveň jejich pravdivá hodnota byla po zobrazení na jednu společnou oktávu blízká nule (tedy původní tón byl blízký nějakému násobku referenční frekvence). Kvůli zobrazení na společnou oktávu se stanou "neznělé nulové odhady" a tóny blízké referenčním frekvencím nerozlišitelné a byly nesprávně považované za korektní.

V praxi chyba této metriky na datasetu MedleyDB mohla dosahovat až sedmi procentních bodů, na repozitáři hostovaném na serveru Github jsme již spolu s autory chybu odstranili [1]. Opravný patch bude zahrnut do další verze balíku.

[1] odkaz na Github issue: https://github.com/craffel/mir_eval/issues/311

### Další metriky

Protože princip vnitřního fungování neuronových sítí často není zřejmý, je užitečné mít co nejvíce různých indikátorů, abychom měli při porovnávání jednotlivých modelů alespoň podrobnou informaci, v jakých ohledech se síť zlepšuje nebo zhoršuje. Pro tento účel jsem při práci implementoval další metriky, které při hledání architektur sítí pomáhaly.

#### Chroma Overall Accuracy

Počítá se obdobně jako Overall Accuracy, ale tóny jsou mapovány na společnou oktávu.

#### Raw Harmonic Accuracy

Metrika počítá odhadovaný tón jako správný, pokud se trefil do některé z harmonických frekvencí tónu. Protože je harmonických frekvencí teoreticky nekonečné množství, parametrem metriky je do jakého celočíselného násobku se ještě odhad počítá.

    $$\mathrm{RHA}(\mathbf{f}, \mathbf{f^*}, n) = \frac{\sum_{k=1}^n \sum_\tau{v^*_\tau v_\tau \mathcal{T}[\mathcal{M}(f_\tau) - \mathcal{M}(k f^*_\tau)} ] }{\sum_\tau{v^*_\tau}}$$

#### Matice záměn not

Pro podrobnější souhrnný přehled četností chyb se pro klasifikační úlohy používá matice záměn. Sloupce označují správné noty, řádky odhadované. Buňka na pozici $(x,y)$ má pak hodnotu podle četnosti odhadu noty $y$ místo správné noty $x$.

#### Histogram vzdáleností odhadu

Histogram hodnot rozdílu $\mathbf{f} - \mathbf{f^*}$, 

- confusion matrix
- estimation distance histogram
- pitch accuracy per note

### Limitace základních metrik
- limitace jsou předvedeny v onsets+frames
    - nakonec nejsou, tam kritizují jenom 
- například je otázka, jestli jsou všechny framy stejně důležité - zejména u vybrnkávání, piana, perkusí je otázka, kdy ještě anotovat, tedy jsou tam sporné konce. Na small_valid je to hodně vidět na té harfě
- nijak se nepenalizuje nekontinualita výstupů, je rozdíl mezi 50% accuracy, kde je zbytek unvoiced a 50% accuracy, kde odhady strašně skáčou

- Bosch metrics \cite{Bosch2016}
    - Weighted Raw Chroma accuracy - počítá vzdálenost v oktávách
    - Octave Jumps - vyjadřuje skokovitost o oktávy v po sobě následujících framech v rámci správných chroma odhadů
    - Chroma continuity - 

# Kvalitativní
- popsat můj small_validation

- ilustrační příklady !!!
	- orchestrální i neorchestrální
		- metody z related work fungují na neorch.
	- jeden hlas
	- melodie nahoře (zkusit vybrat extrém ~ np.max(annotations))
	- melodie dole
	- melodie uprostřed

	- vlastnosti melodie
		- stabilní dlouhý tóny (a kolem doprovod)
		- něco proměnlivého
	- potichu/nahlas
