# Bakalářská práce

## TODO do odevzdání

APPKA
- asi dát Flask do containeru
- jako alespoň dát načítací kolečka do apliakce
- chyba s orchsetem
Traceback (most recent call last):
  File "spectrogram.py", line 265, in <module>
    common.main(sys.argv[1:], construct, parse_args)
  File "/home/jirka/music-transcription/common.py", line 520, in main
    network, train_dataset, validation_datasets, test_datasets = construct(args)
  File "spectrogram.py", line 251, in construct
    train_dataset, test_datasets, validation_datasets = common.prepare_datasets(args.datasets, args, preload_fn, dataset_transform, dataset_transform_train)
  File "/home/jirka/music-transcription/common.py", line 477, in prepare_datasets
    orchset_test_dataset = datasets.AADataset(orchset_test, args, dataset_transform)
  File "/home/jirka/music-transcription/datasets/dataset.py", line 43, in __init__
    raise RuntimeError("Window size is bigger than the audio.")
RuntimeError: Window size is bigger than the audio.
- github
    - readme tak, abych mohl navzázet za 5 let
    - příkladové skripty
    - skripty pro replikaci čísel

- killall nvidia-smi (takže budu moct trénovat experimenty)

## Progress

| Kapitola    | Text | Tabulky | Obrázky |
| ----------- | ---- | ------- | ------- |
| Úvod        | 90%  | -       | 100%    |
| Datasety    | 99%  | 100%    | --      |
| Související | 99%  | --      | 100%    |
| Evaluace    | 99%  | 100%    | 100%    |
| Experimenty | 90%  | 100%    | 80%     |
| Výsledky    | 80%  | 0%      | 0%      |
| Závěr       | 0%   | 0%      | 0%      |

Typografické dodělávky, lahůdky a drobnosti:
- ! zkontrolovat, jestli budou dva spektrogramy v úvodu na dvou různých stránkách
- opravit skloňování citací
- přidat nezalomitelné mezery
- zbavit se černých obdélníčků
- F0 nebo F_0?
- seznam zkratek!
- footnote za interpunkcí
- obrázky barevně!

- buď, nebo = čárka!
- projít kurzívy

- Konzistentní terminologie
    - rozdělení - dat
    - distribuce - pravděpodobnostní
    - monofonní=jednohlas, jednokanálový zvuk píšu jen česky


POKUD ODSTRANIM VOICING:
uvod.tex, 130 -  V této práci se pokusíme navázat na zmiňované práce a otestovat nové architektury hlubokých neuronových sítí pro úlohu extrakce melodie, zejména pak pro hledání nových způsobů výpočtu funkce salience, v menší míře také pro detekci melodie.



## Písemná část
- příloha
    - audio příklady
    - splits v csv souboru
        - pomocí http://10.0.0.42:6088/notebooks/bakalarka/datasety/dataset_info_summaries.ipynb 

- úvod
    - ! přínosy metod
    - definice pojmů
        - monofonní v této práci je jednohlas, nikoli jeden kanál!
        - harmonické subharmonické

- related work
    - (Durrieu)
    - (krátký popis CREPE a Wavenetu, protože je používám v experimentech)
    - ! popsat Hannovo okno
    - ! opravit barvy v obrázku 2.4

    - taky popis neuronových sítí, convnetů, asi prostě bohužel všeho, co používám v experimentech
        - architektura
            = topologie
        - hyperparametr
        - kapacita sítě
        - konvoluční vrstva
            - filtry
            - dilatace
            - recepční pole
                - recepční pole někkolika vrtev za esbou
        - pooling vrstva
        - dropout vrstva
        - batchnorm

- datasety
    "datasety mají mnohem delší poločas rozpadu, jejich pochopení je zásadní k interpretaci výsledků, jejich vznik formuje směr, kterým se výzkum ubírá"
    čtenář musí pochopit, na čem se evalkuuje, co znamenají výsledky
    - (možná kapitola o tom, jaké různé postupy vytváření dat existují (zvlášť zahrnutí multif0 postupů))

- evaluation
    - ! splity do přílohy !
    - přesunout formát z datasetů

- experimenty
    - ! zlepšit obrázek CREPE
    - víc obrázků! K experimentům
    - learning rate decay?
    - dopsat minishrnutí

- výsledky

- závěr
    - zopakovat přínosy
    - jak navázat

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

- specaugment ale uvnitř sítě??

## Programování

- metrics
    - n-peak accuracy
    - output noise (sum of output/sum of reference output)
- early stopping
- opravit training RPA a RCA, nějak tam dělá problémy voicing asi