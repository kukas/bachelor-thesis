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
| Datasety    | 80%  | 20%     | 0%      |
| Související | 55%  | 0%      | 0%      |
| Evaluace    | 75%  | 0%      | 0%      |
| Experimenty | 60%  | 50%     | 50%     |
| Výsledky    | 10%  | 0%      | 0%      |
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
- úvod
    - ! opravit diagram metod
    - ! přínosy metod
    - definice pojmů
        - monofonní v této práci je jednohlas, nikoli jeden kanál!

- related work
    - obecně: nebát se zmínit limitace metod, to jsou doopravdy východiska pro další práce
        - kdyby nebyly limitace, tak by nebylo co dělat
    - strojové učení pro funkci salience !!!!!!
    - (Durrieu)
    - hledání melodie !!
    - přítomnost melodie

    - co se umí (a jak dobře)
        - srovnání metod v rámci MIREXu
        - srovnání metod mimo MIREX (nad MedleyDB)
            - mám Salamona, Bittnerovou, Basarana
        - replication
            - popsat co jsem spustil, a že jsem dosáhl stejných výsledků
            - Durrieu byl k smrti pomalý a ani neházel tak dobré výsledky
                - běží absurdně dlouho a podařilo se mi zpracovat jen Orchset (23 minut za dva dny výpočtu)
            - Bosch nešel spustit kvůli Essentie, nepodařilo se mi to opravit
                - Bosch to opravil sám! Když zbyde čas, tak ho můžu vyhodnotit!

    - (krátký popis CREPE a Wavenetu, protože je používám v experimentech)
    - taky popis neuronových sítí, convnetů, asi prostě bohužel všeho, co používám v experimentech

- datasety
    "datasety mají mnohem delší poločas rozpadu, jejich pochopení je zásadní k interpretaci výsledků, jejich vznik formuje směr, kterým se výzkum ubírá"
    čtenář musí pochopit, na čem se evalkuuje, co znamenají výsledky
    - dopsat pasáž o MDB-synth
    - dopsat úvod
    - dopsat wjazzd
    - napsat krátký popis MIREX dat
    - tabulka!
        - hodit ji na konec úvodu a před ní ještě vysvětlit jednotlivé sloupce
    - tabulka s přehledem počtu a délky train, validačních a test dat

    - pod každou kapitolu napsat, jakým způsobem je dataset používaný v práci

    - možná kapitola o tom, jaké různé postupy vytváření dat existují (zvlášť zahrnutí multif0 postupů)

- experimenty
    - popsat Prostředí pro spouštění, replikaci a evaluaci experimentů
        - viz `experimenty.md`

    - Salience function
        - CREPE - překopat ještě asi dvě sekce, co jsem nepřepsal, jen trochu upravit, ať se to dá číst
            - přidat obrázek první vrstvy? Je to vděčný podle mě
        - WaveNet - je to naprd, ale asi skoro hotový - chybí popis poslední vrstvy
        - HCNN - !!!!!!
    - Voicing detection
        - řada mých experimentů, mám k tomu fůru dat, stačí shrnout a popsat zajímavé příklady

- evaluation
    - dopsat zbytek metrik
    - příkladové obrázky jednotlivých chyb
    - popsat malé datasety

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
