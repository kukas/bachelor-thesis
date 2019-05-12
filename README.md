# Bakalářská práce

## TODO do příští sch

dnes:
    - popsat voicing
        - přetrénovat s HCNN výsledky
    - výsledky
    - hcnn
    - večer dát echo Hajičovi

pátek: 

- výsledky
- github
    - readme tak, abych mohl navzázet za 5 let
    - příkladové skripty

## Progress

| Kapitola    | Text | Tabulky | Obrázky |
| ----------- | ---- | ------- | ------- |
| Úvod        | 90%  | -       | 100%    |
| Datasety    | 99%  | 100%    | --      |
| Související | 99%  | --      | 100%    |
| Evaluace    | 99%  | 100%    | 100%    |
| Experimenty | 60%  | 50%     | 50%     |
| Výsledky    | 50%  | 0%      | 0%      |
| Závěr       | 0%   | 0%      | 0%      |

Typografické dodělávky, lahůdky a drobnosti:
- opravit skloňování citací
- přidat nezalomitelné mezery
- zbavit se černých obdélníčků
- F0 nebo F_0?

- obrázky barevně!

- buď, nebo = čárka!
- projít kurzívy

- Konzistentní terminologie
    - rozdělení - dat
    - distribuce - pravděpodobnostní
    - monofonní=jednohlas, jednokanálový zvuk píšu jen česky


## Písemná část
- příloha
    - audio příklady
    - splits v csv souboru
        - pomocí http://10.0.0.42:6088/notebooks/bakalarka/datasety/dataset_info_summaries.ipynb 


- úvod
    - ! opravit diagram metod
    - ! přínosy metod
    - definice pojmů
        - monofonní v této práci je jednohlas, nikoli jeden kanál!
        - harmonické subharmonické

- related work
    - (Durrieu)
    - (krátký popis CREPE a Wavenetu, protože je používám v experimentech)
    - taky popis neuronových sítí, convnetů, asi prostě bohužel všeho, co používám v experimentech

- datasety
    "datasety mají mnohem delší poločas rozpadu, jejich pochopení je zásadní k interpretaci výsledků, jejich vznik formuje směr, kterým se výzkum ubírá"
    čtenář musí pochopit, na čem se evalkuuje, co znamenají výsledky
    - tabulka s přehledem počtu a délky train, validačních a test dat

    - (možná kapitola o tom, jaké různé postupy vytváření dat existují (zvlášť zahrnutí multif0 postupů))

- evaluation
    - dopsat zbytek metrik
    - příkladové obrázky jednotlivých chyb
    - popsat malé datasety

- experimenty
    - popsat co je hluboké učení
    - popsat Prostředí pro spouštění, replikaci a evaluaci experimentů
        - viz `experimenty.md`

    - Salience function
        - CREPE - překopat ještě asi dvě sekce, co jsem nepřepsal, jen trochu upravit, ať se to dá číst
            - přidat obrázek první vrstvy? Je to vděčný podle mě
        - WaveNet - je to naprd, ale asi skoro hotový - chybí popis poslední vrstvy
        - HCNN - !!!!!!
    - Voicing detection
        - řada mých experimentů, mám k tomu fůru dat, stačí shrnout a popsat zajímavé příklady

- výsledky
    - Pitch Estimation
        - tabulka - kvantitativní
        - kvalitativní příklady
    - Voicing
        - tabulka s celkovýma výsledkama

## Experimenty

- problém s voicingem
    - proč se mi nedaří replikovat voicing od Bittnerové?
        - nevim
    - spustit můj nejlepší voicing modul spolu s nějkaým dobrým modelem
        - srovnání, když má voicing jen spektrogram a když má spektrogram+výstup melody modelu

- HCNN
    - dilatované konvoluce
    - zkusit odebrat residuální propojení ke konci, aby síť mohla vyhladit výstup
    - zkusit residualní udělat jednou za dvě vrstvy
    - zkusit žonglovat s pořadim resnetových bloků, batchnormu, aktivace, relu
    - jak to udělat širší a zrychlit to - harmonic stacking jen každou n-tou vrstvu, mezitím můžou být konvoluce hodně široké a na konci naopak bottleneck, protože se bude stackovat a narostl by počet kanálů na nechutný množství

- zkusit ještě jednou Durrieu s menším počtem iterací
    - čerpat správné hyperparametry z MIREX2009, Bosch githubu, ..?
    - Možná zkusit Bosch github implementaci Durrieu (případně to forknul ještě někdo jinej)
    - Bosch to opravil

- (tone tracking)
    - tak tam dát alespoň Viterbi decoding s ručně nastavenými pravděpodobnostmi přechodu (tak jak to mají třeba Ikemia, CREPE, další..)
    - LSTM
        - je tam nějaká pitomá chyba a neučí se to, nevim proč a je to k vzteku
    - tady se dost možná vyplatí i zmenšit penalizaci sítě, když odhadne jinou melodickou linku
        - použít def3 z MedleyDB (multimelody) a linky mimo hlavní také zvýraznit
        - případně dvouúrovňový model multimelody -> melody
        - případně label smoothing tf.losses.sigmoid_cross_entropy, focal loss
    - kontext - dilated konvoluce https://towardsdatascience.com/review-drn-dilated-residual-networks-image-classification-semantic-segmentation-d527e1a8fb5

## Programování

- metrics
    - n-peak accuracy
    - output noise (sum of output/sum of reference output)
- early stopping
