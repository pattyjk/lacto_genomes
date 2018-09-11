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
