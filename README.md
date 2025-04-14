# GEM preprocessing


## Task: Preprocessing a GEM for down-stream analysis in medical bioinformatics. 

# STEP 1: Install the GEMprep Environment 
``` 
conda create -n gemprep python=3.6 matplotlib mpi4py numpy pandas r scikit-learn seaborn 

source activate gemprep 

git clone https://github.com/SystemsGenetics/GEMprep 
cd GEMprep 
``` 

# STEP 2: Download GTEX and TCGA 
``` 
cd /scratch/leiarar/gem-processing/GEMprep #change to the current working directory 

wget https://ndownloader.figshare.com/files/9150631   #download normal kidney data from GTEX 
mv 9150631 kidney-rsem-fpkm-gtex.txt.gz #download normal kidney data from GTEX 

wget https://ndownloader.figshare.com/files/9150640   #download kidney tumor data from TCGA
mv 9150640 kirp-rsem-fpkm-tcga-t.txt.gz

gunzip kidney-rsem-fpkm-gtex.txt.gz  #unzip the downloaded files 
gunzip kirp-rsem-fpkm-tcga-t.txt.gz 
``` 

# STEP 2a: Process the GEM files Using your Path 
``` 
python /scratch/leiarar/gem-processing/GEMprep/bin/merge.py kidney-rsem-fpkm-gtex.txt kirp-rsem-fpkm-tcga-t.txt kidney-gtex-kirp.txt  #Merge the GTEX and TCGA GEMs

python /scratch/leiarar/gem-processing/GEMprep/bin/normalize.py kidney-gtex-kirp.txt kidney-gtex-kirp.quantile.txt --quantile   #quantile normalization of the merged GEM 

python /scratch/leiarar/gem-processing/GEMprep/bin/normalize.py kidney-gtex-kirp.quantile.txt kidney-gtex-kirp.quantile.log2.txt --log2   #transformation of the quantile-normalized GEM

``` 

# STEP 3: Create labels  and extract the first two lines to ensure properly created output
```
head -n1 kidney-gtex-kirp.quantile.log2.txt | sed 's/\t/\n/g' | sed 's/-/,/g' | awk -F, '{print $1}' | awk 'NR>1' > labels.txt

head -n2 kidney-gtex-kirp.quantile.log2.txt > first_two_lines_gem.txt #check the file 
``` 

# STEP 4: Check all processed files to ensure everything is created properly for GEM processing
```
These normalized matrices can now be used for the following: 
1. Input files for discovery biomarkers. 
2. Construction of gene co-expression networks (GCNs).  
3. Differential gene expression analysis between normal and tumor samples or DEGs. 

```








## License - this project is licensed under the MIT License.

### Contact information: 
Leiara Rivera 
* leiarar@clemson.edu 
* lrivera@email.sc.edu 
