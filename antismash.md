## Antismash 
```
for i in *.fna
do
antismash --input-type nucl -c 4 --outputfolder anti_out/out_$i $i 
   
done
```
