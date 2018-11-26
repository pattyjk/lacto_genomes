# Old Code
```
#number of genes that could be annotated (minus rRNA and tRNA)
length(unique(gene_sum$gascession))
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
```
