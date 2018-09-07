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
