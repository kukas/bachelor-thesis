- Melodie je jednou z nejvýraznějších rysů hudební skladby. 

Pokud omezíme naši pozornost pouze na hudbu, která nějakou melodii obsahuje, 

Proč je ale transkripce melodie obtížný úkol? Podobně jako porozumnění mluvenému slovu je poslech hudby primárně intuitivní. Většině lidí nedělá problém při poslechu písně zpívat text spolu se zpěvákem a i začínající hráč na klavír dokáže po poslechu skladby přehrát notu po notě hlavní motiv skladby. 
Problém ale nastává, jakmile je potřeba problém formalizovat. Definice melodie je přinejmenším mlhavá. Podle \cite{poliner2007melody} je melodie, zhruba řečeno, jednohlasá posloupnost tónů, kterou posluchač zapíská či zazpívá, pokud ho požádáme o reprodukci nějaké polyfonní skladby. Tato posloupnost je vnímána jako samostatná entita a ovlivňuje ji nejen hlasitost, ale také výška, barva a délka zahraných tónů. \cite{dressler2017automatic}. V rámci jedné skladby ji po sobě může nést více různých nástrojů, například při jazzových sólech je obvyklé, že nástroj, který byl do té doby doprovodem, vyvstane do popředí. Co více, nástroj, který hraje melodii, se může změnit přímo v jejím průběhu (to je časté zejména u orchestrální hudby). Obecně je tedy složité určit melodii, i kdybychom měli k dispozici přesný přepis všech znějících tónů v nahrávce (například partituru nebo MIDI soubor). 
Stojí za zmínku, že i tato definice, ačkoli je velmi široká, má svoje omezení. Například nedokáže popsat jev, kdy ve skladbě hraje více rovnocenných melodií, jehož asi nejlepším příkladem jsou Bachova kontrapunktální díla, ve kterých se hlavní téma ztrácí a opět objevuje v organizované spleti ostatních hlasů. I když existují snahy o obecnější definici \cite{Bittner14medleydb:a}, která zahrnuje i vícenásobné melodie, nesetkal jsem se přímo s jejím využitím.

U intuitivního zavedení pojmu melodie ale komplexnost problému nekončí. 

* motivace
    * proč extrahovat melodii?
        * rytmus a melodie je podstatou většiny hudby napříč kulturami 
    
        Vytvoření robustního a přesného algoritmu pro extrakci melodie může výrazným způsobem zlepšit výsledky v dalších odvětvích MIR problémů. Extrakce melodie by se mohla stát prvním krokem při řešení problémů jako "cover song identification" (identifikace přezpívávané skladby), covery známých písniček často spočívají v zachování či pouze mírném pozměnění melodie a harmonie, výrazně se ovšem mění instrumentace. Extrakcí melodie obou porovnávaných nahrávek by bylo možné od hrajících nástrojů abstrahovat a porovnávat pak pouze posloupnosti not.
        Podobným problémem je pak "query by humming" (vyhledávání broukáním). Cílem je nalézt požadovanou skladbu pomocí vybroukané nebo zapískané melodie. Algoritmus pro extrakci by mohl automaticky rozšířit databázi známých skladeb ve kterých pak vyhledávání probíhá. Výhodou je, že pokud je pro porovnávání hledané melodie s databází použit algoritmus robustní vůči chybám, nemusí být výstup z algoritmu extrakce melodie úpůně přesný.
        Jiný způsob využití je pak v úlohách typu "source-separation", tedy separace audio stop jednotlivých nástrojů v polyfonní audio nahrávce. Nejčastěji jde o oddělení zpěvu a doprovodu, pro vytvoření karaoke verze písniček. Prvním krokem pro oddělení stopy je nalezení, kde zní a v jaých frekvencích. (TODO: citovat)
        Výrazně jiným příkladem využití může být tzv. "score following", tedy sledování notové osnovy.
        Hlavním přínosem ME algortimu je tedy zlepšení výsledků řady dalších MIR úloh, které již mají přímé využití pro hudebníky či veřejnost. Samotný algoritmus lze použít například při ruční transkripci nahrávky do notového zápisu

        * přínosy
            - pro hudební teorii, muzikologii, ethnomuzikologie:

                + transkripce sóla = In music theory, performance and education, a live recordings or recorded work that does not exist in written form can be transcribed for further analysis or practice
                    Devaney, J., Ellis, D.P.: An empirical approach to studying intonation tendencies in polyphonic vocal performances. Journal of Interdisci- plinary Music Studies 2 (2008)
                + analýza korpusů hudby -> https://jazzomat.hfm-weimar.de/
                * analýza intonace
                * automatic melodic motif and pattern analysis

            - pro MIR témata:
                + cover song identification
                + query by humming
                + informed source-separation = music de-soloing for the automatic generation of karaoke accompaniment
                    + Kawahara, H., Agiomyrgiannakis, Y., Zen, H.: Using instantaneous frequency and aperiodicity detection to estimate f0 for high-quality speech synthesis
                * singer characterization = zjištění identity zpěváka 
                + score following
                + genre identification
                    + Salamon, J., Rocha, B., G´omez, E.: Musical genre classification using melody features extracted from polyphonic music signals. In: Acoustics, Speech and Signal Processing (ICASSP), 2012 IEEE International Conference
            - pro hudební produkci:
                + Melodyne, AutoTune = pitch correction or modification.
                * adobe audiotion
    * proč je extrakce melodie těžká?
        * definice melodie
            * To frame the technical task of melody extraction, we should start by examining the musicological concept of “melody”, which ultimately relies on the  judgement of human listeners, and will therefore tend to vary across application contexts (e.g. symbolic melodic similarity or symbolic melodic similarity or music transcription). Centuries of musicological study have resulted in no clear consensus regarding the definition of “melody”, but faced with the need for a common interpretation, the MIR community has opted for simplified, pragmatic definitions that result in a task amenable to signal processing. One popular definition \cite{Poliner2007} holds that “the melody is the single (monophonic) pitch sequence that a listener might reproduce if asked to whistle or hum a piece of polyphonic music, and that a listener would recognize as being the essence of that music when heard in comparison”. This definition is still open to a considerable degree of subjectivity, since different listeners might hum different parts after listening to the same song (e.g., lead vocals versus guitar solo
                * v MIREXu a v praxi:
                    research has focused on what we term “single source predominant fundamental frequency estimation”. That is, the melody is constrained to belong to a single sound source throughout the piece being analyzed, where this sound source is considered to be the most predominant instrument or voice in the mixture. \cite{Salamon2014}
                    * takže jeden nástroj za celou skladbu! => Omezující (MedleyDB má více definicí).

                * v praxi se výzkum často omezuje na "extrakci melodie hlasu hlavního zpěváka nebo nástroje"
                * a aby se i tak vyhnulo co nejvíce subjektivitě, pracuje se více s materiálem, který není sporný
                * \cite{Salamon2014}
            * melodii nemusí nést nejhlasitější/nejvyšší nástroj
            * skladba může obsahovat více melodických linek
                * překrývající se harmonické frekvence
                * zpěvačka+doprovod
                * Fuga
            * jeden nástroj může být zároveň doprovod a zároveň jindy nést melodii
                * Jazz
            * polyfonie obecně - existuje spousta kombinací nástrojů
        * další nepříjemnosti
            * rezonance nástroje po zahrání tónu
            * dozvuk místnosti
            * rušení z perkusí
            * mastering = sice je nahrávka více vyrovnaná, ale problémy přepisu ještě zesiluje
                * digitální reverb (= dále rozostřuje hranice mezi začátky a konci not, zvětšuje míru překryvů zahraných not)
                * dynamic range compression = komprese dynamiky zvukového signálu = zmenšuje rozdíly mezi tichými a hlasitými zdroji zvuku
                    * Komprese dynamiky zvukového signálu je proces používaný v audiotechnice ke zmenšení dynamického rozsahu zpracovávaného signálu
        * i kdybychom měli přesnou transkripci (AMT), není triviální zjistit, které tóny patří do hlavní melodie
        * technická stránka
            * nedostatek dat, největší dataset má 7 hodin hudby
                * diskuse o syntetických datech
                    * na první pohled by sítě mohly mít problém s overfittingem, kvůli omezenému počtu samplů nástrojů
                        + například NMF by se nejspíš naučil opravdu jen konktrétní noty konktrétních nástrojů
                    * tady jim to funguje:
                        * Multitask Learning for Fundamental Frequency Estimation in Music

                * datasety jsou často velmi homogenní
                * obsahují chyby/chybějící informace
                    * příklad musicnet, který vznikl dynamic time warpem z midi souborů
                * o nedostatku/složitosti anotace:
                    + Bittner, R.M., Salamon, J., Tierney, M., Mauch, M., Cannam, C., Bello, J.P.: MedleyDB: A multitrack dataset for annotation-intensive MIR research. In: 15th International Society for Music Information Retrieval Conference, ISMIR (2014)
                    + Mauch, M., Dixon, S.: PYIN: a Fundamental Frequency Estimator Using Probabilistic Threshold Distributions. In: ICASSP, pp. 659–663. IEEE (2014)

            * zvuk má veliké rozlišení, srovnání s imagenetem třeba
            - tradeoff spektrogramů
        * case study? - Rozebrat příklady chyb, viz \cite{Salamon2014}
            - jedna úspěšná extrakce (s netriviálním pozadím)
            - jedna s voicing error a pitch error - kvůli doprovodným vokálům
            - jedna s octave error - zajímavý příklad s operním pěvcem, který má silnější první harmonickou frekvenci (to je charakteristické pro operní zpěv)
    * ? biologická motivace - popis ucha

* definice a vysvětlení
    * jednohlasá=monofonní/vícehlasá=polyfonní nahrávka
    * f0 estimation = co je f0
        + digital signal processing: je to nejnižší frekvence periodického signálu
        