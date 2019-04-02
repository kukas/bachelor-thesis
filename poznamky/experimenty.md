obecně zmínit:
- je potřeba řešit shuffle, data se nevejdou do RAMky

# Experimenty

## Prostředí pro spouštění, replikaci a evaluaci experimentů

moje prostředí:
- správa dat
    - jednotný přístup k datasetům
        - ať už jsou multif0 nebo melody
    - možnost data ze zdrojů třídit na množiny
    - snadné vytváření výňatků pro 
        - jednotné pro vstup jako raw audio nebo jako spektrogramy
        - automaticky se vytvoří i anotace k výňatku, na základě původní
    - preprocessing
        - automatický resampling
            - audia i anotací
        - generování spektrogramů
        - obojí paralelizováno a cachováno
    - transformace dat do nastavitelných oken a batchí
        - překrývání oken, nastavitelné skoky, padding na koncích a začátcích nahrávek
    - (zatím skoro ne): augmentace dat a filtrování dat (podle voiced např, pro ne-voicing experimenty)

- model
    - soubor pomocných funkcí pro sestavování experimentů
        - normalizace vstupu
        - reprezentace referenční anotace
            - one-hot vektor
            - gaussovské rozmazání one-hot vektoru
        - loss - buď automaticky nebo pomocí přepínačů přičte
            - voicing
            - miss_weight
            - l2 regularization
        - ukládání nejlepších modelů
        - pravidelná evaluace a vkládání vizualizací do tensorboardu
    - struktura experimentů - trénovací smyčka, evaluační funkce, zavedení vstupů a komponent společných pro všechny experimenty
    - důraz na neopakování se v jednotlivých experimentech
    - důraz na možnost replikace a porovnání modelů
        - pro každý model je uložena jeho sestavující funkce a parametry spuštění
        - všechny experimenty by mělo být možné replikovat spuštěním souboru se správnými parametry

- evaluace
    - implementace nových kvantitativních metrik
    - stejná implementace finální evaluace pro stávající metody a pro moje
        - důležité pro kontrolu a replikaci (csvčka se nezmění, ostatní věci ano)!

- vizualizace
    - vizualizace kvalitativních i kvantitativních výsledků
    - pro použití do juypter notebooků i pro vložení do tensorboardu
    - pianoroll s vizualizací referenční anotace a odhadů
        - barevně odlišené chyby, správné odhady, chyby o oktávu, voicing chyby
        - spolu s tím i zobrazení výstupních pravděpodobností
    - confusion matrix
    - histogramy chyb

## Provedené experimenty

Práce obsahuje souhrnné výsledky experimentů zejména nad datasetem MedleyDB, aby modely byly dobře porovnatelné se state-of-the-art výsledky a výhoda prezentovaných metod netkvěla pouze v použití více dat. U vybraných experimentů došlo k přetrénování na větší trénovací množině, aby bylo možné posoudit vliv množství dat na výsledný výkon.

### CREPE

První sada experimentů se zakládá na architektuře popsané v článku \cite{Kim2018} použité pro *monopitch tracking*. Přestože se nejedná o úlohu extrakce melodie, cílem monopitch trackingu je určit konturu základní frekvence melodického nástroje v monofonní nahrávce, která se skládá ze součtu čistého signálu a šumu v pozadí. Pokud rozšíříme pojem šumu v pozadí tak, aby zahrnoval i melodický doprovod, pak dostáváme formální definici signálu zpracovávaného algoritmy pro přepis melodie \cite{Salamon2014}.

Jinými slovy - *monopitch tracking* je speciálním případem extrakce melodie a tudíž přinejmenším stojí za zkoušku pokusit se tuto architekturu pro extrakci využít. Mimo to monofonní stopy často obsahují přeslech ostatních nástrojů, pokud nahrávka vznikala při společném hraní, tudíž by model trénovaný na výsledných mixech mohl být robustní vůči tomuto druhu rušení. 

Architektura CREPE se sestává ze šesti konvolučních a pooling vrstev, pro regularizaci používá batch normalization a dropout po každé konvoluční vrstvě, jako aktivační funkce se používá ReLU. Po konvolucích následuje plně propojená vrstva se sigmoid aktivací. Vstupem modelu je okno o velikosti 1024 samplů, audio je převzorkovano na 16 kHz. Před první konvolucí je vstup normalizován tak, aby každé jednotlivé okno se vzorky mělo střední hodnotu 0 a směrodatnou odchylku 1.

Výsledný vektor délky 640 aproximuje pravděpodobnostní rozdělení výšky základní frekvence uprostřed vstupního okna, přičemž tento vektor pokrývá rozsah od noty $C_{-1}$ po $G_{9}$, mezi dvěma sousedními predikovanými tóny je vzdálenost 20 centů. Rozsah tedy bezpečně pokrývá obvyklé hudební nástroje a na jednu notu připadá 5 složek (tónů) výsledného vektoru.

Referenční pravděpodobnostní vektor $\mathbf{y}$ 

Optimalizovaná loss funkce modelu $\mathcal{L}(\mathbf{y}, \mathbf{\hat{y}})$ se počítá jako binární vzájemná korelace mezi vektorem referenčních pravděpodobností $y$ a výstupním vektorem $\hat{y}$.

    $$\mathcal{L}(\mathbf{y}, \mathbf{\hat{y}}) = \sum_{i = 0}^{640}{(-y_i\log\hat{y}_i - (1-y_i)\log(1-\hat{y_i}))}$$


rozepsat:
- Důvodem pro toto vyšší frekvenční rozlišení je, že nástroje a zejména lidský hlas se často při hraní odchylují od přesných frekvencí not a vyšší rozlišení tyto odchylky může lépe zachytit. 
- obhajoba raw signálu


#### Replikace CREPE

Pro ověření správnosti vlastní implementace architektury *monopitch trackeru CREPE* jsem spustil svůj model na syntetických, monofonních datech používaných v článku (*mdb-stem-synth*). Na rozdíl od článku jsem model netrénoval na všech datech pomocí postupu *5 fold cross validation*, jiné zásadní rozdíly mezi implementacemi jsem však na základě článku a veřejně dostupného kódu neidentifikoval.

Po jedné epoše trénování model dosáhl vyšší přesnosti, než je uváděná v literatuře, tento rozdíl přičítám zejména zmiňované odlišné evaluační strategii.

Metrika | Průměrná přesnost
--- | ---
Raw Pitch Accuracy             |0.986444
Raw Chroma Accuracy            |0.988044
Raw Pitch Accuracy 25 cent     |0.974920
Raw Chroma Accuracy 25 cent    |0.973362
Raw Pitch Accuracy 10 cent     |0.936620
Raw Chroma Accuracy 10 cent    |0.935243


# CREPE replication
- dodal jsem tam L2 regularizaci - popsat proč a jak to pomohlo
    - nakonec to vypadá, že to zas tak nepomáhá, problémy vyřešil správný shuffle

- data mdb-stem-synth, neudělal jsem 5 fold cross validation, použil jsem pevně daný split podle splitu z ISMIR2017, jelikož jsem měl stem data připravená pro melody experimenty
- původně se mi nedařilo replikovat výsledky
    - špatně zamíchaná data
    - granularita vstupu a výstupu dělá hodně - model se pak plete o půltóny
        - RPA <98% vs >99%
- jakmile jsem opravil nedostatky, je naopak těžké se nedostat na 99%
    - předpokládám, že 5 fold cross validation to stáhne dolů na jejich čísla
    - šlo by udělat větší rozdíly pomocí přísnější evaluace (hranice správné anotace +-50 centů vs +-10 centů)
        - o tom ale bakalářka není, takže uzavírám s tím, že pokud se nějaký model neumí dostat nad 98%, tak je s ním něco špatně


# autocorrelation
- jako FFT - ale vytváří to jenom strašně chaotické predikce (zkusit to znovu a udělat obrázek)
- zjistit, jestli to doopravdy je stejný jako FFT

# Baseline perceptron

- pro ukázku, že úloha nelze vyřešit jednoduchým modelem, ale také jako fajn baseline, který by snad mělo být snadné překonat
- takhle jednoduchý model se umí zaseknout na triviálním předpovídání veškerých frames jako unvoiced, celkem logicky, protože je nejvyšší pravděpodobnost, že se s takovou předpovědí často trefí
    - tahle vlastnost je ale společná i pro větší modely, proto je potřeba zvážit oddělení voicingu a transkripce
    - případně mě napadá exponenciální decay pro váhy unvoiced framů
        - zkoušel jsem to u jednoduchých modelů a celkem očekávatelně se síť naučí zapomínat přepis. 

# CREPE for Melody Extraction
- pokus bez voicingu
- !! není granulární výstup, což značně ovlivňuje výsledky !!

- vzal jsem architekturu CREPE a zkusil ji na extrakci melodie
    - rozdíl je, že síť odhaduje přímo tóny (softmax classification), ne distribuci
    - přidal jsem L2 regularizaci a gradient clipping, obojí pomáhalo
    - vstup je normalizován, podle paperu
- trénováno na mdb_melody_synth, na validačních datech jsem dosáhl v nejlepším případě

- měnil jsem šířku okna, vyšší zhoršovala výkon
- a kapacitu sítě, vyšší pomáhala, ať už s úzkým nebo širokým oknem
- nejlepší výsledek crepe_16mult_normalized-01-25_162034-bs32-apw1-fw93-ctx466-nr128-sr16000
    - šířka okna 1024 (jako v paperu), tedy 0.064 sekundy
    - dosáhl RCA 62%, RPA 54% na MDB-melody-synth vs:
        - Salamon: 54%, 50%
        - Bittner: 79%, 76%
        - Basaran: 87%, 86%
    - tedy dokážu docela snadno porazit Salamona


## CREPE s více daty
- asi bych to měl přetrénovat, protože data nebyla pořádně zamíchaná

## CREPE s rekonstrukcí
- asi bych to měl přetrénovat, protože data nebyla pořádně zamíchaná
- bottleneck architektura, z prostředka se dělá melody extraction
- možná by to mohlo pomoct, výstupy sítě nejsou tak sebejistý jako u předchozího pokusu, což mi přijde fajn - funguje to asi trochu jako druh regularizace

## CREPE bez granulárního výstupu

nejlepší výsledky na MedleyDB validačních datech po šesti epochách

0302_204351-crepe-dmdb-bs32-apw1-fw93-cw466-s16000-inTrue-lr0.0002-cm4-cg0.0-llw0.0-mc0-bps1-as0.0-mw1.0-vsFalse-flc1
    Raw Pitch Accuracy,0.5744710174010863
    Raw Chroma Accuracy,0.6979197393378155
    Total parameter count: 808496

0302_222616-crepe-dmdb-bs32-apw1-fw93-cw466-s16000-inTrue-lr0.0002-cm8-cg0.0-llw0.0-mc0-bps1-as0.0-mw1.0-vsFalse-flc1
    Raw Pitch Accuracy,0.597870447188386
    Raw Chroma Accuracy,0.7039810520266852
    Total parameter count: 3035104

0303_004825-crepe-dmdb-bs32-apw1-fw93-cw466-s16000-inTrue-lr0.0002-cm16-cg0.0-llw0.0-mc0-bps1-as0.0-mw1.0-vsFalse-flc1
    Raw Pitch Accuracy,0.6065284333418696
    Raw Chroma Accuracy,0.7091437352118173
    Total parameter count: 11743040

## CREPE s granulárními výstupy
trénovány pět epoch

### Šířka okna
obecně širší okno potřebuje víc času na natrénování (a taky trénování trvá déle)
experimenty:
0226_233744-crepe-dmdb,orchset-bs32-apw1-fw93-cw210-s16000-inTrue-lr0.0002-cm8-cg0.0-llw0.0-mc0-bps5-as0.25
Raw Pitch Accuracy,0.6368829881474476
Raw Chroma Accuracy,0.7493289984470894

0227_003017-crepe-dmdb,orchset-bs32-apw1-fw93-cw466-s16000-inTrue-lr0.0002-cm8-cg0.0-llw0.0-mc0-bps5-as0.25
Raw Pitch Accuracy,0.6568594554411157
Raw Chroma Accuracy,0.761003923554659

0227_013427-crepe-dmdb,orchset-bs32-apw1-fw93-cw978-s16000-inTrue-lr0.0002-cm8-cg0.0-llw0.0-mc0-bps5-as0.25
Raw Pitch Accuracy,0.6598948339848353
Raw Chroma Accuracy,0.7670859514272944

0227_025011-crepe-dmdb,orchset-bs32-apw1-fw93-cw2002-s16000-inTrue-lr0.0002-cm8-cg0.0-llw0.0-mc0-bps5-as0.25
Raw Pitch Accuracy,0.6645687961957129
Raw Chroma Accuracy,0.7688330561778753

0227_045758-crepe-dmdb,orchset-bs32-apw1-fw93-cw4050-s16000-inTrue-lr0.0002-cm8-cg0.0-llw0.0-mc0-bps5-as0.25
Raw Pitch Accuracy,0.6751112990020703
Raw Chroma Accuracy,0.7749715186497634


### Multiresolution first layer
experimenty:
0227_003017-crepe-dmdb,orchset-bs32-apw1-fw93-cw466-s16000-inTrue-lr0.0002-cm8-cg0.0-llw0.0-mc0-bps5-as0.25
0227_085314-crepe-dmdb,orchset-bs32-apw1-fw93-cw466-s16000-inTrue-lr0.0002-cm8-cg0.0-llw0.0-mc2-bps5-as0.25
0227_094505-crepe-dmdb,orchset-bs32-apw1-fw93-cw466-s16000-inTrue-lr0.0002-cm8-cg0.0-llw0.0-mc3-bps5-as0.25
0227_103748-crepe-dmdb,orchset-bs32-apw1-fw93-cw466-s16000-inTrue-lr0.0002-cm8-cg0.0-llw0.0-mc4-bps5-as0.25


## Bittner

- příklad overlapping window - jak to dělá mezery v pravděpodobnostech

http://jirkabalhar.cz:6089/data/plugin/images/individualImage?ts=1552930689.3933728&run=0318_161357-bittner_batchnorm-dmdb-bs16-s22050-hs10-fw256-cw0-inTrue-apw50-mn24-bps5-as0.25-lr0.0005-cg0.0-llw0.0-mw1.0-scqt&tag=valid_small_mdb%2Fnotes&sample=0&index=9

http://jirkabalhar.cz:6089/data/plugin/images/individualImage?ts=1553075432.3351898&run=0320_062237-cqt_resid_dropout-dmdb-bs16-s22050-hs10-fw256-cw2560-inTrue-apw20-mn24-bps5-as0.25-lr0.001-cg0.0-llw0.0-mw1.0-scqt&tag=valid_small_mdb%2Fnotes&sample=0&index=5



## Wavenet

- Dropout
    - protože valid accuracy rychle vyskočí a pak jen padá

- kapacita
    - skip kapacita
    - residual kapacita



0303_120740-wavenet-dmdb-bs32-apw1-fw93-cw944-s16000-inTrue-lr0.0002-cm16-cg0.0-llw0.0-mc0-bps5-as0.25-mw1.0-vsFalse-flc1
    - stack 1,2...512
    - initial_width=2
    - residual_channels=64
0304_013344-wavenet-dmdb-bs32-apw1-fw93-cw944-s16000-inTrue-lr0.0002-cm16-cg0.0-llw0.0-mc0-bps5-as0.25-mw1.0-vsFalse-flc1
    -> přidání l2 loss na všech vrstvách (biasy i kernely)
        - reálně jsem ale zapomněl nastavit l2loss weight, takže stejný jako minulý
0304_131800-wavenet-dmdb-bs8-apw1-fw93-cw944-s16000-inTrue-lr0.0002-cm16-cg0.0-llw0.0-mc0-bps5-as0.25-mw1.0-vsFalse-flc1
    -> dilatované konvoluce s šířkou 3
0304_170035-wavenet-dmdb-bs8-apw1-fw93-cw944-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw3-ubTrue-sc32-rc16-sn1-md512
    -> zvětšení initial width na 32
    -> zmenšení kapacity na skip_channels=32 a residual_channels=16
0304_183901-wavenet-dmdb-bs8-apw1-fw93-cw944-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw3-ubTrue-sc32-rc16-sn1-md512-dld0.5
    -> dilation_layer_dropout 0.5
        - dropout za dilační vrstvou, ale před residuálním sčítáním
0305_001319-wavenet-dmdb-bs10-apw1-fw93-cw944-s16000-inTrue-lr0.001-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc128-rc32-sn1-md512-dld0.0-sld0.25
    -> skip layer dropout 0.25
    -> větší skiplayer channels, residual channels
0305_082854-wavenet-dmdb-bs8-apw22-fw93-cw1-s16000-inTrue-lr0.001-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc32-sn1-md512-dld0.0-sld0.0
    -> větší okno - apw = 22, kontext 1
    -> menší skiplayer channels = 64
    -> averagepool po sečtení skipů pro jednotlivá okna
        - díky tomu na konci neni fc layer, ale výstup jde rovnou z konvolucí
0305_105410-wavenet-dmdb,wjazzd-bs4-apw44-hsNone-fw93-cw2-s16000-inTrue-lr0.001-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc16-sn2-md1024-dld0.0-sld0.0
    -> ještě větší okno - apw = 44
0305_140352-wavenet-dmdb,wjazzd-bs4-apw44-hsNone-fw93-cw2-s16000-inTrue-lr0.001-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw3-ubTrue-sc64-rc16-sn2-md1024-dld0.0-sld0.0
0306_031838-wavenet-dmdb-bs32-apw1-hsNone-fw93-cw944-s16000-inTrue-lr0.001-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0
    -> skip - averagepool93

## Vliv batch size
0306_123242-wavenet-dmdb-bs32-apw1-hsNone-fw93-cw944-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw2-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu
    -> replikace prvního experimentu
0306_162054-wavenet-dmdb-bs4-apw1-hsNone-fw93-cw944-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw2-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu
0329_120345-wavenet-dmdb-bs4-s16000-hsNone-fw93-cw944-inTrue-apw1-mn0-bps5-as0.25-lr0.0002-cg0.0-llw0.0-mw1.0-ifw2-ifpvalid-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu



    úplnej propadák
    0306_183921-wavenet-dmdb-bs4-apw8-hs1-fw93-cw652-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw2-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f640_k32_s16_Psame

## Vliv počtu predikovaných oken
    - snad by nemělo mít vliv a pokud nebude mít, může to zrychlit výpočty
0306_162054-wavenet-dmdb-bs4-apw1-hsNone-fw93-cw944-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw2-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu
0306_191954-wavenet-dmdb-bs4-apw2-hs1-fw93-cw931-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw2-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu
0306_210838-wavenet-dmdb-bs4-apw4-hs1-fw93-cw838-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw2-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu
0306_225743-wavenet-dmdb-bs4-apw8-hs1-fw93-cw652-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu

    0307_090719-wavenet-dmdb-bs4-apw4-hs2-fw93-cw838-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pavgpool_p4_s4_Psame->conv_f256_k32_s4_Psame_arelu->conv_f256_k32_s16_Psame_arelu
0307_140856-wavenet-dmdb-bs4-apw4-hs1-fw93-cw838-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k64_s16_Psame_arelu->conv_f256_k64_s16_Psame_arelu

0307_103349-wavenet-dmdb-bs4-apw4-hs2-fw93-cw838-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn2-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu


## vliv velikosti prvního filtru
0307_214147-wavenet-dmdb-bs4-apw4-hs1-fw93-cw838-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw2-fw2-ubTrue-sc128-rc128-sn1-md512-dld0.0-sld0.0-pconv_f256_k128_s16_Psame_arelu->conv_f256_k128_s16_Psame_arelu
0309_204420-wavenet-dmdb-bs4-apw4-hs1-fw93-cw838-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc128-rc128-sn1-md512-dld0.0-sld0.0-pconv_f256_k128_s16_Psame_arelu->conv_f256_k128_s16_Psame_arelu

0306_210838-wavenet-dmdb-bs4-apw4-hs1-fw93-cw838-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw2-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu
0328_062000-wavenet-dmdb-bs4-s16000-hs1-fw93-cw838-inTrue-apw4-mn0-bps5-as0.25-lr0.0002-cg0.0-llw0.0-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu

## vliv kapacity - skip kapacita a residual kapacita

0310_003614-wavenet-dmdb-bs4-apw4-hs1-fw93-cw1862-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc32-rc32-sn3-md512-dld0.0-sld0.0-pconv_f256_k128_s16_Psame_arelu->conv_f256_k128_s16_Psame_arelu
0310_055905-wavenet-dmdb-bs4-apw4-hs1-fw93-cw1862-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn3-md512-dld0.0-sld0.0-pconv_f256_k128_s16_Psame_arelu->conv_f256_k128_s16_Psame_arelu
0310_223331-wavenet-dmdb-bs4-apw4-hs1-fw93-cw1862-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc128-rc128-sn3-md512-dld0.0-sld0.0-pconv_f256_k128_s16_Psame_arelu->conv_f256_k128_s16_Psame_arelu


vliv počtu residual stacků

0307_103349-wavenet-dmdb-bs4-apw4-hs2-fw93-cw838-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn2-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu
0306_210838-wavenet-dmdb-bs4-apw4-hs1-fw93-cw838-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw2-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu
0328_062000-wavenet-dmdb-bs4-s16000-hs1-fw93-cw838-inTrue-apw4-mn0-bps5-as0.25-lr0.0002-cg0.0-llw0.0-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu

- nefér v první vrstvě...


vliv velikosti kernelu na výstupu

0307_002832-wavenet-dmdb-bs4-apw8-hs2-fw93-cw652-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f640_k32_s16_Psame_arelu->conv_f640_k32_s16_Psame
0307_022006-wavenet-dmdb-bs4-apw8-hs2-fw93-cw652-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f640_k64_s16_Psame_arelu->conv_f640_k64_s16_Psame
0307_140856-wavenet-dmdb-bs4-apw4-hs1-fw93-cw838-s16000-inTrue-lr0.0002-cg0.0-llw0.0-bps5-as0.25-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k64_s16_Psame_arelu->conv_f256_k64_s16_Psame_arelu
0328_062000-wavenet-dmdb-bs4-s16000-hs1-fw93-cw838-inTrue-apw4-mn0-bps5-as0.25-lr0.0002-cg0.0-llw0.0-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu

- taky nefér v první vrstvě...

0328_062000-wavenet-dmdb-bs4-s16000-hs1-fw93-cw838-inTrue-apw4-mn0-bps5-as0.25-lr0.0002-cg0.0-llw0.0-mw1.0-ifw32-fw2-ubTrue-sc64-rc64-sn1-md512-dld0.0-sld0.0-pconv_f256_k32_s16_Psame_arelu->conv_f256_k32_s16_Psame_arelu