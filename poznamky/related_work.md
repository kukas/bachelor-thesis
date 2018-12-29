- Deep learning pro audio
    + WaveNet
- Transkripce hlavní melodie vícehlasé nahrávky
    - obvyklá struktura melody extraction pipeline, Dressler, \cite{Salamon2014}:
        - polyphonic music ->
        - spectral analysis ->
        - pitch determination ->
        - tone tracking ->
        - melody identification

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
