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
## Anvi'o pangenome worflow (v. 5)
```
#load anvio with Anaconda
source activate anvio5

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
anvi-gen-genomes-storage -e anvi_gen.txt -o lacto-GENOMES.db

The new genomes storage ......................: lacto-GENOMES.db (v6, signature: hashe4d2e738)
Number of genomes ............................: 4,520 (internal: 0, external: 4,520)
Number of gene calls .........................: 10,360,105
Number of partial gene calls .................: 284,255


#pangenome analysis
anvi-pan-genome -g lacto-GENOMES.db -n all_lacto --enforce-hierarchical-clustering -T 8 --exclude-partial-gene-calls --min-occurrence 50

#view analysis
anvi-display-pan -g lacto-GENOMES.db -p [group]/[group]-PAN.db
```
## Calculate ANI between genomes
```
anvi-compute-ani -o all_lacto_ANI -e anvi_gen.txt -T 8 -p 
```

## Make genome tree
```
#generate genome tree
anvi-get-sequences-for-hmm-hits --external-genomes anvi_gen.txt -o concatenated_lacto-proteins.fa  --gene-names Ribosomal_L2,Ribosomal_L3,Ribosomal_L4,Ribosomal_L5,Ribosomal_L6,Ribosomal_L14,Ribosomal_L16,Ribosomal_L18e,Ribosomal_L18p,Ribosomal_L22,Ribosomal_S10,Ribosomal_S17,Ribosomal_S19,Ribosomal_S3_C,Ribosomal_S8 --return-best-hit --get-aa-sequence --concatenate

anvi-gen-phylogenomic-tree -f concatenated-proteins.fa -o pantoea_tree.tree
```

## Extract 16S rRNA gene sequences from HMMs
```
anvi-get-sequences-for-hmm-hits -e anvi_gen.txt --gene-names Bacterial_16S_rRNA -o lacto_16s.fna --return-best-hit --hmm-sources Rinke_et_al,Ribosomal_RNAs,Campbell_et_al
```

## Get summary stats of genomes
```
anvi-script-add-default-collection -c lacto_GENOMES.db
```

## Export gene calls
```
#anvi-migrate-db *.db

for i in *.db
do 
anvi-export-functions -c $i -o /media/pattyjk/Elements/lactobacilliales_genomes/gene_calls/fun_$i
done
```


