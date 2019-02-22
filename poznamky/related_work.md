- Poslední review je z roku 2014 - \cite{Salamon2014}

- sekce "state-of-the-art" - s těma metodama, které jsem změřil


- Deep learning pro audio
    + WaveNet
- Transkripce hlavní melodie vícehlasé nahrávky
    - Na úlohu lze nahlížet jako na pokračování úlohy monopitch trackingu
        - Monophonic pitch trackers usually take the audio signal $x(t)$ and calculate a function $S_x(f_\tau, \tau)$ evaluated across a range of candidate pitch frequencies f that indicates the relative score or likelihood of the pitch candidates at each time frame τ.
        - pak se bere argmax_f, přičemž krom S_x se musí započítat také temporal constraints - průběh trackované frekvence je závislý na předchozí historii
        - Melody Extraction se snaží získat $\hat{f} = \argmax_f \sum_{\tau}{S_x(f_\tau, \tau) + C(f)}$ ze signálu obohaceného o melodický šum $y(t) = x(t) + n(t)$
        - dvě možnosti, jak monopitch trackery zobecnit:
            - Metody založené na salienci: zlepšit funkci $S_x$ tak, aby byla robustní proti dalším periodickým signálům.
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
                - 2005 Poliner and Ellis A classification approach to melody transcription
                    - SVM klasifikace na základě velmi ořezaných spektrogramů - 256 featur -> 60 midi not !
        - mnoho algoritmů se soustředí hlavně na transkripci zpěvu, jednak z komerčních důvodů (jsou nejčastější a nejpopulárnější) a jednak kvůli tomu, že zpěv má oproti hlasu nástrojů svoje unikátní charakteristiky, které metody dokážou využít pro lepší výsledky
    - obvyklá struktura melody extraction pipeline, Dressler, \cite{Salamon2014}:
        - polyphonic music ->
        - spectral analysis ->
        - pitch determination ->
        - tone tracking ->
        - melody identification

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