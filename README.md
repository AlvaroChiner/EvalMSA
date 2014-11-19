EvalMSA
=======

A perl-based application to evaluate multiple sequence alignments

What is this tool used for?
	In a multiple sequence alignment (MSA) sometimes appear sequences that phylogenetically diverge markedly from the rest. Those sequences can influence negative on the MSA. This tool evaluate the alignment by means of determining the weight each sequence have over the quality of the whole MSA.
	In short alignments is easy to identify those divergent sequences. They may stand out from the others for having a high number of gaps, inducing more gaps in the rest of sequences or having mismatches.
	This tool not only scores specifically each sequence, but also identifies those who induce more gaps in the others and those having higher number of gaps.
	
Prerequisites
	The program interacts with the R statistical language in order to print the results.  We should have installed the R modules needed for graphic representations to have the results presented properly.

Usage instructions
Input Data

When running the program, the user must introduce some data:
1. MSA file path. If the file is located in the same directory where the program is executing just the name of the file is necessary. The program only accepts FASTA format files.
2. Scoring matrix file path. If the file is located in the same directory where the program is executing just the name of the file is necessary. The file must be a plain-text file, with the information displayed as showed:
“
   A  R  N  D  C  Q  E  G  H  I  L  K  M  F  P  S  T  W  Y  V   
A  4 -1 -2 -2  0 -1 -1  0 -2 -1 -1 -1 -1 -2 -1  1  0 -3 -2  0 
R -1  5  0 -2 -3  1  0 -2  0 -3 -2  2 -1 -3 -2 -1 -1 -3 -2 -3 
N -2  0  6  1 -3  0  0  0  1 -3 -3  0 -2 -3 -2  1  0 -4 -2 -3    
D -2 -2  1  6 -3  0  2 -1 -1 -3 -4 -1 -3 -3 -1  0 -1 -4 -3 -3    
C  0 -3 -3 -3  9 -3 -4 -3 -3 -1 -1 -3 -1 -2 -3 -1 -1 -2 -2 -1    
Q -1  1  0  0 -3  5  2 -2  0 -3 -2  1  0 -3 -1  0 -1 -2 -1 -2    
E -1  0  0  2 -4  2  5 -2  0 -3 -3  1 -2 -3 -1  0 -1 -3 -2 -2    
G  0 -2  0 -1 -3 -2 -2  6 -2 -4 -4 -2 -3 -3 -2  0 -2 -2 -3 -3    
H -2  0  1 -1 -3  0  0 -2  8 -3 -3 -1 -2 -1 -2 -1 -2 -2  2 -3    
I -1 -3 -3 -3 -1 -3 -3 -4 -3  4  2 -3  1  0 -3 -2 -1 -3 -1  3    
L -1 -2 -3 -4 -1 -2 -3 -4 -3  2  4 -2  2  0 -3 -2 -1 -2 -1  1    
K -1  2  0 -1 -3  1  1 -2 -1 -3 -2  5 -1 -3 -1  0 -1 -3 -2 -2    
M -1 -1 -2 -3 -1  0 -2 -3 -2  1  2 -1  5  0 -2 -1 -1 -1 -1  1    
F -2 -3 -3 -3 -2 -3 -3 -3 -1  0  0 -3  0  6 -4 -2 -2  1  3 -1    
P -1 -2 -2 -1 -3 -1 -1 -2 -2 -3 -3 -1 -2 -4  7 -1 -1 -4 -3 -2    
S  1 -1  1  0 -1  0  0  0 -1 -2 -2  0 -1 -2 -1  4  1 -3 -2 -2    
T  0 -1  0 -1 -1 -1 -1 -2 -2 -1 -1 -1 -1 -2 -1  1  5 -2 -2  0    
W -3 -3 -4 -4 -2 -2 -3 -2 -2 -3 -2 -3 -1  1 -4 -3 -2 11  2 -3    
Y -2 -2 -2 -3 -2 -1 -2 -3  2 -1 -1 -2 -1  3 -3 -2 -2  2  7 -1    
V  0 -3 -3 -3 -1 -2 -2 -3 -3  3  1 -2  1 -1 -2 -2  0 -3 -1  4
”

Pay attention to the fact that each row (except the first one) started with the abbreviation of the corresponding aminoacid or nucleotide. Besides, the white spaces between each numeric vale are important (2 spaces before positive values and 1 before negative ones). In case of having any doubt, just examine the scoring matrix files provided with this material.

3. Reference sequences (outgroups). Name of the sequences that the program must obviate in the analysis, separated by commas. By default, none sequence is set.

4. Gap penalty. A numeric value used for scoring each gap position. If none is set, the program will calculate a value that is equal to the minimum score found in the scoring matrix minus the standard deviation of all the scores of this matrix.

5. Gap threshold. A numeric value, between 0 and 1. Is the relative percentage of gaps that must be in a column of the MSA in order the program identifies the sequences that in this position have an aminoacid/nucleotide as being “gap generators”. For example, if 0.66 is set in all the columns with 66% or more percentage of gaps the tool will identify those sequences with aminoacid/nucleotides as gap generators, and will penalize them. The default value is 0.66.


On the other hand, the input data can be given to the program as execution arguments: 

./evaluador [MSA_path] [matriz_path] [OPTIONAL ARGUMENTS]

ARGUMENTS:

-o GENE1,GENE2,GENE3...
	 Each of the reference genes, sepparated by commas.
-p VALUE
	Gap penalty value
-t VALUE
	Gap threshold value

If the optional arguments are not specified, the program will use the default values.

Output Data
	Before the execution, three different files are created
MSA_file_results.txt: Include the pre-analisys data, the alignment analisys data and the potential errors.
Pre-analisys: Shows the average, median, standard deviation, quartiles, percentiles and the outliers taking as data the original length of the sequences.
Alignment analisys: Shows information related with SP method and the alignment score calculated. Points out the less scored sequence in the alignment, the sequence with higher number of gaps and the sequence who induces more gaps in the rest of the sequences. 
Warning: In case of finding the simbols *, ?, or any other that is not included in the scoring matrix, list the sequence and the position associated. 

• MSA_file_output.pdf: Contains some plots generated with R.
A boxplot generated with the original length of the sequences (before aligning). 
A histogram that represents the scores distribution of the sequences. In yellow is identified the place where the sequence with a higher number of gaps is set. Also in magenta is identified the place of the sequence who induces more gaps in the rest.
A histogram with the gapiness values.

•MSA_files_values.csv. A .csv file that contains the number of gaps, the score and the gapiness value of all the sequences of the alignment.
