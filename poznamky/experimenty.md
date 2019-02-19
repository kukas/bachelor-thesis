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