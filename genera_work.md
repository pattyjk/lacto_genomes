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

## Pangenomes
```
#pangenome analysis
anvi-pan-genome -g leucono-GENOMES.db -o leucono -n leuono --enforce-hierarchical-clustering -T 4 --exclude-partial-gene-calls

anvi-pan-genome -g weisel-GENOMES.db -o weisel -n weisel --enforce-hierarchical-clustering -T 4 --exclude-partial-gene-calls

anvi-pan-genome -g pedio-GENOMES.db -o pedio -n pedio --enforce-hierarchical-clustering -T 4 --exclude-partial-gene-calls

anvi-pan-genome -g lactoc-GENOMES.db -o lactoc -n lactoc --enforce-hierarchical-clustering -T 4 --exclude-partial-gene-calls

anvi-pan-genome -g lactob-GENOMES.db -o lactob -n lactob --enforce-hierarchical-clustering -T 4 --exclude-partial-gene-calls

anvi-pan-genome -g entero-GENOMES.db -o entero -n entero --enforce-hierarchical-clustering -T 4 --exclude-partial-gene-calls

anvi-pan-genome -g strepto-GENOMES.db -o strepto -n strepto --enforce-hierarchical-clustering -T 4 --exclude-partial-gene-calls
```


## View analysis
```
anvi-display-pan -g leucono-GENOMES.db -p pan_genomes/leucono/leuono-PAN.db
anvi-display-pan -g weisel-GENOMES.db -p pan_genomes/weisel/weisel-PAN.db
anvi-display-pan -g pedio-GENOMES.db -p pan_genomes/pedio/pedio-PAN.db
anvi-display-pan -g lactoc-GENOMES.db -p pan_genomes/lactoc/lactoc-PAN.db
anvi-display-pan -g lactob-GENOMES.db -p pan_genomes/lactob/lactob-PAN.db
anvi-display-pan -g entero-GENOMES.db -p pan_genomes/entero/entero-PAN.db
anvi-display-pan -g strepto-GENOMES.db -p pan_genomes/strepto/strepto-PAN.db
```

## Get ribosomal protein sequence
```
anvi-get-sequences-for-hmm-hits --external-genomes pedio_anvi_genomes.txt -o pedio_cat_ribo_proteins.fa --return-best-hit --hmm-source Rinke_et_al --get-aa-sequence --concatenate --gene-names Ribosomal_L1,Ribosomal_L10,Ribosomal_L11,Ribosomal_L11_N,Ribosomal_L13,Ribosomal_L14,Ribosomal_L15e,Ribosomal_L16,Ribosomal_L18p,Ribosomal_L19e,Ribosomal_L2,Ribosomal_L21e,Ribosomal_L22,Ribosomal_L23,Ribosomal_L24e,Ribosomal_L29,Ribosomal_L2_C,Ribosomal_L3,Ribosomal_L30,Ribosomal_L31e,Ribosomal_L32e,Ribosomal_L37ae,Ribosomal_L37e,Ribosomal_L39,Ribosomal_L4,Ribosomal_L44,Ribosomal_L5,Ribosomal_L5_C,Ribosomal_L6,Ribosomal_S11,Ribosomal_S13,Ribosomal_S13_N,Ribosomal_S14,Ribosomal_S15,Ribosomal_S17,Ribosomal_S17e,Ribosomal_S19,Ribosomal_S19e,Ribosomal_S2,Ribosomal_S24e,Ribosomal_S27,Ribosomal_S27e,Ribosomal_S28e,Ribosomal_S3Ae,Ribosomal_S3_C,Ribosomal_S4e,Ribosomal_S5,Ribosomal_S5_C,Ribosomal_S6e,Ribosomal_S7,Ribosomal_S8,Ribosomal_S8e,Ribosomal_S9

anvi-get-sequences-for-hmm-hits --external-genomes weisel_anvi_genomes.txt -o weisel_cat_ribo_proteins.fa --return-best-hit --hmm-source Rinke_et_al --get-aa-sequence --concatenate --gene-names Ribosomal_L1,Ribosomal_L10,Ribosomal_L11,Ribosomal_L11_N,Ribosomal_L13,Ribosomal_L14,Ribosomal_L15e,Ribosomal_L16,Ribosomal_L18p,Ribosomal_L19e,Ribosomal_L2,Ribosomal_L21e,Ribosomal_L22,Ribosomal_L23,Ribosomal_L24e,Ribosomal_L29,Ribosomal_L2_C,Ribosomal_L3,Ribosomal_L30,Ribosomal_L31e,Ribosomal_L32e,Ribosomal_L37ae,Ribosomal_L37e,Ribosomal_L39,Ribosomal_L4,Ribosomal_L44,Ribosomal_L5,Ribosomal_L5_C,Ribosomal_L6,Ribosomal_S11,Ribosomal_S13,Ribosomal_S13_N,Ribosomal_S14,Ribosomal_S15,Ribosomal_S17,Ribosomal_S17e,Ribosomal_S19,Ribosomal_S19e,Ribosomal_S2,Ribosomal_S24e,Ribosomal_S27,Ribosomal_S27e,Ribosomal_S28e,Ribosomal_S3Ae,Ribosomal_S3_C,Ribosomal_S4e,Ribosomal_S5,Ribosomal_S5_C,Ribosomal_S6e,Ribosomal_S7,Ribosomal_S8,Ribosomal_S8e,Ribosomal_S9

anvi-get-sequences-for-hmm-hits --external-genomes leucono_anvi_genomes.txt -o leucono_cat_ribo_proteins.fa --return-best-hit --hmm-source Rinke_et_al --get-aa-sequence --concatenate --gene-names Ribosomal_L1,Ribosomal_L10,Ribosomal_L11,Ribosomal_L11_N,Ribosomal_L13,Ribosomal_L14,Ribosomal_L15e,Ribosomal_L16,Ribosomal_L18p,Ribosomal_L19e,Ribosomal_L2,Ribosomal_L21e,Ribosomal_L22,Ribosomal_L23,Ribosomal_L24e,Ribosomal_L29,Ribosomal_L2_C,Ribosomal_L3,Ribosomal_L30,Ribosomal_L31e,Ribosomal_L32e,Ribosomal_L37ae,Ribosomal_L37e,Ribosomal_L39,Ribosomal_L4,Ribosomal_L44,Ribosomal_L5,Ribosomal_L5_C,Ribosomal_L6,Ribosomal_S11,Ribosomal_S13,Ribosomal_S13_N,Ribosomal_S14,Ribosomal_S15,Ribosomal_S17,Ribosomal_S17e,Ribosomal_S19,Ribosomal_S19e,Ribosomal_S2,Ribosomal_S24e,Ribosomal_S27,Ribosomal_S27e,Ribosomal_S28e,Ribosomal_S3Ae,Ribosomal_S3_C,Ribosomal_S4e,Ribosomal_S5,Ribosomal_S5_C,Ribosomal_S6e,Ribosomal_S7,Ribosomal_S8,Ribosomal_S8e,Ribosomal_S9

anvi-get-sequences-for-hmm-hits --external-genomes lactoc_anvi_genomes.txt -o lactoc_cat_ribo_proteins.fa --return-best-hit --hmm-source Rinke_et_al --get-aa-sequence --concatenate --gene-names Ribosomal_L1,Ribosomal_L10,Ribosomal_L11,Ribosomal_L11_N,Ribosomal_L13,Ribosomal_L14,Ribosomal_L15e,Ribosomal_L16,Ribosomal_L18p,Ribosomal_L19e,Ribosomal_L2,Ribosomal_L21e,Ribosomal_L22,Ribosomal_L23,Ribosomal_L24e,Ribosomal_L29,Ribosomal_L2_C,Ribosomal_L3,Ribosomal_L30,Ribosomal_L31e,Ribosomal_L32e,Ribosomal_L37ae,Ribosomal_L37e,Ribosomal_L39,Ribosomal_L4,Ribosomal_L44,Ribosomal_L5,Ribosomal_L5_C,Ribosomal_L6,Ribosomal_S11,Ribosomal_S13,Ribosomal_S13_N,Ribosomal_S14,Ribosomal_S15,Ribosomal_S17,Ribosomal_S17e,Ribosomal_S19,Ribosomal_S19e,Ribosomal_S2,Ribosomal_S24e,Ribosomal_S27,Ribosomal_S27e,Ribosomal_S28e,Ribosomal_S3Ae,Ribosomal_S3_C,Ribosomal_S4e,Ribosomal_S5,Ribosomal_S5_C,Ribosomal_S6e,Ribosomal_S7,Ribosomal_S8,Ribosomal_S8e,Ribosomal_S9

###
anvi-get-sequences-for-hmm-hits --external-genomes lactob_anvi_genomes.txt -o lactob_cat_ribo_proteins.fa --return-best-hit --hmm-source Rinke_et_al --get-aa-sequence --concatenate --gene-names Ribosomal_L1,Ribosomal_L10,Ribosomal_L11,Ribosomal_L11_N,Ribosomal_L13,Ribosomal_L14,Ribosomal_L15e,Ribosomal_L16,Ribosomal_L18p,Ribosomal_L19e,Ribosomal_L2,Ribosomal_L21e,Ribosomal_L22,Ribosomal_L23,Ribosomal_L24e,Ribosomal_L29,Ribosomal_L2_C,Ribosomal_L3,Ribosomal_L30,Ribosomal_L31e,Ribosomal_L32e,Ribosomal_L37ae,Ribosomal_L37e,Ribosomal_L39,Ribosomal_L4,Ribosomal_L44,Ribosomal_L5,Ribosomal_L5_C,Ribosomal_L6,Ribosomal_S11,Ribosomal_S13,Ribosomal_S13_N,Ribosomal_S14,Ribosomal_S15,Ribosomal_S17,Ribosomal_S17e,Ribosomal_S19,Ribosomal_S19e,Ribosomal_S2,Ribosomal_S24e,Ribosomal_S27,Ribosomal_S27e,Ribosomal_S28e,Ribosomal_S3Ae,Ribosomal_S3_C,Ribosomal_S4e,Ribosomal_S5,Ribosomal_S5_C,Ribosomal_S6e,Ribosomal_S7,Ribosomal_S8,Ribosomal_S8e,Ribosomal_S9
###

anvi-get-sequences-for-hmm-hits --external-genomes entero_anvi_genomes.txt -o entero_cat_ribo_proteins.fa --return-best-hit --hmm-source Rinke_et_al --get-aa-sequence --concatenate --gene-names Ribosomal_L1,Ribosomal_L10,Ribosomal_L11,Ribosomal_L11_N,Ribosomal_L13,Ribosomal_L14,Ribosomal_L15e,Ribosomal_L16,Ribosomal_L18p,Ribosomal_L19e,Ribosomal_L2,Ribosomal_L21e,Ribosomal_L22,Ribosomal_L23,Ribosomal_L24e,Ribosomal_L29,Ribosomal_L2_C,Ribosomal_L3,Ribosomal_L30,Ribosomal_L31e,Ribosomal_L32e,Ribosomal_L37ae,Ribosomal_L37e,Ribosomal_L39,Ribosomal_L4,Ribosomal_L44,Ribosomal_L5,Ribosomal_L5_C,Ribosomal_L6,Ribosomal_S11,Ribosomal_S13,Ribosomal_S13_N,Ribosomal_S14,Ribosomal_S15,Ribosomal_S17,Ribosomal_S17e,Ribosomal_S19,Ribosomal_S19e,Ribosomal_S2,Ribosomal_S24e,Ribosomal_S27,Ribosomal_S27e,Ribosomal_S28e,Ribosomal_S3Ae,Ribosomal_S3_C,Ribosomal_S4e,Ribosomal_S5,Ribosomal_S5_C,Ribosomal_S6e,Ribosomal_S7,Ribosomal_S8,Ribosomal_S8e,Ribosomal_S9

anvi-get-sequences-for-hmm-hits --external-genomes strepto_anvi_genomes.txt -o strepto_cat_ribo_proteins.fa --return-best-hit --hmm-source Rinke_et_al --get-aa-sequence --concatenate --gene-names Ribosomal_L1,Ribosomal_L10,Ribosomal_L11,Ribosomal_L11_N,Ribosomal_L13,Ribosomal_L14,Ribosomal_L15e,Ribosomal_L16,Ribosomal_L18p,Ribosomal_L19e,Ribosomal_L2,Ribosomal_L21e,Ribosomal_L22,Ribosomal_L23,Ribosomal_L24e,Ribosomal_L29,Ribosomal_L2_C,Ribosomal_L3,Ribosomal_L30,Ribosomal_L31e,Ribosomal_L32e,Ribosomal_L37ae,Ribosomal_L37e,Ribosomal_L39,Ribosomal_L4,Ribosomal_L44,Ribosomal_L5,Ribosomal_L5_C,Ribosomal_L6,Ribosomal_S11,Ribosomal_S13,Ribosomal_S13_N,Ribosomal_S14,Ribosomal_S15,Ribosomal_S17,Ribosomal_S17e,Ribosomal_S19,Ribosomal_S19e,Ribosomal_S2,Ribosomal_S24e,Ribosomal_S27,Ribosomal_S27e,Ribosomal_S28e,Ribosomal_S3Ae,Ribosomal_S3_C,Ribosomal_S4e,Ribosomal_S5,Ribosomal_S5_C,Ribosomal_S6e,Ribosomal_S7,Ribosomal_S8,Ribosomal_S8e,Ribosomal_S9
```

## Make tree based on ribosomal proteins
```
anvi-gen-phylogenomic-tree -f pedio_cat_ribo_proteins.fa -o pedio_tree.tree
anvi-gen-phylogenomic-tree -f leucono_cat_ribo_proteins.fa -o leucono_tree.tree
anvi-gen-phylogenomic-tree -f weisel_cat_ribo_proteins.fa -o weisel_tree.tree
anvi-gen-phylogenomic-tree -f lactoc_cat_ribo_proteins.fa -o lactoc_tree.tree
anvi-gen-phylogenomic-tree -f lactob_cat_ribo_proteins.fa -o lactob_tree.tree
anvi-gen-phylogenomic-tree -f strepto_cat_ribo_proteins.fa -o strepto_tree.tree
anvi-gen-phylogenomic-tree -f entero_cat_ribo_proteins.fa -o entero_tree.tree
```

## Calculate ANI
```
anvi-compute-ani -o weisel_ANI -e weisel_anvi_genomes.txt -T 4 -p /home/pattyjk/Desktop/weisel/weisel-PAN.db
anvi-compute-ani -o pedio_ANI -e pedio_anvi_genomes.txt -T 4 -p /home/pattyjk/Desktop/pedio/pedio-PAN.db
anvi-compute-ani -o leucono_ANI -e leucono_anvi_genomes.txt -T 4 -p /home/pattyjk/Desktop/leucono/leucono-PAN.db
anvi-compute-ani -o lactoc_ANI -e lactoc_anvi_genomes.txt -T 4 -p /home/pattyjk/Desktop/lactoc/lactoc-PAN.db
anvi-compute-ani -o lactob_ANI -e lactob_anvi_genomes.txt -T 4 -p /home/pattyjk/Desktop/lactob/lactob-PAN.db
anvi-compute-ani -o entero_ANI -e entero_anvi_genomes.txt -T 4 -p /home/pattyjk/Desktop/entero/entero-PAN.db
anvi-compute-ani -o strepto_ANI -e strepto_anvi_genomes.txt -T 4 -p /home/pattyjk/Desktop/strepto/strepto-PAN.db
```
