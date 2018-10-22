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
