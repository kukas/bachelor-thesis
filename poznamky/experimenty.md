obecně zmínit:
- je potřeba řešit shuffle, data se nevejdou do RAMky

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