# Pangenomes with Prokka and Roary
## Annotate genomes with Prokka
```
#conda install -c conda-forge -c bioconda prokka
#export PERL5LIB=/home/pattyjk/anaconda3/lib/perl5/site_perl/5.22.0

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

## Calculate pangenome with Roary
```
#conda install -c bioconda roary 
#export PERL5LIB=/home/pattyjk/anaconda3/lib/perl5/site_perl/5.22.0
/home/pattyjk/anaconda3/pkgs/roary-3.7.0-0/bin/roary

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
```


