Extracting the Ramp Sequence from genes
ExtRamp.py
Created By: Logan Brase and Justin Miller; version 2.0 by Matthew Cloward
Email: mattcloward@byu.edu

ExtRamp is a tool to extract the Ramp sequence from the beginning of genes.
It uses the tAI values (user provided) or codon proportions to determine the speed of translation
and the appropriate cut off point for the Ramp sequence.

## ARGUMENT OPTIONS

ExtRamp requires 1 user input at the runtime:
```
-i input	INPUT FASTA file containing the cds gene sequences of interest (.gz file extension required for gzipped files)
```

### OPTIONAL ARGUMENTS
```
-a tAI                      INPUT CSV file containing tAI values. Two formats are accepted as shown in the two included examples ([S.cerevisiae_tAI_Values](./example_files/S_cerevisiae_tAI_Values.csv) and [Ecoli_tAI_Values](./example_files/Ecoli_tAI_Values.csv))
-u rscu                     INPUT FASTA file used to compute relative synonymous codon usage and relative adaptiveness of codons
-o ramp                     OUTPUT FASTA file to write the ramp sequences to
-y scores                   OUTPUT TSV file to write ramp strength and robustness scores
-j wij                      OUTPUT CSV file to write calculated codon wij (efficiency) values
-l vals                     OUTPUT FASTA-like file to write tAI/relative adaptiveness values for ribosome-smoothed efficiency at each window. Format: Header newline list of values separated by comma and space
-p speeds                   OUTPUT FASTA-like file to write tAI/relative adaptiveness values for each position in the sequence from the codon after the start codon to the codon before the stop codon. Format: Header newline list of values separated by comma and space
-n noRamp                   OUTPUT text file to write the headers of sequences that contained no ramp sequence
-z removedSequences         OUTPUT text file to write the headers of sequences that are filtered out (e.g., sequence not long enough or not divisible by 3)
-x afterRamp                OUTPUT FASTA file containing gene sequences after the identified ramp sequence
-t threads                  The number of threads used to run the program; default: all available
-w window                   The number of codons in the ribosome window; default: 9 (codons)
-s stdev                    The number of standard deviations below the mean the cutoff value will be; default: not used
-d stdevRampLength          The number of standard deviations in the lengths of the ramp sequences; default: not used
-m middle                   The type of statistic used to measure the middle (consensus) efficiency. Options are 'hmean','mean', 'gmean', and 'median'; default: 'hmean' 
-v verbose                  Flag to print progress to standard error
-r rna                      Flag for RNA sequences; default: DNA.
-f determine_cutoff         Flag to determine outlier percentages for mean cutoff based on species FASTA file; default: the local minimum in first 8 percent of gene
-c cutoff                   Cutoff for where the local minimum must occur in the gene for a ramp to be calculated. If --determine_cutoff (-f) is used, then this value may change. Is not used if standard deviations are set; default: 8
-e determine_cutoff_percent Cutoff for determining percent of gene that is in an outlier region. Used in conjunction with -f; default: true outliers. Other options include numbers from 0-99, which indicate the region of a box plot. For instance, 75 means the 75th quartile or above.
-q seqLength                Minimum nucleotide sequence length; default: 300 (nucleotides)
-g prevalidate              Flag to truncate sequences at early stop codons before testing if sequences are valid
```

NOTE: Only the standard codon table is supported because many codon tables have ambiguous codons that encode for more than one amino acid. Since we use the relative codon adaptiveness for each amino acid, we cannot account for ambiguous codons.

## REQUIREMENTS
ExtRamp.py uses Python version 3.12.2

Required libraries to install are available in the [requirements.txt](./requirements.txt) file. Use the following command to install them:
```
pip3 install -r requirements.txt
```

## USAGE
If you do not have tAI values, check the stAIcalc [here](http://tau-tai.azurewebsites.net/). The stAIcalc database contains a large list of the species with known tAI values. You must select a valid FASTA file and then click submit for the tAI values to be printed at the bottom. The FASTA file does not need to be from the correct species, but it won't print the tAI values until a FASTA file is selected. The values can then be exported to a CSV File.

### WITH TAI FILE (RECOMMENDED)
```
python ExtRamp.py -i path/to/SEQUENCES.fasta.gz -a path/to/tAI.csv -o path/to/OUTFILE.fasta 
```
### WITHOUT TAI FILE
```
python ExtRamp.py -v -i path/to/SEQUENCES.fasta.gz -o path/to/OUTFILE.fasta
```

## EXAMPLE USAGE

### WITH TAI FILE
The following command runs ExtRamp.py on the provided S_cerevisiae example files in the example_files folder:
```
python ExtRamp.py -v -t 1 -i example_files/Saccharomyces_cerevisiae.gz -a example_files/S_cerevisiae_tAI_Values.csv -o outTest.fasta 
```
The output should match the [S_cerevisiae_output.fasta](./example_files/S_cerevisiae_output.fasta) file in the example_files folder (note: due to multithreading, the order of the sequences might vary)
The progress written to standard error should look similar to the [S_cerevisiae.log](./example_files/S_cerevisiae.log) file.

### WITHOUT TAI FILE
```
python ExtRamp.py -v -t 1 -i example_files/Homo_sapiens.gz -o output.fasta
```
For us, that command takes approximately 5 seconds of user time. For small files like this one, additional threads can slow it down (it took 10 seconds for 16 cores). Only larger files (~100-200 MB) are benefitted by additional cores.

output.fasta should match [example_files/Homo_sapiens_output.fasta](./example_files/Homo_sapiens_output.fasta) (note: the order of the sequences might vary).

### WITH RAMP SCORES
```
python ExtRamp.py -v -t 4 -i example_files/Homo_sapiens.gz -a example_files/S_cerevisiae_tAI_Values.csv -o output.fasta -y scores.tsv
```
The scores.tsv file should match [example_files/S_cerevisiae_scores.tsv](./example_files/S_cerevisiae_scores.tsv). The ramp strength score is the 6th column and the ramp robustness score is the 11th column.

##########################

Thank you, and happy researching!
