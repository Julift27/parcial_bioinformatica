# parcial_bioinformatica
Parcial 2

## Antes de empezar
 cd /mnt/c/Users/julif/OneDrive/Escritorio/UROSARIO/"Semestre VII"/Bioinformática
 mkdir parcial
 
### En el cluster:
 mkdir parcial1_JulianaFonseca
 
## Punto 1
mv sequence.fasta parcial
wget "https://raw.githubusercontent.com/paula-torres/bioinformatica_ur/main/files/query_parcial.fasta"

### En atom
Find: (>\w+.\d)\s(\w+)\s(\w+)\s\w+\s\w+-\d\s\w+\s\w+\s\w+\s\w+\s\((\w+)\)\s\w+\,\s\w+\s\w+\;\s\w+
Replace all: $1_$4_$2_$3

### Entrar al cluster:

cd /mnt/c/Users/julif/OneDrive/Escritorio/UROSARIO/Semestre VII/Bioinformática/llave/
ssh -i bio.pt.pem -p 37022 bio.pt@172.25.255.10

### En el cluster

pwd para obtener: /home/bio.pt/data/Parcial1/parcial1_JulianaFonseca

exit

### En la terminal

cd /mnt/c/Users/julif/OneDrive/Escritorio/UROSARIO/Semestre VII/Bioinformática/llave/

scp -i bio.pt.pem -P 37022 /mnt/c/Users/julif/OneDrive/Escritorio/UROSARIO/"Semestre VII"/Bioinformática/parcial/sequence.fasta bio.pt@172.25.255.10:/home/bio.pt/data/Parcial1/parcial1_JulianaFonseca

scp -i bio.pt.pem -P 37022 /mnt/c/Users/julif/OneDrive/Escritorio/UROSARIO/"Semestre VII"/Bioinformática/parcial/query_parcial.fasta bio.pt@172.25.255.10:/home/bio.pt/data/Parcial1/parcial1_JulianaFonseca

ssh -i bio.pt.pem -p 37022 bio.pt@172.25.255.10

### En el cluster

cd data/Parcial1/parcial1_JulianaFonseca

### Blast

salloc
module load blast/2.7.1

makeblastdb -in sequence.fasta -dbtype nucl -parse_seqids -out trans_Noctuidae -title "Noctuidae_transcriptome"
blastn -query query_parcial.fasta -task megablast -db trans_Noctuidae -outfmt 7 -word_size 7 -out blast_query -nu
m_threads 1

El gen corresponde al COI. Según los datos obtenidos del blast la secuencia evaluada corresponde a la especie Spodoptera_cilium, teniendo en cuenta que tiene un % de identidad del 96.939 y logró realizar un alineamiento de 686 pb, con solo 21 mismatches. Estos párametros muestran un alto nivel de coincidencia, además de ser los valores más altos (% de identidad y alineamiento) y más bajos (mismatches) de todos los resultados obtenidos.


## Punto 2

cat sequence.fasta query_parcial.fasta > concat_sequence.fasta

### Muscle

module load muscle/3.8.31
muscle -in concat_sequence.fasta -out sequences_Noctuidae_muscle.fasta

### Árbol de máxima verosimilitud

mkdir FasConcat
cp sequences_Noctuidae_muscle.fasta FasConcat/
cp /home/bio.pt/data/FASconCAT-G_v1.05.1.pl FasConcat/

./FASconCAT-G_v1.05.1.pl

Establecer Nexus por bloques (escribir n una vez) y Phylip Relaxed (escribir p dos veces)
Dar s y enter

module load iqtree
iqtree -s FcC_supermatrix.phy -m GTR+I+G -bb 1000 -pre Noctuidae_muscle

exit (salir de salloc)
exit (salir del cluster)

### En la terminal 
scp -r -i bio.pt.pem -P 37022  bio.pt@172.25.255.10:/home/bio.pt/data/Parcial1/parcial1_JulianaFonseca/FasConcat/Noctuidae_muscle.treefile /mnt/c/Users/julif/OneDrive/Escritorio/UROSARIO/"Semestre VII"/Bioinformática/parcial

### Árbol en phylo.io

![Caos](https://github.com/Julift27/parcial_bioinformatica/blob/main/arbol_parcial.png)


## Punto 3

 wget "https://raw.githubusercontent.com/paula-torres/bioinformatica_ur/main/files/archivo1.csv"
 
less archivo1.csv
wc archivo1.csv
awk -F "\," '{print NF; exit}' archivo1.csv

El archivo tiene 49 filas, 908 caracteres y 4 columnas

sed 's/chr/Cromosoma/' archivo1.csv | sed 's/,/\t/g' > result_file.bed
awk '$4=="coding"' result_file.bed > result_file_coding.bed


































