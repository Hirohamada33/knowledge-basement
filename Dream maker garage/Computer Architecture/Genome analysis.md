Genome analysis is one of the motivation / application of improving the computer architecture. It starts with an idea that to build *an embedded device that can perform comprehensive genome analysis in real time (degree of minutes)*
Genome analysis can give good understanding / information about the patient, including:
1. Which of these DNAs does this DNA segment match with?
2. What is the likely genetic disposition of this patient to this drug?
3. What disease/condition might this particular DNA/RNA piece associated with?

#### Genome sequencing
The goal of sequencing is to find the complete sequence of A, C, G, T’s in DNA (or RNA).
The challenges are
1. There is no machine that takes long DNA as an input, and gives the complete sequence as output
2. All sequencing machines chop DNA into pieces and identify relatively small pieces (but not how they fit together)

![[截圖 2024-12-28 上午11.45.35.png]]
Hence nowadays the genome sequencing process is as above. A large DNA molecule is firstly chopped into small segments, then parse to machine for read. The reading then requires to match the fragment into a particular region of the reference sequence. 


The cost of sequencing, interestingly, by the introduction of high-throughput sequencing, now the rate of cost has significantly decreased. The trendline of the cost of transistor, following Moore's law, is compared. Note that the cost of sequencing is significantly cheaper than transistor. 
![[截圖 2024-12-28 上午11.47.52.png]]

Genome analysis itself is a very difficult question to tackle, as the following example it can be seen that the computationally extensive nature. 
![[截圖 2024-12-28 上午11.56.25.png]]

The image presents the method of metagenomics, genome assembly and de novo sequencing. 
![[截圖 2024-12-28 上午11.57.41.png]]

Essentially there are two questions to be asked:
1. Given some long sequences, find the matches and differences
2. Given some short sequences, identify the approximate species cluster for genomically unknown organisms. 


A bottleneck in mapping exist that the read mapping itself is taking much time than the provide of genome data.  
![[截圖 2024-12-28 上午11.59.59.png]]
##### Method
There are some method of reconstructing the sequence of genome segments, including **read mapping** and **De novo assembly** 
![[截圖 2024-12-28 上午11.51.53.png]]
The Human Genome Project (HGP) provides a complete and accurate sequence of all DNA base pairs that make up the human genome and finds 20,000 to 25,000 human genes, which means there are sufficient database for the reference genome. 

**Read mapping** is the method of aligning a segment to a reference genome and go through each position to find the best matches (and variations). 
**De novo assembly**, on the other hand, is purely using algorithmic approach to merge the segments to reconstruct the sequence. 
Nowadays reading mapping is more popular, for its less computationally extensive nature and matureness of the algorithm. 


Note that in genome analysis, it was never really the **perfect match** that is in the interest, but the **approximate sequence matching (ASM)** as there are error sources presented in genome (individual mutation) and machine reads. 


##### Algorithm
The read alignment algorithm uses **dynamic programming** (see [[Artificial Intelligence/notes/Dynamic programming|Dynamic programming]], string edit distance). 
![[截圖 2024-12-28 下午12.02.58.png]]

The presented challenges are:
1. Need to find many mappings of each read
2. Need to tolerate small variances/errors in each read. (Each individual is different: Subject’s DNA may slightly differ from the reference (Mismatches, insertions, deletions))
3. Need to map each read very fast (i.e., performance is important). (Human DNA is 3.2 billion base pairs long → Millions to billions of reads (State-of-the-art mappers take weeks to map a human’s DNA))

#### Further approaches
##### Approximate string comparisons
##### SIMD Acceleration
##### FPGA-Based Alignment Filtering