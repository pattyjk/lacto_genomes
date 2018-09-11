## Antismash 
```
for i in *.fna
do
antismash --input-type nucl -c 4 --outputfolder anti_out/out_$i $i 
   
done
```

## Rename gene calls by genome name
```
#this relabels the gene call file by the file name
cd anti_out

for directory in *; 
do   pushd "$directory";   index=1;  
for filename in geneclusters.txt; 
do     extension="${filename##*.}";     
if [ "$filename" != "$extension" ]; 
then       extension=".$extension";     
else       extension="";     fi;     target_filename="${directory}$(printf "%06d" "$index")${extension}";   
if [ -f "$target_filename" ]; then       echo "File ${target_filename} exists; aborting.";     
exit 3;     fi;
cp "$filename" "${target_filename}";     ((index++));  
done;   popd; done

#copy all gene call files to new directory
cd ..
mkdir  anti_out2
find -type f -name "out_*" -exec cp {} /media/pattyjk/Elements/lactobacilliales_genomes/anti_out2/ \;

#strip the files of the excess crap in the name
cd ..
cd anti_out2
#remove out_
for filename in *.txt; do 
    [ -f "$filename" ] || continue
    mv "$filename" "${filename//out_/}"
done

#remove .fna000001
for filename in *.txt; do 
    [ -f "$filename" ] || continue
    mv "$filename" "${filename//.fna000001/}"
done
```

## Catenate files
```
#make list of empty files 
find ./ -empty > no_bsg.txt

#ount number of genomes with no BSG
grep -c '.txt' no_bsg.txt 
#803

#remove blank files
find ./ -empty -delete

#move list of blank files to general lactob folder
mv no_bsg.txt /media/pattyjk/Elements/lactobacilliales_genomes/

#catenate the files in arrr
R

#install.package("plyr")
library(plyr)

#list files of interest
paths <- dir(pattern = "\\.csv$")

#add file names to list of files
names(bsg) <- basename(bsg)

#read in the data and catenate
dbsg <- ldply(bsg, read.table)

#change names
names(dbsg)<-c("genome_name", "entry1", "entry2", "bsg", "loc1", "loc2")

#fix genome names
dbsg$genome_name<-gsub("txt", "", dbsg$genome_name)
dbsg$genome_name<-gsub("[[:punct:]]", "", dbsg$genome_name)

#read in files with no BSGs
no_bsg<-read.delim("/media/pattyjk/Elements/lactobacilliales_genomes/no_bsg.txt", header=F)
no_bsg$genome_name<-gsub("[[:punct:]]", "", no_bsg$genome_name)
no_bsg$genome_name<-gsub("txt", "", no_bsg$genome_name)
no_bsg<-no_bsg[-804,]
no_bsg<-as.data.frame(no_bsg)
names(no_bsg)<-"genome_name"

#create blank columns in no bsg file
namey<-c("entry1", "entry2", "bsg", "loc1", "loc2")
no_bsg[,namey]<-NA

#merge files
bsg_cat<-rbind(dbsg, no_bsg)

#get summary for each genome
#install.package("dplyr")
library(dplyr)

bsg_sum<-ddply(bsg_cat, c("genome_name", "bsg"), summarize, n=length(bsg))

#write to files
write.table(bsg_cat, "lacto_bsg.txt",  sep="\t", row.names=F, quote=F)
write.table(bsg_sum, "lacto_bsg_sum.txt",  sep="\t", row.names=F, quote=F)
```
