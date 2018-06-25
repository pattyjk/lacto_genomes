## Anvi'o pangenome worflow (v. 4)
```
#load anvio with Anaconda
source activate anvio4

#fix fasta files to be compatible with Anvio

for i in *.fna
do
    anvi-script-reformat-fasta $i -o fixed_fasta/$i --simplify-names --report-file=fixed_reps/$i
done

#make contigs database for each genome

mkdir contigs_db
cd fixed_fasta

for i in *.fna
do
    anvi-gen-contigs-database -f $i -o /home/pattyjk/Desktop/anvio_test/contigs_db/$i.db -n 'Lacto'
done

#generate COGs
cd ..
mkdir contigs_cog
cd contigs_db

#make a contigis database if not already done (anvi-setup-ncbi-cogs --num-threads 6)

for i in *fna
do

done

#catenate genomes into a single database

anvi-gen-genomes-storage -e dbase.txt -o lacto-GENOMES.db

#pangenome analysis
```

## Make a Anvio genomes database file
```
#change back to root
cd /

#write absoulte files paths to a file
ls -R1 ./home/pattyjk/Desktop/anvio_test/contigs_db |    while read l; do case $l in *:) d=${l%:};; "") d=;; *) echo "$d/$l";; esac; done > /home/pattyjk/Desktop/anvio_test/locations.txt

#fix file name
sed -i 's/\.\//\//g' /home/pattyjk/Desktop/anvio_test/locations.txt

#change back to folder 
cd /home/pattyjk/Desktop/anvio_test
```
