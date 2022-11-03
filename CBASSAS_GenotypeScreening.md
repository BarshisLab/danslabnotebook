AmSam Genotype Screening
================
## 2022-11-02 renaming and trimgaloring
  * renaming samples

 ```bash
 [dbarshis@turing1 Data]$ pwd
/cm/shared/courses/dbarshis/barshislab/VRad/taxons/2022-10-31_BMKData/Data
[dbarshis@coreV2-22-036 Data]$ cat /cm/shared/courses/dbarshis/21AdvGenomics/scripts/renamer_advbioinf.py    #!/usr/bin/env python
####usage renamer.py renamingtable
#### this script take the entries in the first column of table and renames (mv's) them to files with the names in the second column
import sys
import os
fin=open(sys.argv[1],'r')
linecount=0
for line in fin:
	linecount+=1
	if linecount>=2:
		cols=line.rstrip().split('\t')
#		print 'mv %s %s' %(cols[0], cols[1])
		os.popen('mv %s %s' %(cols[0], cols[1]))
[dbarshis@coreV2-22-036 Data]$ /cm/shared/courses/dbarshis/21AdvGenomics/scripts/renamer_advbioinf.py RenamingTable.txt
```
  * mv'ing files to new directory

```bash
[dbarshis@coreV2-22-036 Data]$ mv *_CBASS_* /cm/shared/courses/dbarshis/barshislab/danb/taxons/2022-11_AmSam_BMKData/rawfastqs/
```
  * now trimgaloring Plob

```bash
[dbarshis@coreV2-22-036 rawfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/2022-11_AmSam_BMKData/rawfastqs
[dbarshis@coreV2-22-036 rawfastqs]$ cat TG_Plob.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-02_TrimGalore_Plob.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_Plob

enable_lmod

module load container_env trim_galore

for i in *Plob*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-036 rawfastqs]$ sbatch TG_Plob.sh 
Submitted batch job 9810349
```
  * Trimgaloring Ahya

```bash
[dbarshis@coreV2-22-036 rawfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/2022-11_AmSam_BMKData/rawfastqs
[dbarshis@coreV2-22-036 rawfastqs]$ nano TG_Ahya.sh
[dbarshis@coreV2-22-036 rawfastqs]$ cat TG_Ahya.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-02_TrimGalore_Plob.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_Plob

enable_lmod

module load container_env trim_galore

for i in *Ahya*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done
[dbarshis@coreV2-22-036 rawfastqs]$ sbatch TG_Ahya.sh 
Submitted batch job 9810350
```
  * Trimgaloring Pver

```bash
[dbarshis@coreV2-22-036 rawfastqs]$ nano TG_Pver.sh
[dbarshis@coreV2-22-036 rawfastqs]$ cat TG_Pver.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-02_TrimGalore_Pver.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_Pver

enable_lmod

module load container_env trim_galore

for i in *Pver*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-036 rawfastqs]$ sbatch TG_Pver.sh 
Submitted batch job 9810351
```
  * Trimgalore Mgri

```bash
[dbarshis@coreV2-22-036 rawfastqs]$ nano TG_Mgri.sh
[dbarshis@coreV2-22-036 rawfastqs]$ cat TG_Mgri.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-02_TrimGalore_Mgri.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_Mgri

enable_lmod

module load container_env trim_galore

for i in *Mgri*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-036 rawfastqs]$ sbatch TG_Mgri.sh 
Submitted batch job 9810352
```

## 2022-11-03 Trying multi-thread TrimGalore

```bash
[dbarshis@coreV2-22-036 2022-11_AmSamBMKData]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopra_verrucosa/2022-11_AmSamBMKData
[dbarshis@coreV2-22-036 2022-11_AmSamBMKData]$ cat TG_Pver_multithread.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-03_TrimGalore_Pver_multithread.txt
#SBATCH -n 4
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_Pver_multi

enable_lmod

module load container_env trim_galore

cp /cm/shared/courses/dbarshis/barshislab/danb/taxons/2022-11_AmSam_BMKData/rawfastqs/*Pver*R[12].fq.gz ./

for i in *Pver*_R1.fq.gz ; do `crun trim_galore --cores 4 --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-036 2022-11_AmSamBMKData]$ sbatch TG_Pver_multithread.sh 
Submitted batch job 9810704
```