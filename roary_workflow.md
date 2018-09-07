# Pangenomes with Prokka and Roary
## Annotate genomes with Prokka
```
#conda install -c conda-forge -c bioconda prokka

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
cd ..

mkdir prokka_out

#run prokka
for i in *.fna
do
prokka $i --kingdom Bacteria --cpus 4 --outdir prokka_$i --prefix prokka_$i
done
```

## Calculate pangenome with Roary
```
#find and move all .gff files
mkdir gff_files
cd test
find -type f -name "*.gff" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/gff_files \;
cd ..
cd gff_files



roary -f ./demo -e -n -v ./gff/*.gff
```
