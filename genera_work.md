## Working with individual genera of LAB
```
#copy the database files for each genome to their own folder
mkdir entero
mkdir strepto
mkdir lactob
mkdir pedio
mkdir lactoc
mkdir leucono
mkdir weis

cd contigs_db
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/entero_genomes.txt cp -t /media/pattyjk/Elements/lactobacilliales_genomes/entero/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/strepto_genomes.txt cp -t /media/pattyjk/Elements/lactobacilliales_genomes/strepto/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/lactobac_genomes.txt cp -t /media/pattyjk/Elements/lactobacilliales_genomes/lactob/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/pedio_genomes.txt cp -t /media/pattyjk/Elements/lactobacilliales_genomes/pedio/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/lactococc_genomes.txt cp -t /media/pattyjk/Elements/lactobacilliales_genomes/lactoc/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/leucono_genomes.txt cp -t /media/pattyjk/Elements/lactobacilliales_genomes/leucono/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/weisel_genomes.txt cp -t /media/pattyjk/Elements/lactobacilliales_genomes/weis/
cd ..

#Number of genomes fo each genus
ls -l ./folder | egrep -c '^-'

#Weisellia- 23
#Enterococcus-820
#Streptococcus- 2765
#Lactococcus- 106
#Lactobacillus- 759
#Leuconostoc-8
#Pediococcus- 40
```

## create genome databases for each 
```
# make a file with paths
ls -R1 ./media/pattyjk/Elements/lactobacilliales_genomes/weis |  while read l; do case $l in *:) d=${l%:};; "") d=;; *) echo "$d/$l";; esac; done > /media/pattyjk/Elements/lactobacilliales_genomes/weisel_db_files.txt
ls -R1 ./media/pattyjk/Elements/lactobacilliales_genomes/entero |  while read l; do case $l in *:) d=${l%:};; "") d=;; *) echo "$d/$l";; esac; done > /media/pattyjk/Elements/lactobacilliales_genomes/entero_db_files.txt
ls -R1 ./media/pattyjk/Elements/lactobacilliales_genomes/lactob |  while read l; do case $l in *:) d=${l%:};; "") d=;; *) echo "$d/$l";; esac; done > /media/pattyjk/Elements/lactobacilliales_genomes/lactob_db_files.txt
ls -R1 ./media/pattyjk/Elements/lactobacilliales_genomes/lactoc |  while read l; do case $l in *:) d=${l%:};; "") d=;; *) echo "$d/$l";; esac; done > /media/pattyjk/Elements/lactobacilliales_genomes/lactoc_db_files.txt
ls -R1 ./media/pattyjk/Elements/lactobacilliales_genomes/leucono |  while read l; do case $l in *:) d=${l%:};; "") d=;; *) echo "$d/$l";; esac; done > /media/pattyjk/Elements/lactobacilliales_genomes/leucono_db_files.txt
ls -R1 ./media/pattyjk/Elements/lactobacilliales_genomes/pedio |  while read l; do case $l in *:) d=${l%:};; "") d=;; *) echo "$d/$l";; esac; done > /media/pattyjk/Elements/lactobacilliales_genomes/pedio_db_files.txt
ls -R1 ./media/pattyjk/Elements/lactobacilliales_genomes/strepto |  while read l; do case $l in *:) d=${l%:};; "") d=;; *) echo "$d/$l";; esac; done > /media/pattyjk/Elements/lactobacilliales_genomes/strepto_db_files.txt

#remove "." from db_files
cd ./media/pattyjk/Elements/lactobacilliales_genomes/
for i in *_db_files.txt
do 
sed -i 's/\.\//\//g' $i
done

#make a name file
sed 's/\///g' strepto_db_files.txt > strepto_gen_names.txt
sed 's/\///g' entero_db_files.txt > entero_gen_names.txt
sed 's/\///g' weisel_db_files.txt > weisel_gen_names.txt
sed 's/\///g' lactoc_db_files.txt > lactoc_gen_names.txt
sed 's/\///g' lactob_db_files.txt > lactob_gen_names.txt
sed 's/\///g' pedio_db_files.txt > pedio_gen_names.txt
sed 's/\///g' leucono_db_files.txt > leucono_gen_names.txt

#fix file names
for i in *gen_names.txt
do
sed -i 's/mediapattyjkElementslactobacilliales_genomes//g' $i
done

for i in *_gen_names.txt
do
sed -i 's/\.//g' $i
done

for i in *_gen_names.txt
do
sed -i 's/fnadb//g' $i
done
```

## Name genomes file in R
```
#open R to generate database
R

#read in file locations
weisel_gen<-read.delim("weisel_db_files.txt", header=F)
names(weisel_gen)<-'contigs_db_path'
strepto_gen<-read.delim("strepto_db_files.txt", header=F)
names(strepto_gen)<-'contigs_db_path'
entero_gen<-read.delim("entero_db_files.txt", header=F)
names(entero_gen)<-'contigs_db_path'
pedio_gen<-read.delim("pedio_db_files.txt", header=F)
names(pedio_gen)<-'contigs_db_path'
lactoc_gen<-read.delim("lactoc_db_files.txt", header=F)
names(lactoc_gen)<-'contigs_db_path'
lactob_gen<-read.delim("lactob_db_files.txt", header=F)
names(lactob_gen)<-'contigs_db_path'
leucono_gen<-read.delim("leucono_db_files.txt", header=F)
names(leucono_gen)<-'contigs_db_path'

#read in genome names
weisel_names<-read.delim("weisel_gen_names.txt", header=F)
names(weisel_names)<-'name'
strepto_names<-read.delim("strepto_gen_names.txt", header=F)
names(strepto_names)<-'name'
entero_names<-read.delim("entero_gen_names.txt", header=F)
names(entero_names)<-'name'
lactoc_names<-read.delim("lactoc_gen_names.txt", header=F)
names(lactoc_names)<-'name'
lactob_names<-read.delim("lactob_gen_names.txt", header=F)
names(lactob_names)<-'name'
pedio_names<-read.delim("pedio_gen_names.txt", header=F)
names(pedio_names)<-'name'
leucono_names<-read.delim("leucono_gen_names.txt", header=F)
names(leucono_names)<-'name'

#catenate
weisel_gen<-cbind(weisel_names, weisel_gen)
strepto_gen<-cbind(strepto_names, strepto_gen)
entero_gen<-cbind(entero_names, entero_gen)
lactoc_gen<-cbind(lactoc_names, lactoc_gen)
lactob_gen<-cbind(lactob_names, lactob_gen)
pedio_gen<-cbind(pedio_names, pedio_gen)
leucono_gen<-cbind(leucono_names, leucono_gen)


#write file
write.table(weisel_gen, 'weisel_anvi_genomes.txt', quote=F, row.names=F, sep='\t')
write.table(strepto_gen, 'strepto_anvi_genomes.txt', quote=F, row.names=F, sep='\t')
write.table(entero_gen, 'entero_anvi_genomes.txt', quote=F, row.names=F, sep='\t')
write.table(lactoc_gen, 'lactoc_anvi_genomes.txt', quote=F, row.names=F, sep='\t')
write.table(lactob_gen, 'lactob_anvi_genomes.txt', quote=F, row.names=F, sep='\t')
write.table(pedio_gen, 'pedio_anvi_genomes.txt', quote=F, row.names=F, sep='\t')
write.table(leucono_gen, 'leucono_anvi_genomes.txt', quote=F, row.names=F, sep='\t')

#exit R
quit()
n
```

## Make genome storage for each genera
```
source activate anvio5
#if databases are outdated, use anvi-migrate-db ./[group_folder]/*.fna.db

anvi-gen-genomes-storage -e leucono_anvi_genomes.txt -o leucono-GENOMES.db
#The new genomes storage ......................: leucono-GENOMES.db (v6, signature: hashf9b80219)
#Number of genomes ............................: 8 (internal: 0, external: 8)
#Number of gene calls .........................: 15,676
#Number of partial gene calls .................: 223

anvi-gen-genomes-storage -e strepto_anvi_genomes.txt -o strepto-GENOMES.db
#The new genomes storage ......................: strepto-GENOMES.db (v6, signature: hash629fcf5b)
#Number of genomes ............................: 2,765 (internal: 0, external: 2,765)
#Number of gene calls .........................: 5,642,043
#Number of partial gene calls .................: 138,123

anvi-gen-genomes-storage -e entero_anvi_genomes.txt -o entero-GENOMES.db
#The new genomes storage ......................: entero-GENOMES.db (v6, signature: hashcea9c0fc)
#Number of genomes ............................: 820 (internal: 0, external: 820)
#Number of gene calls .........................: 2,443,525
#Number of partial gene calls .................: 62,671



anvi-gen-genomes-storage -e lactob_anvi_genomes.txt -o lactob-GENOMES.db
#The new genomes storage ......................: lactob-GENOMES.db (v6, signature: hash91d51c68)
#Number of genomes ............................: 759 (internal: 0, external: 759)
#Number of gene calls .........................: 1,870,319
#Number of partial gene calls .................: 71,373


anvi-gen-genomes-storage -e pedio_anvi_genomes.txt -o pedio-GENOMES.db
#The new genomes storage ......................: pedio-GENOMES.db (v6, signature: hash872f0d0f)
#Number of genomes ............................: 40 (internal: 0, external: 40)
#Number of gene calls .........................: 81,888
#Number of partial gene calls .................: 1,301

anvi-gen-genomes-storage -e lactoc_anvi_genomes.txt -o lactoc-GENOMES.db
#The new genomes storage ......................: lactoc-GENOMES.db (v6, signature: hash121c8e5b)
#Number of genomes ............................: 106 (internal: 0, external: 106)
#Number of gene calls .........................: 267,059
#Number of partial gene calls .................: 9,301

anvi-gen-genomes-storage -e weisel_anvi_genomes.txt -o weisel-GENOMES.db
#The new genomes storage ......................: weisel-GENOMES.db (v6, signature: hash1c29efbe)
#Number of genomes ............................: 23 (internal: 0, external: 23)
#Number of gene calls .........................: 41,579
#Number of partial gene calls .................: 1,294
```


