# Hansa-toy-version
Using metrics like Gribskov score, Probability of finding mutant/wild type amino acid at a given psoition, Buried/exposed amino acid data and structurue of amino acid [helix/coil/other], I tried to create a basic version of Hansa, a method that uses SVM to classify mutations into neutral and deleterious
For this analysis I used five sequences: I used the human protien Sodium- and chloride-dependent glycine transporter 2 protien [UniProt ID Q9Y345], its rat an dmouse ortholog [obtained from BLAST ALigner], and then for the fourht protien, I inserted neutral mutations and in the last sequence I inserted deleterious mutations
The data for these mutations for this particualr protien was obtained form the HumVar.tar.gz dataset from the Harvard university Database. The hansa paper also has used this dataset
Like Hansa, I have tried calculating Probability(Wild type), Probability(mutant), Difference b/w both the probabilities, Gribskov score for the posiiton's mutation, buried/exposed data [ 10% RSA < exposed], and strucutre [coil/helix/strand]. The strucutural data for this protien was obtained from "https://services.healthtech.dtu.dk/cgi-bin/webface2.cgi?jobid=6A0C46C300279D39CD285359&wait=20" webpage.
I used SVM, and Cost parameter as 0.1, which affects the strictness of the model
I also converted the data into numerical values to be fed into the model
    position                       wild_type                        mutation       diff       score     acess      strct
0        101                      {'G': 1.0}                      {'G': 1.0}       0.0        0           0           0
1        123            {'F': 0.4, 'S': 0.2}  {'F': 0.4, 'I': 0.4, 'S': 0.2}       0.2       -4           0           0
2        131            {'A': 0.4, 'G': 0.2}  {'A': 0.4, 'E': 0.4, 'G': 0.2}       0.0        2           0           0
3        456  {'K': 0.4, 'T': 0.4, 'N': 0.2}  {'K': 0.4, 'T': 0.4, 'N': 0.2}       0.0        3           0           0
4        462  {'D': 0.4, 'L': 0.4, 'N': 0.2}  {'D': 0.4, 'L': 0.4, 'N': 0.2}       0.0       -3           0           0
5        498  {'Y': 0.4, 'N': 0.4, 'F': 0.2}  {'Y': 0.4, 'N': 0.4, 'F': 0.2}       0.0       -4           0           1
6        766                      {'G': 0.4}  {'G': 0.4, 'H': 0.4, 'R': 0.2}      -0.2       -3           0           0
7        306                              {}            {'P': 0.6, 'V': 0.4}       0.0       -7           0           0
8        425            {'A': 0.6, 'F': 0.4}            {'A': 0.6, 'F': 0.4}       0.0       -1           1           1
9        481            {'W': 0.4, 'A': 0.4}  {'W': 0.4, 'A': 0.4, 'C': 0.2}       0.0       -5           0           0
10       490            {'Y': 0.4, 'S': 0.4}  {'Y': 0.4, 'S': 0.4, 'C': 0.2}       0.4        3           1           0
11       508            {'N': 0.4, 'S': 0.2}  {'N': 0.4, 'C': 0.4, 'S': 0.2}       0.2        4           1           1
12       509            {'S': 0.4, 'T': 0.4}  {'S': 0.4, 'T': 0.4, 'R': 0.2}       0.2        9           1           1

Here, the acess column is the Buired/exposed where 0 is exposed and 1 is buried, strct is the column for structure[helix/strand]
The 'position' column is the the position of the given mutation in the amino aicd sequence [0 indexing]
The 'score' column is the Gribskov score, and the 'diff' is the difference between the probability of a wild type amino acid and mutation amino acid.
To be fed into the SVM model, wild type and mutation column data were converted into numerical values, with a reference vector of [A, R, N, D, C, Q, E, G, H, I, L, K, M, F, P, S, T, W, Y, V]
Therefore the final data fed into the model was
(0,0,1.0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0), (0,0,1.0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0), 0.0, 0, 0, 0  
(0.2,0.4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0), (0.2,0.4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0.4), 0.2, -4, 0, 0  
(0,0,0.2,0,0,0,0,0,0,0,0,0.4,0,0,0,0,0,0,0), (0,0,0.2,0,0,0,0,0,0,0,0,0.4,0,0,0,0,0,0.4,0), 0.0, 2, 0, 0  
(0,0,0,0.2,0,0,0,0,0.4,0,0,0,0,0,0,0,0.4,0,0), (0,0,0,0.2,0,0,0,0,0.4,0,0,0,0,0,0,0,0.4,0,0), 0.0, 3, 0, 0  
(0,0,0,0.2,0,0,0,0,0,0,0,0,0,0,0.4,0.4,0,0,0), (0,0,0,0.2,0,0,0,0,0,0,0,0,0,0,0.4,0.4,0,0,0), 0.0,-3, 0, 0  
(0,0.2,0,0.4,0,0,0,0,0,0.4,0,0,0,0,0,0,0,0,0), (0,0.2,0,0.4,0,0,0,0,0,0.4,0,0,0,0,0,0,0,0,0), 0.0,-4, 0, 1  
(0,0,0.4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0), (0,0,0.4,0,0.2,0,0,0,0,0,0,0,0,0.4,0,0,0,0,0), -0.2, -3, 0, 0  
(0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0), (0,0,0,0,0,0.4,0,0,0,0,0,0,0.6,0,0,0,0,0,0), 0.0, -7, 0, 0  
(0,0.4,0,0,0,0,0,0,0,0,0,0.6,0,0,0,0,0,0,0), (0,0.4,0,0,0,0,0,0,0,0,0,0.6,0,0,0,0,0,0,0), 0.0, -1, 1,1  
(0,0,0,0,0,0,0,0,0,0,0.4,0.4,0,0,0,0,0,0,0), (0,0,0,0,0,0,0,0.2,0,0,0.4,0.4,0,0,0,0,0,0,0), 0.0, -5, 0, 0  
(0.4,0,0,0,0,0,0,0,0,0.4,0,0,0,0,0,0,0,0,0), (0.4,0,0,0,0,0,0,0.2,0,0.4,0,0,0,0,0,0,0,0,0), 0.4, 3, 0, 0  
(0.2,0,0,0.4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0), (0.2,0,0,0.4,0,0,0,0.4,0,0,0,0,0,0,0,0,0,0,0), 0.2, 4, 1, 1  
(0.4,0,0,0,0,0,0,0,0.4,0,0,0,0,0,0,0,0,0,0), (0.4,0,0,0,0.2,0,0,0,0.4,0,0,0,0,0,0,0,0,0,0), 0.2, 9, 1, 1

I had used 7 neutral mutations and 6 deleterious mutations, the mdoel predicted 7/7 nuetral mutations and predicted 4/6 deleterious mutatiosn correctly. There were two false positives for neutral.
There are significant drawbacks to my pipeline:
1) The training dataset is very small, not lareg enough to arrive at statistically significant analyses.
2) The dataset does not contain equal amount of information for both types.

This toy version of Hansa was thus implemented.
I also used the internet and AI to help me understand the concepts in the paper and to help me debugg the code.

