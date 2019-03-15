TODO
- ✓ zkusit přidat MatthewEntwistle_FairerHopes hluboký tón do všech nahrávek, vypadá to, že to mrví korelaci - promyslet
- resampling anotací, aby šly použít pro trénování
- přepínač iterations místo epochs..
# Nápady
## Obecně
- augmentace dat
- použít musicnet a předtrénovat síť. Nejjednodušší by bylo vzít nejvyšší frekvenci jako melodii a třeba by to pomohlo
    - musicnet by šel možná zpřesnit na multif0 (teď je midi) pomocí vyhledávání nejbližších peaků
- ✓ sloučit notes a probs do jednoho grafu, dát do něj i spektrogram
- ✓ histogram intervalů o které se metoda spletla
- ✓ histogram přesnosti anotace podle výšky tónu
- L2 a BatchNorm? jak to spolu funguje?
- ukládat spektrogramy jako JPEG. Zjistit, jak moc to ovlivní kvalitu zvuku při inverzní transformaci
## Architektury (od nejjednodušších)
### Raw samples
- crepe spíš odhaduje jen jednu frekvenci v daném okně - zkusit méně penalizovat, když bude zkoušet víc frekvencí?
- přidat resnetové propojení do crepe
- větší stride na okrajích celého kontextu - síť nepotřebuje tak přesnou informaci o kontextu daleko od prostředka
    - případně nějak attention?
- přednastavení kernelu první vrstvy na sinusovky/filtry
- WaveNet   
    - nejdřív zkusit jako monopitch tracker
- Banka filtrů, vstup vícekanálový raw signál
- kouknout se na instantaneous frequency
- kouknout se na pYIN, auto-correlation
    - transformace - auto-correlation a pak, když najdu peaky, tak ještě kolem nich zpřesnit, protože posun o jeden sample může být příliš velký

- konvoluce vs. fourierka https://en.wikipedia.org/wiki/Convolution_theorem
https://librosa.github.io/librosa/generated/librosa.core.iirt.html

- extrémně široký okna (onsets and frames měli 20 sekund)
### Spectrogram
- Bittnerová
- Source separation multitask
    - pomocí informed-sourceseparation lze udělat trénovací data například i z WJazzD
    - Wave-U-Net

### Temporal dependencies
- LSTM
- seq2seq + attention



- inspirovat se multitask learningem od Bittnerové, ale zkusit přidat source separation jako další task.
    > Multitask Learning for Fundamental Frequency Estimation in Music
    > http://ismir2018.ircam.fr/doc/pdfs/138_Paper.pdf, https://www.youtube.com/watch?v=oGHC0ric6wo
    - zajímavý podle mě je, že ten source separation nemusí být dokonalý, nemusím řešit některé problémy, co musí řešit opravdové metody, protože mi o kvalitu separace vlastně nejde.

- samostatný voicing detection - někde jsem to viděl

- metrika: melodická kontura (jestli se trefuje o intervaly)

- nová augumentace dat - lidská persepce a identifikace tónu a nástroje je nezávislá na vynechání několika harmonických frekvencích. Vlastně zvuk zní hodně podobně, až stejně. Známý efekt je "phantom fundamental" - lidské ucho slyší fundamentální frekvenci, i když neexistuje. Co kdyby tedy jedna z augumentací fungovala tak, že vymaže některé harmonické frekvence?
    - ale nevím, to je jako kdyby v image recognition byla augumentace vkládání poloprůhledných obdélníků přes části obrázku, jo, lidi poznají obsah obrázku i tak, ale pomohlo by to sítím se líp naučit, co na obrázku je?

- onset detection - pro piano velmi zepšuje výsledky, co to zkusit pro ostatní nástroje? (já vím, piano je charakteristické, ale..?)
    - FFT s různými window size pro onset detection

- Multitask/autoencoder/regularizace AMT
    - zkusit ať se naučí dělat z raw dat spektrogram a někde z prostředního bottlenecku udělat melody extraction

- evoluční algoritmy v AMT, nebo v piano transcription
	- https://www.google.cz/search?q=evolution+algorithm+music+transcription

- pak pro temporální závislosti - seq2seq, transformer, attention atd.