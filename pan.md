## Pan
```
#move prokka annotation tables
mkdir annotation_files
find -type f -name "*.tsv" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/annotation_files \;

#remove hypothetical proteins
mkdir no_hypo

for i in *.tsv
do
sed -n '/hypothetical protein/!p' $i > no_hypo/$i
done

#strip etra stuff from file names
for filename in *tsv; do 
    [ -f "$filename" ] || continue
    mv "$filename" "${filename//.fna/}"
done

for filename in *tsv; do 
    [ -f "$filename" ] || continue
    mv "$filename" "${filename//prokka_/}"
done


#read into R
R

#make list of files
library(plyr)
lacto<-dir(pattern = "\\.tsv$")

#add file names to list of files
names(lacto) <- basename(lacto)

#read in the data and catenate into a single data frame (file name is .id)
lacto_gen <- ldply(lacto, read.delim)

#rename headers
names(lacto_gen)<-c("genome", "locus_tag", "ftype", "bp", "gene", "EC", "COG", "product")

#number of genomes in analysis
length(unique(lacto_gen$genome))
#4520

#remove '.tsv' from genomes
lacto_gen$genome<-gsub(".tsv", "", lacto_gen$genome)

#read in taxonomic information
lacto_tax<-read.delim("~/Desktop/lacto_taxo.txt", header=T)

#merge taxonomy to gene info
lacto_gen2<-merge(lacto_gen, lacto_tax,by="genome")

#remove extra characters from genes
lacto_gen2$gene<-gsub("_[[:digit:]]+", "", lacto_gen2$gene)

#remove rows with no genes
lacto_gen2<- lacto_gen2[-which(lacto_gen2$gene == ""),]

#create column with '1'
lacto_gen2$presence<-rep(1, nrow(lacto_gen2))

#get genes that are shared accross all genes
library(plyr)
gene_sum<-ddply(lacto_gen2,  c("gene"), summarize, n=length(presence))

#number of genes that could be annotated (minus rRNA and tRNA)
length(unique(gene_sum$gene))
#5951 genes

out2<-NA
for (i in 1:4520){
out2[i]<-t(as.data.frame(length(which(gene_sum$n == i))))
}

out2<-as.data.frame(out2)
out2$no_genomes<-row.names(out2)
names(out2)<-c("no_genes", "no_genomes")
out2$no_genomes<-as.numeric(out2$no_genomes)
```

