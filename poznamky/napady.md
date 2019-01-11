- inspirovat se multitask learningem od Bittnerové, ale zkusit přidat source separation jako další task.
    > Multitask Learning for Fundamental Frequency Estimation in Music
    > http://ismir2018.ircam.fr/doc/pdfs/138_Paper.pdf, https://www.youtube.com/watch?v=oGHC0ric6wo
    - zajímavý podle mě je, že ten source separation nemusí být dokonalý, nemusím řešit některé problémy, co musí řešit opravdové metody, protože mi o kvalitu separace vlastně nejde.

- samostatný voicing detection - někde jsem to viděl

- metrika: melodická kontura (jestli se trefuje o intervaly)

- nová augumentace dat - lidská persepce a identifikace tónu a nástroje je nezávislá na vynechání několika harmonických frekvencích. Vlastně zvuk zní hodně podobně, až stejně. Známý efekt je "phantom fundamental" - lidské ucho slyší fundamentální frekvenci, i když neexistuje. Co kdyby tedy jedna z augumentací fungovala tak, že vymaže některé harmonické frekvence?
    - ale nevím, to je jako kdyby v image recognition byla augumentace vkládání poloprůhledných obdélníků přes části obrázku, jo, lidi poznají obsah obrázku i tak, ale pomohlo by to sítím se líp naučit, co na obrázku je?

- Použít jazztube jako ME dataset http://mir.audiolabs.uni-erlangen.de/jazztube/

- onset detection - pro piano velmi zepšuje výsledky, co to zkusit pro ostatní nástroje? (já vím, piano je charakteristické, ale..?)
    - FFT s různými window size pro onset detection

- pravděpodobně blbost: použít banku bandpass filtrů a pak feedovat do sítě přefiltrované raw signály

- Multitask/autoencoder/regularizace AMT

- evoluční algoritmy v AMT, nebo v piano transcription
	- https://www.google.cz/search?q=evolution+algorithm+music+transcription
