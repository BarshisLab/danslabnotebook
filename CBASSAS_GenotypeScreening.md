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
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData
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

[dbarshis@coreV2-22-036 2022-11_AmSamBMKData]$ scontrol show job 9810704
JobId=9810704 JobName=TG_Pver_multi
   UserId=dbarshis(414328) GroupId=users(14514) MCS_label=N/A
   Priority=14236 Nice=0 Account=odu QOS=turing_default_qos
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=0 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:21:28 TimeLimit=UNLIMITED TimeMin=N/A
   SubmitTime=2022-11-03T19:27:02 EligibleTime=2022-11-03T19:27:02
   AccrueTime=2022-11-03T19:27:02
   StartTime=2022-11-03T19:27:02 EndTime=Unknown Deadline=N/A
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2022-11-03T19:27:02 Scheduler=Main
   Partition=main AllocNode:Sid=coreV2-22-036:104477
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=coreV2-22-036
   BatchHost=coreV2-22-036
   NumNodes=1 NumCPUs=4 NumTasks=4 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
   TRES=cpu=4,node=1,billing=4
   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
   MinCPUsNode=1 MinMemoryNode=0 MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
   Command=/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData/TG_Pver_multithread.sh
   WorkDir=/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData
   StdErr=/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData/2022-11-03_TrimGalore_Pver_multithread.txt
   StdIn=/dev/null
   StdOut=/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData/2022-11-03_TrimGalore_Pver_multithread.txt
   Power=
   MailUser=dbarshis@odu.edu MailType=END
```

##2022-11-04_Running MultiQC on Ahya

```bash
[dbarshis@coreV2-25-041 2022-11_AmSam_BMKData]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData
[dbarshis@coreV2-25-041 2022-11_AmSam_BMKData]$ enable_lmod

The following have been reloaded with a version change:
  1) slurm/20.11.5 => slurm/21.08

[dbarshis@coreV2-25-041 2022-11_AmSam_BMKData]$ module load container_env multiqc

The following have been reloaded with a version change:
  1) sge/2011.11p1 => sge/2011

[dbarshis@coreV2-25-041 2022-11_AmSam_BMKData]$ crun ./multiqc
/etc/container_runtime/runtime: line 17: /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/multiqc: No such file or directory
[dbarshis@coreV2-25-041 2022-11_AmSam_BMKData]$ crun multiqc ./
[WARNING]         multiqc : MultiQC Version v1.13 now available!
[INFO   ]         multiqc : This is MultiQC v1.9
[INFO   ]         multiqc : Template    : default
[INFO   ]         multiqc : Searching   : /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData
Searching 129 files..  [####################################]  100%          
[INFO   ]        cutadapt : Found 32 reports
[INFO   ]          fastqc : Found 32 reports
[INFO   ]         multiqc : Compressing plot data
[INFO   ]         multiqc : Report      : multiqc_report.html
[INFO   ]         multiqc : Data        : multiqc_data
[INFO   ]         multiqc : MultiQC complete
```

  * prepping transcript file for Ahyacinthus from [López-Nandam et al. 2021](https://www.biorxiv.org/content/10.1101/2021.07.20.453148v2), [download link](https://drive.google.com/drive/u/0/folders/15dh6eE8q850frTRVtglndXtzxSwj673g) found in Elora's [github](https://github.com/eloralopez/CoralGermline)

```bash
#### filtering for over 500bp
(base) danbarshis@BIOLLBB0 2021_Lopez-Nandam_et_al % pwd
/Users/danbarshis/dansstuff/Projeks/ODU/Projeks/reference_omes/Acropora_hyacinthus/2021_Lopez-Nandam_et_al
(base) danbarshis@BIOLLBB0 2021_Lopez-Nandam_et_al % python2 ~/scripts/fasta_manips/fasta_len_filter.py 500 _over500 Ahyacinthus.transcripts.fasta
Number of total seqs for Ahyacinthus.transcripts.fasta: 27110
Number of seqs over 500 for Ahyacinthus.transcripts.fasta: 20854
#### also trying over 250bp
(base) danbarshis@BIOLLBB0 2021_Lopez-Nandam_et_al % python2 ~/scripts/fasta_manips/fasta_len_filter.py 250 _over250 Ahyacinthus.transcripts.fasta
Number of total seqs for Ahyacinthus.transcripts.fasta: 27110
Number of seqs over 250 for Ahyacinthus.transcripts.fasta: 25810
```

  * multiqc on Plob, Ahya, Mgris (Pver multi-thread finished)

```bash
[dbarshis@coreV2-25-044 rawfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/2022-11_AmSam_BMKData/rawfastqs
[dbarshis@coreV2-25-044 rawfastqs]$ multiqc ./
multiqc: Command not found.
[dbarshis@coreV2-25-044 rawfastqs]$ crun multiqc ./
[WARNING]         multiqc : MultiQC Version v1.13 now available!
[INFO   ]         multiqc : This is MultiQC v1.9
[INFO   ]         multiqc : Template    : default
[INFO   ]         multiqc : Searching   : /cm/shared/courses/dbarshis/barshislab/danb/taxons/2022-11_AmSam_BMKData/rawfastqs
Searching 643 files..  [####################################]  100%          
[INFO   ]        cutadapt : Found 154 reports
[INFO   ]          fastqc : Found 122 reports
[INFO   ]         multiqc : Compressing plot data
[WARNING]         multiqc : Previous MultiQC output found! Adjusting filenames..
[WARNING]         multiqc : Use -f or --force to overwrite existing reports instead
[INFO   ]         multiqc : Report      : multiqc_report_1.html
[INFO   ]         multiqc : Data        : multiqc_data_1
[INFO   ]         multiqc : MultiQC complete
```

  * multiqc on Pver multithread

```bash

```

  * Pver transcriptome

```bash
(base) danbarshis@BIOLLBB0 ~ % cd /Users/danbarshis/dansstuff/Projeks/ODU/Projeks/reference_omes/Pocillopora_verrucosa
(base) danbarshis@BIOLLBB0 Pocillopora_verrucosa % python2 ~/scripts/fasta_manips/avg_cov_len_fasta_DJB.py Pver_transcriptome_v1.0.fasta
The total number of sequences is 49384
The average sequence length is 1355
The total number of bases is 66939700
The minimum sequence length is 301
The maximum sequence length is 37784
The N50 is 2161
Median Length = 1177
contigs < 150bp = 0
contigs >= 500bp = 34525
contigs >= 1000bp = 21537
contigs >= 2000bp = 10374
(base) danbarshis@BIOLLBB0 Pocillopora_verrucosa % pwd
/Users/danbarshis/dansstuff/Projeks/ODU/Projeks/reference_omes/Pocillopora_verrucosa
(base) danbarshis@BIOLLBB0 Pocillopora_verrucosa % python2 ~/scripts/fasta_manips/fasta_len_filter.py 250 _over250 Pver_transcriptome_v1.0.fasta
Number of total seqs for Pver_transcriptome_v1.0.fasta: 49384
Number of seqs over 250 for Pver_transcriptome_v1.0.fasta: 49384
(base) danbarshis@BIOLLBB0 Pocillopora_verrucosa % python2 ~/scripts/fasta_manips/fasta_len_filter.py 500 _over500 Pver_transcriptome_v1.0.fasta
Number of total seqs for Pver_transcriptome_v1.0.fasta: 49384
Number of seqs over 500 for Pver_transcriptome_v1.0.fasta: 34526
```

  * format references in STAR

```bash
####Pocillopora verrucosa
[dbarshis@coreV2-25-044 ReferenceTranscriptome]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/ReferenceTranscriptome
[dbarshis@coreV2-25-044 ReferenceTranscriptome]$ cat GenomeGenerate.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-04_PvergenomeGenerate.txt
#SBATCH -n 4
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Pvergenomegenerate

enable_lmod

module load container_env star

STAR --runMode genomeGenerate --runThreadN 4 --genomeDir ./ --genomeFastaFiles Pver_transcriptome_v1.0_over500.fasta --genomeChrBinNbits 16

[dbarshis@coreV2-25-044 ReferenceTranscriptome]$ sbatch GenomeGenerate.sh 
Submitted batch job 9810891
####Acropora_hyacinthus_López
[dbarshis@coreV2-25-044 ReferenceTranscriptome]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome
[dbarshis@coreV2-25-044 ReferenceTranscriptome]$ cat Ah
Ahyacinthus_transcripts_over500_Lopez-Nandam.fasta* AhyaGenomeGenerate.sh*                              
[dbarshis@coreV2-25-044 ReferenceTranscriptome]$ cat AhyaGenomeGenerate.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-04_AhyagenomeGenerate.txt
#SBATCH -n 4
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Ahyagenomegenerate

enable_lmod

module load container_env star

STAR --runMode genomeGenerate --runThreadN 4 --genomeDir ./ --genomeFastaFiles Ahyacinthus_transcripts_over500_Lopez-Nandam.fasta --genomeChrBinNbits 16

[dbarshis@coreV2-25-044 ReferenceTranscriptome]$ sbatch AhyaGenomeGenerate.sh 
Submitted batch job 9810892
```

  * Ahya alignment

```bash
[dbarshis@coreV2-25-044 qualtrimedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/qualtrimedfastqs
[dbarshis@coreV2-25-044 qualtrimedfastqs]$ cat AhyaStar.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-04_AhyaSTARMapping.txt
#SBATCH -n 16
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STARAhya

enable_lmod

module load container_env star

for i in *_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/ --runThreadN 16 --outSAMattributes All --outSAMattrRGline ID:${i%_TF_R1_val_1.fq.gz} --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2LopezAhya`; done

[dbarshis@coreV2-25-044 qualtrimedfastqs]$ sbatch AhyaStar.sh 
Submitted batch job 9810894
```

  * Pver alignment

```bash
[dbarshis@coreV2-25-044 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData/qualtrimmedfastqs
[dbarshis@coreV2-25-044 qualtrimmedfastqs]$ cat PverStar.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-04_PverSTARMapping.txt
#SBATCH -n 16
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STARPver

enable_lmod

module load container_env star

for i in *_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/ReferenceTranscriptome/ --runThreadN 16 --outSAMattributes All --outSAMattrRGline ID:${i%_TF_R1_val_1.fq.gz} --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2BuitragoPver`; done
[dbarshis@coreV2-25-044 qualtrimmedfastqs]$ sbatch PverStar.sh 
Submitted batch job 9810895
```

## 2022-11-05 MultiQC on Ahya and Pver STAR

  * Ahya

```bash
[dbarshis@coreV2-25-041 2022-11_AmSam_BMKData]$ enable_lmod

The following have been reloaded with a version change:
  1) slurm/20.11.5 => slurm/21.08

[dbarshis@coreV2-25-041 2022-11_AmSam_BMKData]$ module load container_env multiqc

The following have been reloaded with a version change:
  1) sge/2011.11p1 => sge/2011

[dbarshis@coreV2-25-041 2022-11_AmSam_BMKData]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData
[dbarshis@coreV2-25-041 2022-11_AmSam_BMKData]$ crun multiqc ./

  /// MultiQC 🔍 | v1.13

|           multiqc | Search path : /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData
|         searching | ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 277/277  
|              star | Found 16 reports
|          cutadapt | Found 32 reports
|            fastqc | Found 32 reports
|           multiqc | Compressing plot data
|           multiqc | Report      : multiqc_report.html
|           multiqc | Data        : multiqc_data
|           multiqc | MultiQC complete
```

  * Pver

```bash
[dbarshis@coreV2-25-041 2022-11_AmSamBMKData]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData
[dbarshis@coreV2-25-041 2022-11_AmSamBMKData]$ crun multiqc ./

  /// MultiQC 🔍 | v1.13

|           multiqc | Search path : /cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData
|         searching | ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 481/481  
|              star | Found 28 reports
|          cutadapt | Found 56 reports
|            fastqc | Found 56 reports
|           multiqc | Compressing plot data
|           multiqc | Report      : multiqc_report.html
|           multiqc | Data        : multiqc_data
|           multiqc | MultiQC complete
```

## 2022-11-06 Comparing Barshis-Palubmi Ahya transcriptome

  * formatting reference

```bash
[dbarshis@turing1 Barshis-Palumbi]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Barshis-Palumbi
[dbarshis@turing1 Barshis-Palumbi]$ cat AhyaGenomeGenerate.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-06_AhyagenomeGenerate.txt
#SBATCH -n 4
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Ahyagenomegenerate

enable_lmod

module load container_env star

STAR --runMode genomeGenerate --runThreadN 4 --genomeDir ./ --genomeFastaFiles 33496_Ahyacinthus_CoralContigs_suffixed.fasta --genomeChrBinNbits 16

[dbarshis@turing1 Barshis-Palumbi]$ sbatch AhyaGenomeGenerate.sh 
Submitted batch job 9810966
```

  * mapping to PalubmiReference

```bash
[dbarshis@turing1 qualtrimedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/qualtrimedfastqs
[dbarshis@turing1 qualtrimedfastqs]$ cat AhyaStar2BarPal.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-06_AhyaSTARMapping2BarPal.txt
#SBATCH -n 16
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STARAhyaBarPal

enable_lmod

module load container_env star

for i in *_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Barshis-Palumbi/ --runThreadN 16 --outSAMattributes All --outSAMattrRGline ID:${i%_TF_R1_val_1.fq.gz} --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2BarPalAhya`; done

[dbarshis@turing1 qualtrimedfastqs]$ sbatch AhyaStar2BarPal.sh 
Submitted batch job 9810967
```

## 2022-11-07_STARing Mgris/Plob

  * genome format Mcap

```bash
[dbarshis@turing1 M_capitata_Stephens-Bhattacharya_v3]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/ReferenceTranscriptome/M_capitata_Stephens-Bhattacharya_v3
[dbarshis@turing1 M_capitata_Stephens-Bhattacharya_v3]$ cat McapGenomeGenerate.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_McapgenomeGenerate.txt
#SBATCH -n 4
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Mcapgenomegenerate

enable_lmod

module load container_env star

STAR --runMode genomeGenerate --runThreadN 4 --genomeDir ./ --genomeFastaFiles Montipora_capitata_SteBhat_cds.fasta --genomeChrBinNbits 16

[dbarshis@turing1 M_capitata_Stephens-Bhattacharya_v3]$ sbatch McapGenomeGenerate.sh 
Submitted batch job 9811093
```

  * now Star aligning to Mcap

```bash
[dbarshis@turing1 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/2022-11_AmSam_BMKData/qualtrimmedfastqs
[dbarshis@turing1 qualtrimmedfastqs]$ cat StarMgri.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_MgriSTARMapping.txt
#SBATCH -n 16
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STARMgri

enable_lmod

module load container_env star

for i in *_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/ReferenceTranscriptome/M_capitata_Stephens-Bhattacharya_v3/ --runThreadN 16 --outSAMattributes All --outSAMattrRGline ID:${i%_TF_R1_val_1.fq.gz} --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2SteBhaMcap`; done

[dbarshis@turing1 qualtrimmedfastqs]$ sbatch StarMgri.sh 
Submitted batch job 9811223
```

  * comparing VoolstraPlut and KenkelPlob
  * formatting

```bash
####Formatting plut
[dbarshis@coreV2-25-031 VoolstraPlut]$ pwd 
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/VoolstraPlut
[dbarshis@coreV2-25-031 VoolstraPlut]$ cat PlutGenomeGenerate.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_PlutgenomeGenerate.txt
#SBATCH -n 4
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Plutgenomegenerate

enable_lmod

module load container_env star

STAR --runMode genomeGenerate --runThreadN 4 --genomeDir ./ --genomeFastaFiles  plut2v1.1.transcripts.fasta --genomeChrBinNbits 16
[dbarshis@coreV2-25-031 VoolstraPlut]$ sbatch PlutGenomeGenerate.sh 
Submitted batch job 9811233

#### Formatting Plob
[dbarshis@coreV2-25-031 KenkelPlob]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/KenkelPlob
[dbarshis@coreV2-25-031 KenkelPlob]$ cat PlobGenomeGenerate.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_PlobgenomeGenerate.txt
#SBATCH -n 4
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Plobgenomegenerate

enable_lmod

module load container_env star

STAR --runMode genomeGenerate --runThreadN 4 --genomeDir ./ --genomeFastaFiles  plob_LongestContig.fasta --genomeChrBinNbits 16
[dbarshis@coreV2-25-031 KenkelPlob]$ sbatch PlobGenomeGenerate.sh 
Submitted batch job 9811235
```

  * now aligning 2 Voolstra Plut

```bash
[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/2022-11_AmSam_BMKData/qualtrimmedfastqs
[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ cat StarPlut.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_PlutSTARMapping.txt
#SBATCH -n 16
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STARPlut

enable_lmod

module load container_env star

for i in *_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/VoolstraPlut/ --runThreadN 16 --outSAMattributes All --outSAMattrRGline ID:${i%_TF_R1_val_1.fq.gz} --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2VoolPlut`; done

[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ sbatch StarPlut.sh 
Submitted batch job 9811239
```

  * now aligning 2 Kenkel Plob

```bash
[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/2022-11_AmSam_BMKData/qualtrimmedfastqs
[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ cat StarPlob.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_PlobSTARMapping.txt
#SBATCH -n 16
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STARPlob

enable_lmod

module load container_env star

for i in *_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/KenkelPlob/ --runThreadN 16 --outSAMattributes All --outSAMattrRGline ID:${i%_TF_R1_val_1.fq.gz} --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2KenkPlob`; done
[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ sbatch StarPlob.sh 
Submitted batch job 9811240
```

  * Starting genotyping on Ahya

```bash
#### Need to index fasta files
[dbarshis@coreV2-25-031 ReferenceTranscriptome]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome
[dbarshis@coreV2-25-031 qualtrimedfastqs]$ module load container_env samtools
[dbarshis@coreV2-25-031 ReferenceTranscriptome]$ crun samtools faidx Ahyacinthus_transcripts_over500_Lopez-Nandam.fasta
[dbarshis@coreV2-25-031 Barshis-Palumbi]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Barshis-Palumbi
[dbarshis@coreV2-25-031 Barshis-Palumbi]$ crun samtools faidx 33496_Ahyacinthus_CoralContigs_suffixed.fasta

#### Need to sort BAMs and index sorted BAMs
#### BarPal
[dbarshis@turing1 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/mapping/Barshis-Palumbi/BAMs
[dbarshis@turing1 BAMs]$ cat BamSortIdx.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_AhyaBarPalSortIdx.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=AhyaBarPalSortIdx

enable_lmod

module load container_env samtools
for i in *.out.bam; do `crun samtools sort -O bam -o ${i%.out.bam}_sorted.bam $i`; done
for i in *_sorted.bam; do `crun samtools index $i`; done

[dbarshis@turing1 BAMs]$ sbatch BamSortIdx.sh 
Submitted batch job 9811248

####Lopez-Nandam
[dbarshis@coreV2-25-031 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/mapping/Lopez-Nandam/BAMs
[dbarshis@coreV2-25-031 BAMs]$ cat LopezBamSortIdx.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_AhyaLopezSortIdx.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=AhyaLopezSortIdx

enable_lmod

module load container_env samtools
for i in *.out.bam; do `crun samtools sort -O bam -o ${i%.out.bam}_sorted.bam $i`; done
for i in *_sorted.bam; do `crun samtools index $i`; done

[dbarshis@coreV2-25-031 BAMs]$ sbatch LopezBamSortIdx.sh 
Submitted batch job 9811252

#### Now vcf-ing
[dbarshis@coreV2-22-036 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/mapping/Barshis-Palumbi/BAMs
[dbarshis@coreV2-22-036 BAMs]$ cat BarPalVCFing.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_AhyaBarPalSNPing.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=AhyaBarPalSNP

enable_lmod
module load container_env bcftools

crun bcftools mpileup -Ou -f /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Barshis-Palumbi/33496_Ahyacinthus_CoralContigs_suffixed.fasta *_sorted.bam | crun bcftools call -Ou -mv | crun bcftools filter -O z -s LowQual -e "QUAL<30 || DP>100" > Ahya_2BarPal_var.flt.vcf.gz

[dbarshis@coreV2-22-036 BAMs]$ sbatch BarPalVCFing.sh 
Submitted batch job 9811255

###also trying with no -O z option just in case
[dbarshis@coreV2-22-036 BAMs]$ cat BarPalVCFnoz.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_AhyaBarPalSNPing_noZ.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=NoZAhyaBarPalSNP

enable_lmod
module load container_env bcftools

crun bcftools mpileup -Ou -f /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Barshis-Palumbi/33496_Ahyacinthus_CoralContigs_suffixed.fasta *_sorted.bam | crun bcftools call -Ou -mv | crun bcftools filter -s LowQual -e "QUAL<30 || DP>100" > Ahya_2BarPal_var_noZ.flt.vcf
[dbarshis@coreV2-22-036 BAMs]$ sbatch BarPalVCFnoz.sh 
Submitted batch job 9811260
```

  * starting on Pver

```bash
####indexing .fasta
[dbarshis@turing1 ReferenceTranscriptome]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/ReferenceTranscriptome
[dbarshis@turing1 ReferenceTranscriptome]$ crun samtools faidx Pver_transcriptome_v1.0_over500.fasta

#### Sorting BAMs and index sorted BAMs
[dbarshis@coreV2-22-036 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData/mapping/BAMs
[dbarshis@coreV2-22-036 BAMs]$ cat PverBamSortIdx.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_PverSortIdx.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=PverSortIdx

enable_lmod

module load container_env samtools
for i in *.out.bam; do `crun samtools sort -O bam -o ${i%.out.bam}_sorted.bam $i`; done
for i in *_sorted.bam; do `crun samtools index $i`; done

[dbarshis@coreV2-22-036 BAMs]$ sbatch PverBamSortIdx.sh 
Submitted batch job 9811251
```