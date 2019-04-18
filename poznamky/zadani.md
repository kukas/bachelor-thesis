# Zadání

Extrakce melodie je úloha, jejímž vstupem je vícehlasá hudba a výstupem je pro každý okamžik základní frekvence tónu, který je v daném okamžiku součástí melodie (za předpokladu, že melodie v onom okamžiku existuje). Melodie je definována jako „jednohlasá sekvence tónů, kterou posluchač nejspíše bude reprodukovat, pokud jej požádáme o zapískání či zabroukání příslušné skladby“. [1] Tím pádem je melodie vysoce relevantní pro vyhledávání v hudebních datech (Music Information Retrieval, MIR), neboť se jedná o jeden z nejvýraznějších rysů hudební skladby; zájem o melodii dokládá např. existence systémů na vyhledávání broukáním (query by humming). Extrakce melodie také může být užitečný krok v izolování zdrojů audia (source separation), např. pro automatické extrahování podkresů pro karaoke. 

Extrakce melodie je jednou z dlouhodobě řešených úloh v oboru MIR. Každoročně se objevuje v sérii soutěží MIREX, které představují „páteř“ výzkumu v oboru, čímž je zaručena dostupnost dat a dobře zavedené metodologie pro evaluaci extrakčních metod. Typicky se extrakce melodie řeší pomocí metod zpracování signálu (Digital Signal Processing, DSP), pomocí strojového učení, a kombinacemi obou přístupů [2,3,4]. Zatímco DSP se typicky používalo buď pro úplné řešení úlohy, nebo pro extrakci poměrně vysokoúrovňového popisu signálu (např. melodické kontury) jako vstupu pro strojové učení, nově etablované paradigma tzv. hlubokého učení (Deep Learning) umožňuje pracovat s poměrně nízkoúrovňovými rysy, jako např. přímo s frekvenčním spektrogramem. 

Řešitel prozkoumá současné nejúspěšnější přístupy k extrakci melodie, navrhne a implementuje nové metody (s důrazem na hluboké učení [2,5]), a experimentálně vyhodnotí úspěšnost svých návrhů pomocí zavedené metodologie MIREX. Demo vytvořených metod bude zpřístupněno online, s možností porovnat je s dosavadními přístupy. 

[1] G. E. Poliner, D. P. W. Ellis, F. Ehmann, E. Gómez, S. Steich, and B. Ong. „Melody transcription from music audio: Approaches and evaluation,“ IEEE Transactions on Audio, Speech and Language Processing, vol. 15, no. 4, pp. 1247-1256, 2007. 

[2] Sangeun Kum, Changheun Oh, Juhan Nam. "Melody Extraction on Vocal Segments Using Multi-Column Deep Neural Networks." In: Proceedings of the 17th International Society for Music Information Retrieval Conference, ISMIR 2016, New York City, United States, August 7-11, 2016, pp. 819-825. 2016, ISBN 978-0-692-75506-8. 

[3] Yukara Ikemiya, Katsutoshi Itoyama and Kazuyoshi Yoshii. "Singing Voice Separation and Vocal F0 Estimation Based on Mutual Combination of Robust Principal Component Analysis and Subharmonic Summation." IEEE/ACM Transactions on Audio, Speech, and Language Processing, vol. 24, no. 11, pp. 2084-2095, Nov. 2016. 

[4] J. Bosch & E. Gómez. "Melody extraction based on a source-filter model using pitch contour selection." In: Proceedings of the 13th Sound and Music Computing Conference (SMC 2016), Hamburg, Germany, pp. 67-74, Aug. 2016. 

[5] Ian Goodfellow, Yoshua Bengio and Aaron Courville. Deep Learning. MIT Press, 2016. [Online] http://www.deeplearningbook.org

------

# Cíl práce (proto-abstrakt)

- Extrakce melodie spočívá v odhadu kontury základní frekvence (F0) nejvýraznějšího melodického hlasu hudební skladby. 
- Se zvolna rostoucím množstvím dostupných dat se přístupy k řešení úlohy posouvají od deterministického zpracování signálu ke statistickým metodám strojového učení.
- V práci ukazujeme slibnost tohoto (technologického) posunu demonstrací současné úrovně systémů založených na hlubokém učení a zlepšením jejich výsledků provedením vlastních experimentů.
- Jejich úspěšnost vyhodnocujeme pomocí zavedené metodologie soutěže MIREX a uvádíme do kontextu stěžejních prací v oboru Music Information Retrieval.