# Pangenomes with Prokka and Roary
## Annotate genomes with Prokka
```
#conda install -c conda-forge -c bioconda prokka

#fix fasta file headers
cd fixed_fasta
#this reanmes the faste headers by the file name
for file in *.fna;
do
sed -i "s/>.*/>${file%%.*}/" "$file" ;
done

#this adds a number to each fasta header, so that they are distinct for prokka
perl -i.bak -pe 's/$/_$seen{$_}/ if ++$seen{$_}>1 and /^>/; ' *.fna

cd ..

mkdir prokka_out

#run prokka
for i in *.fna
do
prokka $i --outdir $i --kingdom Bacteria --cpus 4 
done
```

## Calculate pangenome with Roary
```
#sudo apt-get install roary

```
