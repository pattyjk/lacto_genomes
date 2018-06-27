## Preparng files for Anvio
```
#in each folder wtih genomes of interest perform these scripts
#extract all the files
ls *tar.gz | xargs -n1 tar -xzf

#you can move the files to another place, or delete them.
#move is mv *.tar.gz
#delete is rm *.tar.gz

#the file of interest is [genome_name].fna, so we'll search for all .fna files
find . -name \*.fna -exec cp {} . \;

#now delet extra files
rm *.genes.fna
rm *.intergenic.fna

#with that, the filesa are ready for anvio
```
## Anvi'o pangenome worflow (v. 4)
```
#load anvio with Anaconda
source activate anvio4

#fix fasta files to be compatible with Anvio
mkdir fixed_reps
mkdir fixed_fasta

#run for loop
for i in *.fna
do
    anvi-script-reformat-fasta $i -o fixed_fasta/$i --simplify-names --report-file=fixed_reps/$i
done

#make contigs database for each genome
mkdir contigs_db
cd fixed_fasta

for i in *.fna
do
    anvi-gen-contigs-database -f $i -o /home/pattyjk/Desktop/lacto_genomes/[group_folder]/contigs_db/$i.db -n '[group_name]'
done

#generate COGs
cd ..
cd contigs_db

#make a contigis database if not already done (anvi-setup-ncbi-cogs --num-threads 6)
for i in *.db
do
anvi-run-ncbi-cogs -c $i --num-threads 8
done

#run HMMs for single copy genes
#cd contigs_db 

for i in *.db
do
anvi-run-hmms -c $i -T 8
done

cd ..
```

## Make a Anvio genomes database file with bash and R
```
#change back to root
cd /

#write absoulte files paths to a file
ls -R1 ./home/pattyjk/Desktop/lacto_genomes/[group_folder]/contigs_db |    while read l; do case $l in *:) d=${l%:};; "") d=;; *) echo "$d/$l";; esac; done > /home/pattyjk/Desktop/lacto_genomes/[group_folder]/db_files.txt

#fix file name
sed -i 's/\.\//\//g' /home/pattyjk/Desktop/lacto_genomes/[group_folder]/db_files.txt

#change back to folder 
cd /home/pattyjk/Desktop/lacto_genomes/[group_folder]

#make a genomes name file and fix it up
sed 's/\///g' db_files.txt > gen_names.txt
sed -i 's/homepattyjkDesktoplacto_genomesleucono_genomescontigs_//g' gen_names.txt
sed -i 's/\.//g' gen_names.txt
sed -i 's/fnadb//g' gen_names.txt

#open R to generate database
R

#read in file locations
anvi_gen<-read.delim('db_files.txt', header=F)
names(anvi_gen)<-'contigs_db_path'

#read in genome names
anvi_names<-read.delim('gen_names.txt', header=F)
names(anvi_names)<-'name'

#catenate
anvi_gen<-cbind(anvi_names, anvi_gen)

#write file
write.table(anvi_gen, 'anvi_gen.txt', quote=F, row.names=F, sep='\t')

#exit R
quit()
```


## Catenate genomes into a single database
```
anvi-gen-genomes-storage -e anvi_gen.txt -o [group]-GENOMES.db

#pangenome analysis
anvi-pan-genome -g [group]-GENOMES.db -n [group_name]

#view analysis
anvi-display-pan -g [group]-GENOMES.db -p [group]/[group]-PAN.db
```
