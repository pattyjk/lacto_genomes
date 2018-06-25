## Anvi'o pangenome worflow
```
#load anvio with Anaconda
source activate anvio4


#fix fasta file headers
anvi-script-reformat-fasta 2501651209.fna -o 2501651209_f.fna --simplify-names --report-file=2501651209_rep

anvi-script-reformat-fasta 2502171170.fna -o 2502171170_f.fna --simplify-names --report-file=2502171170_rep


#make contigs database for each genome
anvi-gen-contigs-database -f 2501651209_f.fna -o 2501651209_contig.db -n 'Lacto'

anvi-gen-contigs-database -f 2502171170_f.fna -o 2502171170_contig.db -n 'Lacto'

#generate COGs


#catenate genomes into a single database
anvi-gen-genomes-storage -e dbase.txt -o lacto-GENOMES.db

#pangenome analysis
```
