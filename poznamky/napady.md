TODO
- ✓ zkusit přidat MatthewEntwistle_FairerHopes hluboký tón do všech nahrávek, vypadá to, že to mrví korelaci - promyslet
    - nakonec nemrvilo, CREPE prostě tu harfu neslyšel ani bez doprovodu
- resampling anotací, aby šly použít pro trénování
- přepínač iterations místo epochs..

Music Technology conference list
http://conferences.smcnetwork.org/2018

# Nápady
## Obecně
- augmentace dat
    - šum
    - pitch
    - kombit STEMy medleydb, mdb
    - pomocí source separation ztišit hlavní melodii

- použít musicnet a předtrénovat síť. Nejjednodušší by bylo vzít nejvyšší frekvenci jako melodii a třeba by to pomohlo
    - musicnet by šel možná zpřesnit na multif0 (teď je midi) pomocí vyhledávání nejbližších peaků
- ✓ sloučit notes a probs do jednoho grafu, dát do něj i spektrogram
- ✓ histogram intervalů o které se metoda spletla
- ✓ histogram přesnosti anotace podle výšky tónu
- L2 a BatchNorm? jak to spolu funguje?
- ukládat spektrogramy jako JPEG. Zjistit, jak moc to ovlivní kvalitu zvuku při inverzní transformaci

## Metriky
- zjistit, proč umí loss stoupat a přitom OA zůstávat
    - třeba jestli predikuje víc kandidátů, ale nevim, jak to úplně zachytit
- přesnost podle nástrojů
- metrika kontinuálnosti f0 (třeba součet skoků / součet skoků v datech)
- pro orchset lze vytvořit víc metrik - podle toho, kdo hraje melodii a jestli se nástroje hrající melodii střídají. Na základě přiloženého CSV


## Architektury (od nejjednodušších)
### Raw samples
- ✓ crepe spíš odhaduje jen jednu frekvenci v daném okně - zkusit méně penalizovat, když bude zkoušet víc frekvencí?
- přidat resnetové propojení do crepe
- ✓ větší stride na okrajích celého kontextu - síť nepotřebuje tak přesnou informaci o kontextu daleko od prostředka
    - případně nějak attention?
- přednastavení kernelu první vrstvy na sinusovky/filtry
- ✓ WaveNet   
    - nejdřív zkusit jako monopitch tracker
    - As a last experiment we looked at speech recognition with WaveNets on the TIMIT (Garofolo et al., 1993) dataset. For this task we added a mean-pooling layer after the dilated convolutions that aggregated the activations to coarser frames spanning 10 milliseconds (160× downsampling). The pooling layer was followed by a few non-causal convolutions. We trained WaveNet with two loss terms, one to predict the next sample and one to classify the frame, the model generalized better than with a single loss and achieved 18.8 PER on the test set, which is to our knowledge the best score obtained from a model trained directly on raw audio on TIMIT.
- Banka filtrů, vstup vícekanálový raw signál
- kouknout se na instantaneous frequency
- kouknout se na pYIN, auto-correlation
    - transformace - auto-correlation a pak, když najdu peaky, tak ještě kolem nich zpřesnit, protože posun o jeden sample může být příliš velký
    - jak vypadá autokorelace dvou blízkých signálů? Třeba 440Hz a 435Hz? Protože na STFT to skoro nebude vidět (Jen se trochu zvětší ten blob kolem nejbližších binů), ale na té autokorelační by to třeba mohlo být vidět? Nevim

- nová Magenta: GANSynth https://openreview.net/pdf?id=H1xQVn09FX

- konvoluce vs. fourierka https://en.wikipedia.org/wiki/Convolution_theorem
https://librosa.github.io/librosa/generated/librosa.core.iirt.html

- jak udělat z conv1d fourierku?

- extrémně široký okna (onsets and frames měli 20 sekund)

### Spectrogram
- Bittnerová
- cqt co jde transformovat zpět: https://mtg.github.io/essentia-labs/news/2019/02/07/invertible-constant-q/
- Source separation multitask
    - pomocí informed-sourceseparation lze udělat trénovací data například i z WJazzD
    - Wave-U-Net
- **multiresolution stft** + dilated první vrstva o oktávy
    - menší rozlišení se musí naškálovat
- multihop stft - více kanálů, každý s jiným hopsize? To je asi blbost, ale způsob, jak dát levně síti kontext
- **gaussovské rozmazání v čase, pro snížení počtu jednooknových predikcí**
- různý window fn jako kanály?

### Temporal dependencies
- LSTM
- seq2seq + attention

- zaokrouhlit f0 salience na MIDI, tedy na abstraktnější reprezentaci, nad tim udělat HMM/LSTM/něco a pak zpětně dohledat přesný frekvence (Ryynanen)

### Dataset creation
- anotace MusicNet - pomocí informed source separation získat hlasitosti nástrojů a sekcí. Zkombinovat tuto informaci s teoriemi jak získat z MIDI melodii. 
    - pokud se získává melodie z MIDI, pak chybí lidská interpretace
    - pokud se získává melodie z audia, chybí přesná anotace
        - s obojím by možná šlo melodii extrahovat lépe


- inspirovat se multitask learningem od Bittnerové, ale zkusit přidat source separation jako další task.
    > Multitask Learning for Fundamental Frequency Estimation in Music
    > http://ismir2018.ircam.fr/doc/pdfs/138_Paper.pdf, https://www.youtube.com/watch?v=oGHC0ric6wo
    - zajímavý podle mě je, že ten source separation nemusí být dokonalý, nemusím řešit některé problémy, co musí řešit opravdové metody, protože mi o kvalitu separace vlastně nejde.

- samostatný voicing detection - někde jsem to viděl

- metrika: melodická kontura (jestli se trefuje o intervaly)

- nová augmentace dat - lidská persepce a identifikace tónu a nástroje je nezávislá na vynechání několika harmonických frekvencích. Vlastně zvuk zní hodně podobně, až stejně. Známý efekt je "phantom fundamental" - lidské ucho slyší fundamentální frekvenci, i když neexistuje. Co kdyby tedy jedna z augmentací fungovala tak, že vymaže některé harmonické frekvence?
    - ale nevím, to je jako kdyby v image recognition byla augmentace vkládání poloprůhledných obdélníků přes části obrázku, jo, lidi poznají obsah obrázku i tak, ale pomohlo by to sítím se líp naučit, co na obrázku je?
    
    google to používá na speech recognition! https://ai.googleblog.com/2019/04/specaugment-new-data-augmentation.html

- onset detection - pro piano velmi zepšuje výsledky, co to zkusit pro ostatní nástroje? (já vím, piano je charakteristické, ale..?)
    - FFT s různými window size pro onset detection

- Multitask/autoencoder/regularizace AMT
    - zkusit ať se naučí dělat z raw dat spektrogram a někde z prostředního bottlenecku udělat melody extraction

- evoluční algoritmy v AMT, nebo v piano transcription
	- https://www.google.cz/search?q=evolution+algorithm+music+transcription

- pak pro temporální závislosti - seq2seq, transformer, attention atd.