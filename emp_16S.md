## EMP 16S rRNA gene data

```
#sudo apt-get install fastx-toolkit
#fastx-toolkit v. 0.0.14

#quality filter the reads
for i in $(find . -maxdepth 3 -type f -name "*.fq"); do
  fastq_quality_filter -i $i -q 30 -o $i.fastq
done

#convert to fasta file 
#this inflates the size of files present so >1tb of storage is needed
for i in $(find . -maxdepth 3 -type f -name "*.fastq"); do
  fastq_to_fasta -i $i -o $i.fasta
done

#catenate fasta files into one file
for i in $(find . -maxdepth 3 -type f -name "*.fna"); do
cat $i > emp_16s.fna
done

#pick OTUs aginst Silva with uclust
source activate qiime1

