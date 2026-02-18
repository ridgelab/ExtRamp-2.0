# ExtRamp 2.0

ExtRamp is a tool to extract the ramp sequence from the beginning of mRNA coding sequences. ExtRamp 2.0 is up to ***108 times*** faster than ExtRamp 1.0 and up to ***3 times*** more memory efficient.

Ramps are inefficiently translated codon clusters at the beginning of coding sequences, predicted to evenly space ribosomes along a transcript, reducing ribosomal collisions and increasing protein expression.

## Links
ExtRamp 2.0 Paper- coming soon!\
[ExtRamp 2.0 Benchmark GitHub](https://github.com/ridgelab/ExtRamp-2.0-Benchmarking)\
[ExtRamp 1.0 Paper](https://academic.oup.com/nar/article/47/3/1123/5289485)\
[ExtRamp 1.0 GitHub](https://github.com/ridgelab/ExtRamp)

## ARGUMENTS

ExtRamp 2.0 options are documented at the top of [ExtRamp.py](./ExtRamp.py) in the makeArgParser function. The only required argument is the input file (-i or --input). All other arguments are optional.

### NEW ARGUMENTS
In addition to the arguments in [ExtRamp 1.0](https://github.com/ridgelab/ExtRamp), ExtRamp 2.0 provides the following new arguments:
```
-y (--scores):      OUTPUT TSV file to write ramp strength and robustness scores. The output columns are described [here](./scores.md)
-j (--wij)          OUTPUT CSV file to write calculated codon wij (efficiency) values. This file can be treated as a tAI file with the -a (--tAI) option in subsequent commands
-g (--prevalidate)  Flag to truncate sequences at early stop codons before testing if sequences are valid. Assumes canonical stop codons: "TAA", "TAG", and "TGA"
```

### IMPROVED ARGUMENTS
The output of -l (--vals) has been modified to match the FASTA-like structure of the -p (--speeds) file: for each sequence, a header followed by a list of window mean speeds separated by a comma and space.\
The -v (--verbose) option now prints more info, such as the number of sequences filtered out due to being to short or not divisible by 3 and the duration of each task.\
Adding ".gz" to the end of output file paths on the command line now writes results as gzipped files. Arguments this works for are -o (--ramp), -p (--speeds), -x (--afterRamp), -n (--noRamp), and -y (--scores).

## REQUIREMENTS
ExtRamp.py was tested and developed in Python version 3.12.2.

Required libraries are available in the [requirements.txt](./requirements.txt) file. Use the following command to install them:
```
pip3 install -r requirements.txt
```

## USAGE
If you do not have tAI values, check the stAIcalc [here](http://tau-tai.azurewebsites.net/). The stAIcalc database contains a large list of the species with known tAI values. You must select a valid FASTA file and then click submit for the tAI values to be printed at the bottom. The FASTA file does not need to be from the correct species, but it won't print the tAI values until a FASTA file is selected. The values can then be exported to a CSV File.

### WITH TAI FILE (RECOMMENDED)
```
python ExtRamp.py -i path/to/SEQUENCES.fasta.gz -a path/to/tAI.csv -o path/to/OUTFILE.fasta 
```
#### EXAMPLE 
The following command runs ExtRamp.py on the provided S_cerevisiae example files in the example_files folder:
```
python ExtRamp.py -v -t 1 -i example_files/Saccharomyces_cerevisiae.gz -a example_files/S_cerevisiae_tAI_Values.csv -o outTest.fasta 
```
The output should match the [S_cerevisiae_output.fasta](./example_files/S_cerevisiae_output.fasta) file in the example_files folder (note: due to multithreading, the order of the sequences might vary)
The progress written to standard error should look similar to the [S_cerevisiae.log](./example_files/S_cerevisiae.log) file.

### WITHOUT TAI FILE
```
python ExtRamp.py -v -i path/to/SEQUENCES.fasta.gz -o path/to/OUTFILE.fasta
```
#### EXAMPLE
```
python ExtRamp.py -v -t 1 -i example_files/Homo_sapiens.gz -o output.fasta
```
For us, that command takes approximately 5 seconds of user time. For small files like this one, additional threads can slow it down (it took 10 seconds for 16 cores). Only larger files (~100-200 MB) are benefitted by additional threads.

output.fasta should match [example_files/Homo_sapiens_output.fasta](./example_files/Homo_sapiens_output.fasta) (note: the order of the sequences might vary).

### EXAMPLE WITH RAMP SCORES
```
python ExtRamp.py -v -t 4 -i example_files/Saccharomyces_cerevisiae.gz -a example_files/S_cerevisiae_tAI_Values.csv -o outTest.fasta -y scores.tsv
```
The scores.tsv file should match [example_files/S_cerevisiae_scores.tsv](./example_files/S_cerevisiae_scores.tsv). The ramp strength score is the 6th column and the ramp robustness score is the 11th (last) column.

## CONTACT
Questions? Open a new issue on GitHub or email us at: mattcloward@byu.edu

Thank you, and happy researching!