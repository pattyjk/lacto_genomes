## EMP 16S rRNA gene data

```
#sudo apt-get install fastx-toolkit
#fastx-toolkit v. 0.0.14

#quality filter the reads
for i in $(find . -maxdepth 3 -type f -name "*.fq"); do
  fastq_quality_filter -i $i -q 30 -o $i.fastq
done

#convert to fasta file 
for i in $(find . -maxdepth 3 -type f -name "*.fq"); do
  fastq_quality_filter -i $i -q 30 -o $i.fna
done
