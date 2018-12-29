po algoritmu chceme:
- správnou výšku tónů melodie (pitch estimation)
- jestli melodie zní nebo ne (voicing detection)


- (mirex formát) - 10 ms hop, seznam časů a frekvencí, 0 = žádná melodie

# Measures
- per-frame comparison (MIREX 2005)
    - Voicing Recall Rate: The proportion of frames labeled as melody frames in the ground truth that are estimated as melody frames by the algorithm.
    - Voicing False Alarm Rate: The proportion of frames labeled as non-melody in the ground truth that are mis- takenly estimated as melody frames by the algorithm.
    - Raw pitch accuracy: The proportion of melody frames in the ground truth for which fτ is considered correct
    - Raw Chroma Accuracy: As raw pitch accuracy, except that both the estimated and ground truth f0 sequences are mapped onto a single octave.
    - Overall Accuracy: this measure combines the perfor- mance of the pitch estimation and voicing detection tasks to give an overall performance score for the system.