##  Lactobacillilales Pangenome

### Read in data into a single data frame
```
setwd("/home/pattyjk/Dropbox/R/lacto_genomes/no_hypo/")
setwd("/home/pattyjk/Dropbox/R/lacto_genomes/gene_calls/")

setwd("/home/pattyjk/Dropbox/R/lacto_genomes/no_hypo/")
lacto<-dir(pattern = "\\.tsv\$")
setwd("/home/pattyjk/Dropbox/R/lacto_genomes/gene_calls/")
lacto<-dir(pattern = "*.db")

#add file names to list of files
names(lacto) <- basename(lacto)

#read in the data and catenate into a single data frame (file name is .id)
library(plyr)
lacto_gen <- ldply(lacto, read.delim)
nrow(lacto_gen)
#16,462,762

setwd("/home/pattyjk/Dropbox/R/lacto_genomes/")

#rename headers
#names(lacto_gen)<-c("genome", "locus_tag", "ftype", "bp", "gene", "EC", "COG", "product")
```

### Fix data frame
```
#number of genomes in analysis
length(unique(lacto_gen$.id))
#4521 genomes

#remove '.tsv' from genomes
#lacto_gen$genome<-gsub(".tsv", "", lacto_gen$genome)
#lacto_gen$genome<-gsub("[[:space:]]", "", lacto_gen$genome)
#lacto_gen$genome<-as.character(lacto_gen$genome)

lacto_gen$.id<-gsub(".fna.db", "", lacto_gen$.id)
lacto_gen$.id<-gsub("fun_", "", lacto_gen$.id)
lacto_gen$.id<-gsub("[[:space:]]", "", lacto_gen$.id)
lacto_gen$.id<-as.character(lacto_gen$.id)

#split by function
lacto_gen_split<-split(lacto_gen, lacto_gen$source)
lacto_gen_cog<-lacto_gen_split$COG_FUNCTION
nrow(lacto_gen_cog)
#8,231,381

test<-lacto_gen_cog
#lacto_gen<-test

#remove extra characters from genes
#lacto_gen_cog$gene <-gsub("_[[:digit:]]+", "", lacto_gen$gene)

#remove rows with no genes
#lacto_gen<- lacto_gen[-which(lacto_gen$gene == ""),]

#create column with '1'
lacto_gen_cog$presence<-rep(1, nrow(lacto_gen_cog))

#remove duplicated genes in genomes
#lacto_gen<-lacto_gen[!duplicated(lacto_gen[c(1,5)]),]

#get genes that are shared accross all genes
library(plyr)
gene_sum<-ddply(lacto_gen_cog,  c("accession", ".id"), summarize, n=length(presence))
```

### Map Taxonomy to genes
```
#read in taxonomic information
library(readr)
lacto_tax<-read.delim("lacto_taxo.txt", header=T, sep='\t')
lacto_tax$taxon_oid<-as.character(lacto_tax$taxon_oid)

#merge taxonomy to gene info to full dataset
#lacto_gen2<-merge(lacto_gen_cog, lacto_tax, by.x='.id', by.y='taxon_oid', all.x=T)
#dim(lacto_gen2)
#8231381  by   113

#merged to summarized dataset
lacto_gen_sum<-merge(gene_sum, lacto_tax, by.x='.id', by.y='taxon_oid', all.x=T)
dim(lacto_gen_sum)
#5430827     109

#split by genus
lacto_split<-split(lacto_gen_sum, lacto_gen_sum$Genus)
library(ggplot2)

ggplot(lacto_split$Pediococcus, aes(.id, accession))+
geom_tile(aes(fill=n))

ggplot(lacto_split$Streptococcus, aes(.id, accession))+
geom_tile(aes(fill=n))
```
