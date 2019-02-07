# Baseline perceptron

- pro ukázku, že úloha nelze vyřešit jednoduchým modelem, ale také jako fajn baseline, který by snad mělo být snadné překonat
- takhle jednoduchý model se umí zaseknout na triviálním předpovídání veškerých frames jako unvoiced, celkem logicky, protože je nejvyšší pravděpodobnost, že se s takovou předpovědí často trefí
    - tahle vlastnost je ale společná i pro větší modely, proto je potřeba zvážit oddělení voicingu a transkripce
    - případně mě napadá exponenciální decay pro váhy unvoiced framů
        - zkoušel jsem to u jednoduchých modelů a celkem očekávatelně se síť naučí zapomínat přepis. 
