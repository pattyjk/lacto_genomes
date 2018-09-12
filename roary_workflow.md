# Pangenomes with Prokka and Roary and ANI with FastANI
## Annotate genomes with Prokka
```
#conda install -c conda-forge -c bioconda prokka

#make sure the computer can find perl
export PERL5LIB=/home/pattyjk/anaconda3/lib/perl5/site_perl/5.22.0

#fix fasta file headers
cd fixed_fasta
#this renames the fasta headers by the file name
for file in *.fna;
do
sed -i "s/>.*/>${file%%.*}/" "$file" ;
done

#this adds a number to each fasta header, so that they are distinct for prokka
perl -i.bak -pe 's/$/_$seen{$_}/ if ++$seen{$_}>1 and /^>/; ' *.fna
rm *.bak

mkdir prokka_out

#run prokka
for i in *.fna
do
prokka $i --kingdom Bacteria --cpus 4 --outdir prokka_out/prokka_$i --prefix prokka_$i
done
```

## Calculate order-level pangenome with Roary
```
#conda install -c bioconda roary 

#make sure the computer can find perl
export PERL5LIB=/home/pattyjk/anaconda3/lib/perl5/site_perl/5.22.0

#roary is located at: /home/pattyjk/anaconda3/pkgs/roary-3.7.0-0/bin/roary

#find and move all .gff files
mkdir gff_files
cd test
find -type f -name "*.gff" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/gff_files \;
cd ..
cd gff_files

mkdir roary_out
/home/pattyjk/anaconda3/pkgs/roary-3.7.0-0/bin/roary *.gff -o roary_out

/home/pattyjk/anaconda3/pkgs/roary-3.7.0-0/bin/roary-create_pan_genome_plots.R
```

## Calculate genus level pangenome
```
#put each prokka annotation in genus folder
mkdir entero
mkdir strepto
mkdir lactob
mkdir pedio
mkdir lactoc
mkdir leucono
mkdir weis

cd /media/pattyjk/Elements/lactobacilliales_genomes/prokka_out

#move prokka folders
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/entero_genomes.txt cp -r -t /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/entero/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/strepto_genomes.txt cp -r -t /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/strepto/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/lactobac_genomes.txt cp -r -t /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/lactob/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/pedio_genomes.txt cp -r -t /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/pedio/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/lactococc_genomes.txt cp -r -t /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/lactoc/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/leucono_genomes.txt cp -r -t /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/leucono/
xargs -a /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/weisel_genomes.txt cp -r -t /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/weis/

cd  /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/

#make gff folders
mkdir entero_gff
mkdir strepto_gff
mkdir lactob_gff
mkdir pedio_gff
mkdir lactoc_gff
mkdir leucono_gff
mkdir weis_gff

#move gff files
cd strepto
find -type f -name "*.gff" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/strepto_gff \;
cd ..


cd entero
find -type f -name "*.gff" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/entero_gff \;
cd ..
cd pedio
find -type f -name "*.gff" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/pedio_gff \;
cd ..
cd lactob
find -type f -name "*.gff" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/lactob_gff \;
cd ..
cd lactoc
find -type f -name "*.gff" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/lactoc_gff \;
cd ..
cd leucono
find -type f -name "*.gff" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/leucono_gff \;
cd ..
cd weis
find -type f -name "*.gff" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/individual_prokka/weis_gff \;
cd ..

# calculate genus-level pangenomes with roary
export PERL5LIB=/home/pattyjk/anaconda3/lib/perl5/site_perl/5.22.0

cd weis_gff
mkdir roary_out
/home/pattyjk/anaconda3/pkgs/roary-3.7.0-0/bin/roary *.gff -o roary_out
cd ..

cd lactoc_gff
mkdir roary_out
/home/pattyjk/anaconda3/pkgs/roary-3.7.0-0/bin/roary *.gff -o roary_out
cd ..

cd entero_gff
mkdir roary_out
/home/pattyjk/anaconda3/pkgs/roary-3.7.0-0/bin/roary *.gff -o roary_out
cd ..

cd strepto_gff
mkdir roary_out
/home/pattyjk/anaconda3/pkgs/roary-3.7.0-0/bin/roary *.gff -o roary_out
cd ..

cd leucono_gff
mkdir roary_out
/home/pattyjk/anaconda3/pkgs/roary-3.7.0-0/bin/roary *.gff -o roary_out
cd ..

cd pedio_gff
mkdir roary_out
/home/pattyjk/anaconda3/pkgs/roary-3.7.0-0/bin/roary *.gff -o roary_out
cd ..

cd lactob_gff
mkdir roary_out
/home/pattyjk/anaconda3/pkgs/roary-3.7.0-0/bin/roary *.gff -o roary_out
cd ..
```

## Family level pangenomes
```
#make family-level genome files
cat lactobac_genomes.txt pedio_genomes.txt > lactobacillaceae_gen.txt
cat weisel_genomes.txt leucono_genomes.txt > leuconostocaceae_gen.txt
cat strepto_genomes.txt lactococc_genomes.txt > streptococcaceae_gen.txt
```

## Extract 16S copy no, genome size, CDS, contigs, tRNA from prokka annotation
```
mkdir genome_info
cd fixed_fasta/prokka_out

#copy the files of interest
find -type f -name "*.txt" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/genome_info \;

#change directories
cd ..
cd ..
cd genome_info

#remove colon
for i in *.txt
do
sed -i 's/\://g' $i
done

#process in arrr
R
library(plyr)
gen_names<-list.files(".", pattern="*.txt", full.names=T)
names(gen_names)<-basename(gen_names)
gen<-ldply(gen_names, read.delim)

#split column into meaningful values
library(tidyr)
gen_split<-separate(gen, organism.Genus.species.strain, sep=" ", into=c("type", "value"))
names(gen_split)<-c("genome_name", "type", "value")

#fix first column
gen_split$genome_name<-gsub("prokka_", "", gen_split$genome_name)
gen_split$genome_name<-gsub(".fna.txt", "", gen_split$genome_name)

#write data to file
write.csv(gen_split, "genome_info.csv", row.names=F, quote=F)

#split data by type
gen_split2<-split(gen_split, gen_split$type)
```

## Make prokka annotations into KEGG
```
#source("https://bioconductor.org/biocLite.R")
#biocLite("KEGGREST")
library(KEGGREST)

```


## Calculate ANI with FastANI
```
#sudo apt-get install libgsl-dev
#cd ~
#git clone https://github.com/ParBLiSS/FastANI.git
#cd FastANI
#./bootstrap.sh
#./configure

#make list of genomes to calculate ANI on
cd /

ls -R1 ./media/pattyjk/Elements/lactobacilliales_genomes/fixed_fasta |    while read l; do case $l in *:) d=${l%:};; "") d=;; *) echo "$d/$l";; esac; done > /media/pattyjk/Elements/lactobacilliales_genomes/lactobacillilales_genomes.txt

cd /media/pattyjk/Elements/lactobacilliales_genomes/
sed -i 's/\.//\//g' lactobacillilales_genomes.txt

/home/pattyjk/FastANI/fastANI -t 4 -o ANI_out --queryList lactobacillilales_genomes.txt --refList lactobacillilales_genomes.txt
```
