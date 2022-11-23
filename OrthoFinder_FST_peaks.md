We will be using Orthofinder to identify genes in a region where there is divergence along the genome. We have identified this region at 38 - 38.5 Mb.

Files we will be using:
A gff file, with annotations and features (house_sparrow_chr1A.gff)
A protein file for the house sparrow, with aminoacid sequences (house_sparrow_proteins.faa)
A protein file for a closely related model-organism (chicken; downloaded from ensembl; Gallus_gallus.GRCg6a.pep.all.faa )

```
# Note to self. Data is in saga /cluster/projects/nn9449k/cerca/024_physalia_FST_peaks/00_data
```

1 - We start by cleaning up the space and setting an working directory.

```
# We make a folder and move inside
mkdir Orthofinder_exercise; cd Orthofinder_exercise

# We make a structure for us to work
mkdir 01_data 02_orthofinder_run 03_processingResults
```

2 - Let's copy the data, and then modify it.
```
cd 01_data
cp /where/ever/the/data/is/*faa .

# Orthofinder will give us tons of results. When we run it with more than two species it often gets confusing. My solution? Adding the species name to ach gene :)
# Run this code, but think, what does it do?
sed -i "/^>/ s/>/>House_sparrow_/" house_sparrow_proteins.faa
sed -i "/^>/ s/>/>chicken_/" Gallus_gallus.GRCg6a.pep.all.faa 
```

3 - Let's run OrthoFinder
```
# Loading OrthoFinder

XXX

# Run it on the data folder
orthofinder -f . -t 4

# move the folder to the results
mv OrthoFinder ../02_orthofinder_run/
```

4 - Now/ While we wait, we will find what house sparrow genes are in CHR1A between 38 - 38.5 MB.
```
# CHR1A Rombos vs West : 38 - 38.5 Mb
# Try to understand the code and what it does.
awk '$1=="chr1A"' house_sparrow_chr1A.gff | awk '$5>38000000 && $4<38500000' | awk '$3=="gene"' | sed "s/.*ID=//; s/\-.*//; s/;.*//" | sort -u > genes_chr1A_38Mb_38.5Mb.tsv

# Result
IV00_00019202
IV00_00019209
```

5 - Now, we look over the Orthofinder results. Explore the files in the folder 'Comparative_Genomics_Statistics'
```
# go to
cd Orthogroups
grep "IV00_00019202" Orthogroups.tsv
grep "IV00_00019209" Orthogroups.tsv

# There is a correponding chicken-ortholog in each. Save the gene name (e.g. chicken_ENSGALP00000041112.2)
# Go back on the data folder and grep for that gene
grep "ENSGALP00000041112.2" Gallus_gallus.GRCg6a.pep.reduced.faa
# What is the gene name and symbol? What do we know about that gene?
```
