# Related Work


- předzpracování signálu
    - bandpass? Resampling?
- spektrální analýza
    - proč - k čemu je
        - převod zvukového signálu z časové domény do frekvenční domény
        - jelikož tóny jsou zpravidla složeny z harmonických složek, které mají jasnou frekvenci, na spektru se projeví jako řada maxim
    - metody spektrální analýzy
        - STFT
            - tu asi rozepsat trochu
        - STFT odvozené - CQT, MRTFT
            - MRFFT \cite{Goto1999}
            - CQT: (Tachibana et al., 2010) and (Chien et al., 2012)
        - correlogram
        - There is evidence that the choice of the spectral analysis front-end has not such a marked influence on the overall accuracy of the melody extraction.
            - \cite{Dressler2016}, \cite{Salamon2014}
    - postprocessing spektra
        - filterbank \cite{Ryynanen2008}
        - peak picking  
            - lokální maxima \cite{Arora2013}
            - IF peak correction (Salamon)
            - IF-based peak selection? (\cite{Goto1999})
            - prokládání kubických funkcí
        - equal loudness
        - suppresing low amplitude or noise content
            - spectral whitening
        - bandpass filtr \cite{Goto1999}
        - harmonic/percussive source separation (HPSS)

In loose terms, the salience of a pitch is a measure of its perceptibility. Salience is closely related to loudness – in general, the louder a pitch is, the more salient it is. For example, there is typically a wide range of salience of across instrumental parts, where the melody is typically the most salient, and inner voices are less salient
Jednotlivé tóny se v této reprezentaci jeví jako množiny lokálních maxim v odpovídajících frekvenčních pásmech spektrogramu. Obecným cílem \textit{salienční funkce} je pak zvýraznění fundamentálních frekvencí tónů, odstranění zbylých alikvót a potlačení neharmonického obsahu.

Častým následujícím krokem je převod frekvenční osy na logaritmickou škálu pomocí banky filtrů (, ). Každý hudební interval odpovídá určitému podílu frekvencí tónů, v logaritmické škále jsou tudíž 

- zpracování spektra
    - co je salience function, proč
        Pitch salience representations are time-frequency representations that aim to measure the saliency (i.e. perceived amplitude/energy) of frequencies over time. They typically rely on the assumption that sounds humans perceive as having a pitch have some kind of harmonic structure. The ideal salience function is zero everywhere where there is no perceptible pitch, and a positive value that reflects the pitches’ perceived loudness at the fundamental frequency. - bittner17
    - temporal approaches
        - tohle furt moc nechápu
            - filterbank -> subband signals
            -> compression, half-wave rectification, low-pass
            -> predominant signal period in each frequency band

------

kapitoly:

- popis rule-based algoritmů
    - získání kandidátních f0 (pitch estimation)
        - spektrální analýza vs. autokorelace vs. 
    - tone tracking
    - voicing

- jak scénu mění data-based metody  
    - monopitch tracking, multif0, melody
- state of the art
    - Bittner
    - Basaran

- ??? ano nebo ne ???
    - machine learning
    - spectral representations (stft, cqt, mfcc, ...)

## Related work historický úvodní odstavec

Pokusy o vytvoření metody pro automatickou transkripci hudby se objevují již od sedmdesátých let \citep{Poliner2007}, dodnes však zůstává tento problém otevřený, zejména z důvodu strmě rostoucí obtížnosti úlohy s každým dalším přidaným hlasem ve zkoumaném signálu. V jedné z prvních prací, která se zabývá automatickým popisem muzikálních aspektů skladeb, se \cite{Goto1999} záměrně omezují na identifikaci jedné, nejhlasitější, spojité křivky fundamentní frekvence (F0) hlasu v omezeném frekvenčním rozsahu. Vzniklé transkripce pak sice nejsou kompletní, na druhou stranu je jejich získání výpočetně nenáročné a přitom poskytují sémanticky bohatý popis nahrávek, který je poměrně často shodný s melodií. Ustanovením úlohy pojmenované "Predominant-F0 Estimation" (PreFEst), budují \cite{Goto1999} základy pro vznik navazujících prací a soutěží zabývající se automatickým přepisem melodie.

## Definice melodie

<!-- 
- melodii nelze přesně definovat, vždy ve výsledku záleží na posluchačích.
    - staletí výzkumu v muzikologii nevedlo k jasné definici melodie.
    - Melodie je definována jako „jednohlasá sekvence tónů, kterou posluchač nejspíše bude reprodukovat, pokud jej požádáme o zapískání či zabroukání příslušné skladby.“
        The melody is the single (mono- phonic) pitch sequence that a listener might reproduce if asked to whistle or hum a piece of polyphonic music, and that a listener would recognize as being the “essence” of that music when heard in comparison. \cite{Poliner2007}
        - přesto, že nejde o objektivní definici, v praxi se posluchači často na jedné konkrétní sekvenci tónů shodnou, a to jak u populární hudby, kde melodii často nese lidský zpěv, tak u orchestrální hudby \citep{Bosch2016}.

        - spor: skladba může obsahovat více melodických linek
            * překrývající se harmonické frekvence
            * zpěvačka+doprovod
            * Fuga

- v praxi se výzkum omezuje na pragmatické a zjednodušené definice
    * problémy singlesource prefest:
        * omezující ->
        * jeden nástroj může být zároveň doprovod a zároveň jindy nést melodii
            * Jazz
    * problémy prefest:
        - co znamená predominant?
            - melodii nemusí nést nejhlasitější/nejvyšší nástroj  
            - Composers and performers use several cues to make melodies perceptually salient, including loudness, timbre, frequency variation or note onset rate.
                - Melody extraction methods commonly use cues such as pitch continuity and pitch salience, and some of them group pitches into higher level objects (such as tones or contours)
        - missing fundamental
           + Amacher a její hudba rezonující v uchu
        - často také omezení na harmonický signál
    
    * a aby se i tak vyhnulo co nejvíce subjektivitě a problémům s definicemi, pracuje se více s materiálem, který není sporný
- nové datasety používají již širší definice 
    MedleyDB, Orchset
    - Orchset ale nemá spojitou informaci o f0, ale jen výšky not


* definice melodie
    * To frame the technical task of melody extraction, we should start by examining the musicological concept of “melody”, which ultimately relies on the  judgement of human listeners, and will therefore tend to vary across application contexts (e.g. symbolic melodic similarity or symbolic melodic similarity or music transcription). Centuries of musicological study have resulted in no clear consensus regarding the definition of “melody”, but faced with the need for a common interpretation, the MIR community has opted for simplified, pragmatic definitions that result in a task amenable to signal processing. One popular definition \cite{Poliner2007} holds that “the melody is the single (monophonic) pitch sequence that a listener might reproduce if asked to whistle or hum a piece of polyphonic music, and that a listener would recognize as being the essence of that music when heard in comparison”. This definition is still open to a considerable degree of subjectivity, since different listeners might hum different parts after listening to the same song (e.g., lead vocals versus guitar solo
        * v MIREXu a v praxi:
            research has focused on what we term “single source predominant fundamental frequency estimation”. That is, the melody is constrained to belong to a single sound source throughout the piece being analyzed, where this sound source is considered to be the most predominant instrument or voice in the mixture. \cite{Salamon2014}
            * takže jeden nástroj za celou skladbu! => Omezující (MedleyDB má více definicí).

        * v praxi se výzkum často omezuje na "extrakci melodie hlasu hlavního zpěváka nebo nástroje"
        * a aby se i tak vyhnulo co nejvíce subjektivitě, pracuje se více s materiálem, který není sporný
        * \cite{Salamon2014}

    * polyfonie obecně - existuje spousta kombinací nástrojůG
\cite{Bosch2016a}

Melody is regarded as one of the most relevant aspects of music, and melody extraction is an important task in Mu- sic Information Retrieval (MIR). Salamon et al. [21] define melody extraction as the estimation of the sequence of fun- damental frequency (f0) values representing the pitch of the lead voice or instrument, and this definition is the one employed by the Music Information Retrieval Evaluation eXchange (MIREX) [7]. While this definition provides an objective and clear task for researches and engineers, it is also very specific to certain types of music data. Recently proposed datasets consider broader definitions of melody, which are not restricted to a single instrument [2, 4, 6]. Composers and performers use several cues to make
melodies perceptually salient, including loudness, timbre frequency variation or note onset rate. Melody extraction methods commonly use cues such as pitch continuity and pitch salience, and some of them group pitches into higher level objects (such as tones or contours), using principles from Auditory Scene Analysis [8, 13, 16, 18, 20]. Some approaches have also considered timbre, either within a source separation framework [10, 17], with a machine learning approach [11], or in a salience based approach [14, 16]. One

\cite{Poliner2007}


- Providing a strict definition of the melody is, however, no simple task: it is a musicological concept based on the judgment of human listeners, and will not, in general, be uniquely defined for all recordings. Roughly speaking, the melody is the single (mono- phonic) pitch sequence that a listener might reproduce if asked to whistle or hum a piece of polyphonic music, and that a lis- tenerwould recognize as being the “essence” of that music when heard in comparison.
    - v některých případech se posluchači bez obtíží shodnou, zejména v případě populární hudby, která má často hlavní hlas, který melodii nese
    - i v orchestrálních nahrávkách nebo v i polyfonních skladbách pro klavír, se ale často posluchači shodnou


\cite{Salamon2014}

- definice melodie záleží ve výsledku na posluchačích
- 
-->

## Shrnutí do roku kol. 2012?



Podrobný rozbor souvisejících metod by přesahoval rámec této práce, jen do soutěže MIREX se od roku 2005 přihlásilo 45 týmů s 62 různými metodami. 

V množství rozmanitých přístupů k extrakci melodie lze najít společné rysy

- kompilace souhrnných prací Salamon, Dressler, Bosch

The task of melody extraction from polyphonic music recordings has been generally approached with salience- based or separation-based methods [1]. Salience-based ap- proaches compute a frame-based pitch salience function, while separation-based approaches attempt to isolate the melody source from the mixture more or less explicitly. \cite{Bosch2016b}

The most commonly used pitch salience function is harmonic summation -> A. Klapuri, “Multiple fundamental frequency estima- tion by summing harmonic amplitudes,” in Proc. IS- MIR, 2006, pp. 216–221.


## Popisy algoritmů

\cite{Goto1999}
- multirate filterbank
- IF calculation
- peak selection using IF (Candidate Frequency Components)
- bandpass weighting
- modelování těchto frekvencí jako váženého součtu harmonických struktur
    - odhad parametrů modelu pomocí Expectation-Maximization algoritmu
- tracking agents


\cite{Ryynanen2008}
- přepis melodie, bass a akordů
-  The salience, or strength, of each F0 candidate is calculated as a weighted sum of the amplitudes of its harmonic partials in a spectrally whitened signal fram
- STFT s hamming window a zeropadded -> bandpass filterbank with triangular response -> spectral whitening (jakási normalizace spektra pronásobením síly signálu na každém filtru umocněnou silou filtru (s exponentem patřícím do <0,1>))
- salience je pomocí harmonic summation, pro sumu se bere maximum z frekvenčního pásma každé harmonické (takže drobně neharmonické frekvence to bere v potaz), váhy harmonické struktury jsou typu 1/x
- používá differential salience, jako \delta s_t = s_t -s_{t-1}
- HMM pro modelování znění not (diskrétní MIDI!) (attack, sustain release stavy)
- HMM pro modelování muzikologických jevů (stupnice a přechodů mezi notami)
- pak zpřesňování na f0 na základě výstupních not

\cite{Durrieu2010}
Metoda založená na modelování (výkonového) spektra signálu jako součtu složek hlavního nástroje a doprovodu. Hlavní hlas je reprezentován modelem \textit{source/filter}, doprovod je modelován pomocí váženého součtu spektrálních šablon. Parametry modelu pro hlavní hlas jsou odhadnuty pomocí \textit{Expectation-Maximization} algoritmu, doprovod je dekomponován na šablony pomocí metody příbuzné nezáporné maticové faktorizaci. 

Metoda založená na source-filter modelování hlavního nástroje a modelování doprovodu pomocí nezáporné maticové faktorizaci

\cite{Poliner}
- downsample to 8kHz
- STFT, only bins 0-2kHz selected (256 features), normalization
- 256 feature vector -> 60 output MIDI notes
    - problém na vibratu
- v mirex 2005 srovnatelný s nejlepšími metodami
. 

The first to appear in Table I is the data driven approach by Poliner and Ellis [30]. Rather than handcraft knowledge about musical acoustics into the system (e.g. in the form of a salience function based on harmonic summation), they propose to use machine learning in order to train a classifier to estimate the melody note directly from the power spectrum. As a preprocessing step they downsample the audio to 8kHz, and use the STFT to obtain a spectral representation. Bins corresponding to frequencies above 2 kHz are discarded and the magnitude of the remaining bins is normalized over a short time period to reduce the influence of different instrument timbres. The resulting 256 feature vector is used to train a support vector machine classifier using training data where each frame is labeled with one of 60 possible output classes corresponding to 60 MIDI notes spanning five octaves. Voicing detection is done by means of a global threshold based on the magnitude squared energy found between 200 and 1800 Hz.


\cite{Paiva2006}
-Paiva uses the Lyon–Slaney auditory model up to the summary autocorrelation
    - založený na bandpass filtrech a autokorelační funkci
    - Both these approaches use autocorrelation, which also achieves phase invariance
    - also has the attractive property of summing all harmonics relating to a common period into a peak at that period. The
    - The Lyon–Slaney system actually calculates autocorrelation on an approximation of the auditory nerve excitation, which separates the original signal into multiple frequency bands, then sums their normalized results
- Paiva’s multipitch detection involves simply choosing the largest peaks from this summary autocorrelation. 
- tonetracking vytváří jednotlivé noty, ale drží si k nim původní f0
    - first create pitch tracks by connecting pitch candidates with similar frequency values in consecu- tive frames
        - pitch track can contain more than one note -> glissando, vibrato
- a pak melody identification
    - ghost notes removal (pokud jsou dvě pitchtracks vzdálené o oktávu a mají stejnou f0, tak smaž tu méně salientní)
    - vybere melodické na základě
        - salience
        - frekvence (v prvních krocích zahazuje <50 MIDI)
        - melodic smoothness

\cite{Dressler2012}


-------

### Data-based

\cite{Kum2016} ISMIR 2016

- ukazují, že výkon sítí je nepřímo úměrný jemnosti kvantizace, přitom já dokazuju pravý opak
- multi column deep neural networks
    - používají více neuronových sítí s různě kvantizovanými výstupy, aby je na konci pronásobili a dostali funkci salience
        - 3 hidden layers , RELU
- pitch shifting augmentation
- HMM temporal smoothing - Viterbi decoding
- trénováno pouze na voiced datech
    - VAD velmi jednoduchý - normalized spectral energy sum + threshold
- resample 8kHz -> STFT 1024, 10ms hopsize -> first 256 samples (jako poliner)
- kontext pomáhá do 70-90ms, pak už ne

\cite{Rigaud2016} ISMIR 2016

- rozdělení na dvě DNN, f0 estimation a VAD
- 16khz mono
- signal decomposition double-stage Harmonic/Percussive Source Separation
    - na základě S. Leglaive, R. Hennequin, and R. Badeau. Singing voice detection with deep recurrent neural networks.
    - výsledkem jsou 4 různé výstupy dekompozice
        - dekompozice z STFT s oknem 300ms, první signál p_1 obsahuje melodii a bubny, druhý signál h_1 zbylé stabilní nástroje (klavír, kytara, kontrabas)
            - signál p_1 s melodií a bubny se dále dělí pomocí STFT s oknem 30ms na h2 s melodií a p2 s perkusemi

- data augmentation
- Voicing network: Bidirectional LSTM, 3 layers, 50 units
- F0 network: 2-layer fully connected network, ReLU
    - výstup kvantifikován na čtvrtinu půltónu
- postprocessing: Viterbi tracking 

\cite{Bittner2017}

- denoising problem
    - HCQT -> salience
- input
    - HCQT - několik vrstev CQT spektrogramů s počatečení frekvencí posunutou o harmonický skok. Tím, že CQT má logaritmickou osu frekvence, tak jsou skoky konstantní
        - argument: harmonické pro jiné h*f_0 než h=2,4,8... se nedají změřit přesně
            - můj protiargument: tím, že používáme konvoluci, tak se síť prostě pro daný kanál naučí trochu posunuté váhy
    - HCQT 0.5, 1, ... 5
    - hopsize 11ms, 6 oktáv s 5 biny na půltón, f_min = 32.7 (~C1)
- output
    - ground truth quantized to nearest bin and gaussian blurred
        - decays to zero within a quartertone
        - in order to soften the penalty for near-correct predictions
        - také ground truth může mít chyby do 20 centů
- no temporal smoothing
    - ale 150 ms kontext...
- voicing by threshold



\cite{Balke2017}


### \cite{Dressler2016}

- obvyklá struktura melody extraction
    - polyphonic music ->
    - spectral analysis ->
        - The aim of the spectral analysis is to transform the audio signal from the time domain to the frequency domain, which facilitates the identification of distinct sound sources in the audio input: Melody tones are required to have a pitch which allows the ordering of sounds on a frequency-related scale. A common attribute of pitched sounds (in melodies) is that they consist of sinusoidal partials. Hence, the most relevant melody information can be found in the deterministic components of the audio signal, which often can be identified as spectral peaks in the frequency domain.
        - tři kategorie - filterbank, FFT, FFT s proměnným rozlišením (CQT, multiresolution-fft)
            - filterbank - obvykle bližší lidskému sluchu
            - multiresolution fft - také se podobají lidskému sluchu - the spectral resolution is better at lower frequencies, whereas the temporal resolution improves at higher frequencies.?????
            - porovnáním metod Dresslerová dochází k tomu, že na nynější úrovni na vstupním spektrogramu nezáleží

        - magnitude weighting step which reestimates the importance of the spectral components.
            - the weighting may be perceptually motivated and mimic the sensitivity of humans to signals of different frequencies according to equal-loudness contours (Heinz, 2006; Paiva, 2006; Salamon and G´omez, 2012), or, the weighting aims to level out magni- tude differences between spectral peaks independent of their frequency. The latter procedure is often referred to as spectral whitening.

        TREND: STFT, a prý na tom moc nezáleží

    - pitch estimation ->
        - temporal cues (autocorrelation) vs. spectral cues (harmonic sieve)
            - temporal cues
                - band-wise processing of the signal (filterbank)
                - subbands - compressed, half-wave rectification, low-pass filtering
                - predominant signal period is detected in each frequency channel (autocorrelation, differece function or stft)
            - spectral approach
                - Deterministic methods (rule based):
                    - Goto (2004)
                        - f0 salience + tracking agents
                    - Hermes (1988)
                        - subharmonic summation algorithm (?)
                    - Rao and Rao (2010) use a combination of a harmonic matching algorithm and the two-way-mismatch-error method, as originally described in (Maher and Beauchamp, 1994).
                    - Chien et al. (2012) evaluate the likelihood of each pitch candidate by comparing the observed constant-Q spectrum with a set of (synthesized monophonic) vocal spectrum exam- ples with the same hypothesized pitch and a scaled loudness.
                - Probabilistic methods:
                    - Poliner and Ellis (2006) implemented a sup- port vector machine to classify the melody directly from the spectral components
                    - Durrieu et al. (2010) computed the best non-negative linear combination of spec- tral combs of all possible notes, evaluating also the smoothness of timbre between analysis frames.
            TREND: subharmonic summation (SHS),
                
        - Even though the melody is usually the predominant pitch in the audio mixture, pitch extraction methods which are designed to work with monophonic audio often fail to produce satisfactory results with music, as the algorithm has to be very robust against interference from simultaneous sounds. Neither a simple autocorrelation of the audio waveform nor a harmonic sieve used in the spectral domain would give satisfactory results if applied to music.
        - multiple fundamental frequency estimation
            - iterative detection + deletion
            - joint pitch candidate evaluation


    - tone tracking ->
    - melody identification

    - doopravdy pouze melody identification je unikátní pro úlohu extrakce, jinak se tyto podproblémy řeší i v jiných úlohách.

### \cite{Poliner2007}

- příklad spektrogramu s vokální stopou a celým mixem, ilustrace toho, že nemusí být nejhlasitější
- extrakce melodie je příbuzná s úlohou sledování výšky tónu. V kontextu identifikace melodie v hudbě s více hlasy je úloha sledování výšky tónu komplikovanější, jelikož ze všech možných znějících tónů patří do melodie nejvýše jeden. 
    - všechny metody extrakce melodie tedy nutně řeší dva problémy - 1) identifikaci množiny tónů v daném čase a 2) přiřazení některého (pokud nějakého) tónu k melodii.

- kroky
    - initial signal processing
        - stft
            - invariantní k fázovým posunům, což se hodí, protože percepce výšky tónu je závislá právě na frekvenci, nikoli na fázi
            -Since the frequency resolution of the STFT improves with temporal window length, these systems tend to use long windows, from 46 ms for Dressler, to 128 ms for Po- liner.
            - hierarchické stft (multiresolution)
        - autocorrelation
            - 
    - spectral peak processing
        - instantaneous frequency
    - multipitch - jak systém rozpozná kandidáty na fundamentální frekvence
        - v případě STFT jde o to, jak rozdělit harmonické k jednotlivým fundamentálám
            - nejjednodušší způsob - projít všechny kandidáty na f0 a vyhodnotit harmonické frekvence
                - octave errors - 2*f0 má také dost harmonických
            - procházení f0 odspoda a případně úprava spektra, aby tam harmonické od nalezené f0 nebyly
            - přiřazení vah ke všem kandidátům f0 tak, aby fundamentály soutěžily o harmonické - 

    - onset events
        - hledání not či jiných delších logických celků
        - získávání více informací (například o dynamice) nebo naopak abstrahování (například vibrata)
        - HMM, které 
    - post-processing
        - získání melodie
        - množina pravidel pro výběr z nalezených not
            - důraz na kontinuitu výšky a síly not v melodii
            - vymazání velkých skoků, krátkých v délce
            - preference pro nějaký frekvenční rozsah
            - výběr nejvyšších nebo nejnižších frekvencí při souzvuku
        - tracking agents
            - soupeřící hypotézy, výtězí ta, která nejlépe splňuje stanovené podmínky
        - HMM
    - voicing
        - někteří neřeší
        - a simple global energy threshold over an appropriate frequency range was reported to work as well as a more complex scheme based on a trained classifie
        - případně je řešen výběrem not z předchozího kroku (také se dají ještě odfiltrovat slabé noty)


- Poslední review je z roku 2014 - \cite{Salamon2014}

- sekce "state-of-the-art" - s těma metodama, které jsem změřil
## State-of-the-art
### Salamon 2014


- Deep learning pro audio
    + WaveNet
- Transkripce hlavní melodie vícehlasé nahrávky
    - Na úlohu lze nahlížet jako na pokračování úlohy monopitch trackingu
        - Monophonic pitch trackers usually take the audio signal $x(t)$ and calculate a function $S_x(f_\tau, \tau)$ evaluated across a range of candidate pitch frequencies f that indicates the relative score or likelihood of the pitch candidates at each time frame τ.
        - pak se bere argmax_f, přičemž krom S_x se musí započítat také temporal constraints - průběh trackované frekvence je závislý na předchozí historii
        - Melody Extraction se snaží získat $\hat{f} = \argmax_f \sum_{\tau}{S_x(f_\tau, \tau) + C(f)}$ ze signálu obohaceného o melodický šum $y(t) = x(t) + n(t)$
        - dvě možnosti, jak monopitch trackery zobecnit:
            - Metody založené na salienci: zlepšit funkci $S_x$ tak, aby byla robustní proti dalším periodickým signálům.
                - největší skupina (asi cite Salamon2014, kdyžtak Salamon2012)
                - tady třeba Salamon salience
                - je potřeba určitě zlepšit i $C(f)$, tedy temporal constraint
                    - tracking techniques: Viterbi decoding, tracking agents, clustering, etc.
                - běžná struktura:
                    - preprocessing
                        - filtr pro zvýraznění frekvenčního pásma, kde očekáváme melodii
                        - filtr pro ekvalizaci hlasitosti podle lidského vnímání
                        - source separation jako preprocessing pro zvýraznění melodie
                    - spectral transform
                        - STFT - most straight forward - window length typically 50-100ms
                        - multiresolution transforms
                            - multirate filterbank, constant-Q transform, multi-resolution FFT
                            - argumentují, že MRFFT pro některé přístupy nemá přidanou hodnotu (některé metody ale možná vyžadují)
                                - to samé psala Karin Drassler
                        - \cite{Durrieu2011}: Mid-level representations
                            - něco jako spektrální transformace, ale za cenu neexistující inverzní funkce líp zachytí jiné charakteristiky než sílu frekvencí
                        - autocorrelation

                    - spectral peak processing
                        - většina algoritmů ze spektrogramu extrahuje pouze peaky, splňující různé podmínky, tím se odfiltruje část nemelodických šumů
                    - Salience function
                        - this function provides an estimate of the salience of each possible pitch value over time
                        - the peaks of this function are taken as possible candidates for the melody
                        - most approaches use some form of harmonic summation - the salience of a certain pitch is calculated as a weighted sum of the amplitude of its harmonic frequencies
                    - tracking
                        - získání melodie z kandidátů v salience function
                        - zde se algoritmy nejvíce různí - clustering, heuristic-based agents, HMMs, dynamic programming, ...
                    - voicing
                        - common approach is to use a fixed or dynamic per-frame salience-based threshold

            - Metody založené na source separation (dekompozici signálu na jednotlivé zdroje?): Jeden ze zdrojů bude náležet hlavní melodii, na něj pak můžeme spustit obyčejný monopitch tracker.
                - decomposition, matrix factorization techniques
            - Data-based metody
                - Jeden z prvních: 2005 Poliner and Ellis A classification approach to melody transcription
                    - klasifikace frame po framu pomocí SVM
                    - učeno na tisících ukázkách - spektrogram -> melodie
                    - musí se naučit to, že tón je množina harmonických frekvencí
                        - a taky to, jak odlišit hlavní melodii od doprovodu
                    - žádný postprocessing
                    - dosáhl tehdejšího state-of-the-art
                    - SVM klasifikace na základě velmi ořezaných spektrogramů - 256 featur -> 60 midi not !
        - mnoho algoritmů se soustředí hlavně na transkripci zpěvu, jednak z komerčních důvodů (jsou nejčastější a nejpopulárnější) a jednak kvůli tomu, že zpěv má oproti hlasu nástrojů svoje unikátní charakteristiky, které metody dokážou využít pro lepší výsledky

    - orchestrální hudba: \cite{Bosch2016}
        - Bosch sestavil Orchset a použil ho pro porovnání tehdejších state-of-the-art algoritmů na orchestrálních datech
            - porovnání melody extraction, multif0 i jejich salience funkcí
            - porovnání salience funkcí probíhá tak, že se hledá melodie mezi 1/2/4/10 nejsilnějšími frekvencemi podle funkce salience
            - obdobně multif0
        - z porovnání vychází, že Durrieu má velmi dobrou funkci salience
            SF-DUR obtains 61.7% raw pitch accuracy even without any smoothing
            ME-DUR obtains the highest raw pitch accuracy: 66.9%

        A manual examination of the estimation errors suggests that the most challenging excerpts contain chords and harmonisations of the melody, a highly energetic accompaniment, and in some cases percussion. Most accurate estimations are generally obtained in excerpts with a very predominant melody (e.g. those in which the orchestra plays in unison). A more detailed analysis of the influence of several musical characteristics is presented in the following section.
        - o Salamonovi: The accuracy obtained with SF-SAL, is the lowest compared to the rest of salience functions. In comparison to SF-DUR, it achieves 27.4 percentage points (pp) less RP for N=1, which partially explains that the complete melody extraction method (ME-SAL) also performs much worse in comparison to ME- DUR (38.5 pp. less RP). Additionally, this rule-based approach (which obtained the highest overall accuracy in MIREX) seems to be tuned to the pitch contour features of vocal music (pop, jazz), and is not able to generalise to the character- istics of our dataset. The salience-based voicing detection is quite conservative in this dataset, and classifies only 57.4% of the frames as voiced, possibly due to the high dynamic range in symphonic music.
        - Durrieu: Vadí mu alternující sekce hrající melodii, protože v principu se učí barvu hlavního hlasu pro každý výňatek zvlášť, takže měnící se barva hlasu sníží přesnost (ale ne příliš)
        - obecně jsou nejjednodušší zeště, pro všechny testované algoritmy
        - Durrieu umí poznat smyčce
        - co se týče tvaru melodie, algoritmům nejvíc vadí pitch complexity (předvídatelnost melodie) a pitch density (amout of notes per second)


- Transkripce f0 jednohlasé nahrávky
    - Podle \cite{Salamon2014} historicky extrakce melodie vychází z úlohy f0 estimation
    - kořeny ve speech processingu - první monopitch trackery z něj vycházely.

    I přesto, že je transkripce jednohlasé nahrávky jednodušší podúlohou transkripce melodie, v rámci pokusů o úplnou anotaci audiostop datasetu MedleyDB (viz kapitola experimenty) jsme otestovali dva algoritmy.
    * pYIN
        * nedává piano
        * dozvuk dělá voicing problémy
        * bleed samozřejmě vadí
            - lze srovnat dvě nahrávky se zpěvem, jedna s bleedem jedna bez
        * problém také tiché nahrávky, ale lze normalizovat
    * CREPE
        * chybí voicing detection