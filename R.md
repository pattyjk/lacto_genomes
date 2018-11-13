#Lactobacillilales Pangenome
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

#number of genomes in analysis
length(unique(lacto_gen$genome))
#4520 genomes

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

test<-lacto_gen
#lacto_gen<-test

#remove extra characters from genes
lacto_gen$gene<-gsub("_[[:digit:]]+", "", lacto_gen$gene)

#remove rows with no genes
lacto_gen<- lacto_gen[-which(lacto_gen$gene == ""),]

#create column with '1'
lacto_gen$presence<-rep(1, nrow(lacto_gen))

#remove duplicated genes in genomes
lacto_gen<-lacto_gen[!duplicated(lacto_gen[c(1,5)]),]

#get genes that are shared accross all genes
library(plyr)
gene_sum<-ddply(lacto_gen,  c("gene"), summarize, n=length(presence))

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

plot(out2$no_genomes, out2$no_genes)

#95% of genes
0.95*4520
#4294

length(which(gene_sum$n > 4294))
#451


#read in taxonomic information
library(readr)
lacto_tax<-read.delim("~/Desktop/lacto_genomes.csv", header=T, sep='\t')
lacto_tax$taxon_oid<-as.character(lacto_tax$taxon_oid)

#merge taxonomy to gene info
lacto_gen2<-merge(lacto_gen_cog, lacto_tax,by.x='.id', by.y='taxon_oid', all.x=T)
length(unique(lacto_gen2))
