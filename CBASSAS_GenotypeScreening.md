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
#### Barshis-Palumbi
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

#### Lopez-Nandam
[dbarshis@coreV2-22-036 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/mapping/Lopez-Nandam/BAMs
[dbarshis@coreV2-22-036 BAMs]$ cat LopezVCFing.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-07_AhyaLopezSNPing_noZ.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=NoZAhyaLopezSNP

enable_lmod
module load container_env bcftools

crun bcftools mpileup -Ou -f /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Ahyacinthus_transcripts_over500_Lopez-Nandam.fasta *_sorted.bam | crun bcftools call -Ou -mv | crun bcftools filter -s LowQual -e "QUAL<30 || DP>100" > Ahya_2Lopez_var_noZ.flt.vcf

[dbarshis@coreV2-22-036 BAMs]$ sbatch LopezVCFing.sh 
Submitted batch job 9811264
```

## 2022-11-08_VCFFiltering Ahya2Lopez

```bash
[dbarshis@turing1 Lopez-Nandam]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/mapping/Lopez-Nandam/BAMs
[dbarshis@coreV2-25-031 BAMs]$ vcftools --vcf Ahya_2Lopez_var_noZ.flt.vcf --max-missing 0.5 --mac 3 --minQ 30 --minDP 10 --max-alleles 2 --maf 0.015 --remove-indels --recode --recode-INFO-all --out 244528_Ahya_2Lopez_HEAFiltersFinal

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf Ahya_2Lopez_var_noZ.flt.vcf
	--recode-INFO-all
	--mac 3
	--maf 0.015
	--max-alleles 2
	--minDP 10
	--minQ 30
	--max-missing 0.5
	--out 244528_Ahya_2Lopez_HEAFiltersFinal
	--recode
	--remove-indels

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 16 out of 16 Individuals
Outputting VCF file...
After filtering, kept 244528 out of a possible 768593 Sites
Run Time = 15.00 seconds

####Looking at missing individuals
[dbarshis@turing1 BAMs]$ vcftools --vcf 244528_Ahya_2Lopez_HEAFiltersFinal.recode.vcf --missing-indv

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 244528_Ahya_2Lopez_HEAFiltersFinal.recode.vcf
	--missing-indv

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 16 out of 16 Individuals
Outputting Individual Missingness
After filtering, kept 244528 out of a possible 244528 Sites
Run Time = 3.00 seconds
[dbarshis@turing1 BAMs]$ cat out.imiss
INDV	N_DATA	N_GENOTYPES_FILTERED	N_MISS	F_MISS
20220219T1400_CBASS_AS_S2_Ahya_2_TF_2LopezAhyaAligned_sorted.bam	244528	0	29733	0.121593
20220219T1400_CBASS_AS_S2_Ahya_5_TF_2LopezAhyaAligned_sorted.bam	244528	0	56925	0.232795
20220219T1400_CBASS_AS_S2_Ahya_6_TF_2LopezAhyaAligned_sorted.bam	244528	0	17005	0.0695421
20220219T1400_CBASS_AS_S2_Ahya_7_TF_2LopezAhyaAligned_sorted.bam	244528	0	22004	0.0899856
20220219T1400_CBASS_AS_S2_Ahya_9_TF_2LopezAhyaAligned_sorted.bam	244528	0	52466	0.21456
20220225T1400_CBASS_AS_S1_Ahya_10_TF_2LopezAhyaAligned_sorted.bam	244528	0	25144	0.102827
20220225T1400_CBASS_AS_S1_Ahya_13_TF_2LopezAhyaAligned_sorted.bam	244528	0	41835	0.171085
20220225T1400_CBASS_AS_S1_Ahya_14_TF_2LopezAhyaAligned_sorted.bam	244528	0	32593	0.133289
20220225T1400_CBASS_AS_S1_Ahya_1_TF_2LopezAhyaAligned_sorted.bam	244528	0	15209	0.0621974
20220225T1400_CBASS_AS_S1_Ahya_2_TF_2LopezAhyaAligned_sorted.bam	244528	0	69881	0.285779
20220225T1400_CBASS_AS_S1_Ahya_3_TF_2LopezAhyaAligned_sorted.bam	244528	0	56888	0.232644
20220225T1400_CBASS_AS_S1_Ahya_4_TF_2LopezAhyaAligned_sorted.bam	244528	0	11735	0.0479904
20220225T1400_CBASS_AS_S1_Ahya_5_TF_2LopezAhyaAligned_sorted.bam	244528	0	25072	0.102532
20220225T1400_CBASS_AS_S1_Ahya_6_TF_2LopezAhyaAligned_sorted.bam	244528	0	45281	0.185177
20220225T1400_CBASS_AS_S1_Ahya_7_TF_2LopezAhyaAligned_sorted.bam	244528	0	48437	0.198084
20220225T1400_CBASS_AS_S1_Ahya_9_TF_2LopezAhyaAligned_sorted.bam	244528	0	39957	0.163405

##87844 loci genotyped in all individuals!
[dbarshis@turing1 BAMs]$ vcftools --vcf 244528_Ahya_2Lopez_HEAFiltersFinal.recode.vcf --max-missing 1 --recode --recode-INFO-all --out 87844_Ahya_2Lopez_HEAFilters_allindividuals

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 244528_Ahya_2Lopez_HEAFiltersFinal.recode.vcf
	--recode-INFO-all
	--max-missing 1
	--out 87844_Ahya_2Lopez_HEAFilters_allindividuals
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 16 out of 16 Individuals
Outputting VCF file...
After filtering, kept 87844 out of a possible 244528 Sites
Run Time = 5.00 seconds

####Transferring to laptop
(base) danbarshis@BIOLLBB0 SNPTyping % pwd
/Users/danbarshis/dansstuff/Research/2021-2024_GlobalSearch/2022-AmSam/Sequencing/2022-11_BMKGenoTypingData/SNPTyping
(base) danbarshis@BIOLLBB0 SNPTyping % python2 ~/scripts/vcftogenepop_advbioinf.py 87844_Ahya_2Lopez_HEAFilters_allindividuals.recode.vcf Ahyapopfile.txt 
Indivs with genotypes in vcf file: CBASS_AS_S2_Ahya_2	CBASS_AS_S2_Ahya_5	CBASS_AS_S2_Ahya_6	CBASS_AS_S2_Ahya_7	CBASS_AS_S2_Ahya_9	CBASS_AS_S1_Ahya_10	CBASS_AS_S1_Ahya_13	CBASS_AS_S1_Ahya_14	CBASS_AS_S1_Ahya_1	CBASS_AS_S1_Ahya_2	CBASS_AS_S1_Ahya_3	CBASS_AS_S1_Ahya_4	CBASS_AS_S1_Ahya_5	CBASS_AS_S1_Ahya_6	CBASS_AS_S1_Ahya_7	CBASS_AS_S1_Ahya_9
20 87844 87844 87844 87844 16

```
  * running adegenet

```R
#Script written by Daniel Barshis, adapted by Hannah Aichelman, and re-adapted by Daniel Barshis
#Last updated: 11/2022

#This script takes genepop file formats (.gen) and uses principal components analyses and distance clustering to consider population structure and clonality

#load required packages
library(adegenet)
library(poppr)
library(gplots)
library(RColorBrewer)

#set working directory
setwd("/Users/danbarshis/dansstuff/Research/2021-2024_GlobalSearch/2022-AmSam/Sequencing/2022-11_BMKGenoTypingData/SNPTyping/Ahya2Lopez")

datafile28<-read.genepop('87844_Ahya_2Lopez_HEAFilters_allindividuals.recode_genepop.gen', ncode=2)

sum(is.na(datafile28$tab))
datafile28 #shows info
YOURdata<-scaleGen(datafile28, NA.method='mean')
X<-YOURdata
##for 87844_16samples
pca1 <- dudi.pca(X,cent=T, scale=T, scannf=F, nf=3)
summary(pca1)
propvar<-format(pca1$eig/sum(pca1$eig)*100, digits=1)

#### PCAs ####
# individual labels
s.label(pca1$li)

# population elipses
s.class(pca1$li, pop(datafile28))

#color symbols, pop names
pdf("Ahya2Lopez_87844_16samples_ColorPCA1v2.pdf")
Sytes<-c(rep("S2",5),rep("S1",9))
datafile28$pop<-factor(datafile28$pop, Sytes)
Colorz<-c('#2c7bb6','#fdae61')
names(Colorz)<-levels(datafile28$pop)
Syms<-c(19,17)
names(Syms)<-levels(datafile28$pop)
plot(pca1$li$Axis1, pca1$li$Axis2, col=Colorz[datafile28$pop], pch=Syms[datafile28$pop], xlim=c(min(pca1$li$Axis1),max(pca1$li$Axis1)), ylim=c(min(pca1$li$Axis2)-50,max(pca1$li$Axis2)+50), cex=3,xlab=paste("PC1, ",propvar[1],"% variance explained", sep=""), ylab=paste("PC2, ",propvar[2],"% variance explained)", sep=""), cex.axis=1.5, cex.lab=1.5)
abline(v=0,h=0,col="grey", lty=2)
legend("bottomright", c("S2","S1"), col=Colorz, pch=Syms, cex=1.5, pt.cex=3)
dev.off()


####Dissimilarity Distance####
#Create a diss.dist dissimilarity distance matrix to determine a threshhold based on clones, https://www.rdocumentation.org/packages/poppr/versions/2.8.5/topics/diss.dist
#ignores missing data and counts the shared genotypes 
distgenDISS <- diss.dist(datafile28, percent = FALSE, mat = FALSE) 
#make percent = TRUE to get Prevosti distance
#By default, diss.dist() returns a distance reflecting the number of allelic differences between two individuals (Hamming's distance).

distgenDISS2 <- as.matrix(distgenDISS)


pdf("Ahya2Lopez_87844_16samples_distance-dendrogram.pdf")
#Cluster with hclust and plot
clust_tree<-hclust(distgenDISS, "ave")
plot(clust_tree, cex=0.4)+abline(h=50)
dev.off()

```

  * running on Ahya2BarPal

```bash
[dbarshis@turing1 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/mapping/Barshis-Palumbi/BAMs
[dbarshis@turing1 BAMs]$ enable_lmod

The following have been reloaded with a version change:
  1) slurm/20.11.5 => slurm/21.08

[dbarshis@turing1 BAMs]$ module load vcftools

The following have been reloaded with a version change:
  1) sge/2011.11p1 => sge/2011

[dbarshis@turing1 BAMs]$ vcftools --vcf Ahya_2BarPal_var_noZ.flt.vcf 

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf Ahya_2BarPal_var_noZ.flt.vcf

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 16 out of 16 Individuals
After filtering, kept 498829 out of a possible 498829 Sites
Run Time = 4.00 seconds

### HEAFilters
[dbarshis@turing1 BAMs]$ vcftools --vcf Ahya_2BarPal_var_noZ.flt.vcf --max-missing 0.5 --mac 3 --minQ 30 --minDP 10 --max-alleles 2 --maf 0.015 --remove-indels --recode --recode-INFO-all --out 173230_Ahya_2BarPal_var.vcf

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf Ahya_2BarPal_var_noZ.flt.vcf
	--recode-INFO-all
	--mac 3
	--maf 0.015
	--max-alleles 2
	--minDP 10
	--minQ 30
	--max-missing 0.5
	--out 173230_Ahya_2BarPal_var.vcf
	--recode
	--remove-indels

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 16 out of 16 Individuals
Outputting VCF file...
After filtering, kept 173230 out of a possible 498829 Sites
Run Time = 12.00 seconds

### max missing 1
[dbarshis@turing1 BAMs]$ vcftools --vcf 173230_Ahya_2BarPal_var.vcf.recode.vcf --max-missing 1 --recode --recode-INFO-all --out 76942_Ahya_2BarPal_var

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 173230_Ahya_2BarPal_var.vcf.recode.vcf
	--recode-INFO-all
	--max-missing 1
	--out 76942_Ahya_2BarPal_var
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 16 out of 16 Individuals
Outputting VCF file...
After filtering, kept 76942 out of a possible 173230 Sites
Run Time = 5.00 seconds

##Transferring to laptop
(base) danbarshis@BIOLLBB0 Ahya2BarPal % pwd
/Users/danbarshis/dansstuff/Research/2021-2024_GlobalSearch/2022-AmSam/Sequencing/2022-11_BMKGenoTypingData/SNPTyping/Ahya2BarPal
(base) danbarshis@BIOLLBB0 Ahya2BarPal % python2 ~/scripts/vcftogenepop_advbioinf.py 76942_Ahya_2BarPal_var.recode.vcf Popfile.txt 
Indivs with genotypes in vcf file: CBASS_AS_S2_Ahya_2	CBASS_AS_S2_Ahya_5	CBASS_AS_S2_Ahya_6	CBASS_AS_S2_Ahya_7	CBASS_AS_S2_Ahya_9	CBASS_AS_S1_Ahya_10	CBASS_AS_S1_Ahya_13	CBASS_AS_S1_Ahya_14	CBASS_AS_S1_Ahya_1	CBASS_AS_S1_Ahya_2	CBASS_AS_S1_Ahya_3	CBASS_AS_S1_Ahya_4	CBASS_AS_S1_Ahya_5	CBASS_AS_S1_Ahya_6	CBASS_AS_S1_Ahya_7	CBASS_AS_S1_Ahya_9
20 76942 76942 76942 76942 16
```

  * now adegenet

```R
#Script written by Daniel Barshis, adapted by Hannah Aichelman, and re-adapted by Daniel Barshis
#Last updated: 11/2022

#This script takes genepop file formats (.gen) and uses principal components analyses and distance clustering to consider population structure and clonality

#load required packages
library(adegenet)
library(poppr)
library(gplots)
library(RColorBrewer)

#set working directory
setwd("/Users/danbarshis/dansstuff/Research/2021-2024_GlobalSearch/2022-AmSam/Sequencing/2022-11_BMKGenoTypingData/Acropora_hyacinthus/SNPTyping/Ahya2BarPal")

datafile<-read.genepop('76942_Ahya_2BarPal_var.recode_genepop.gen', ncode=2)

sum(is.na(datafile$tab))
datafile #shows info
YOURdata<-scaleGen(datafile, NA.method='mean')
X<-YOURdata
##for 76942_16samples
pca1 <- dudi.pca(X,cent=T, scale=T, scannf=F, nf=3)
summary(pca1)
propvar<-format(pca1$eig/sum(pca1$eig)*100, digits=1)

#### PCAs ####
# individual labels
s.label(pca1$li)

# population elipses
s.class(pca1$li, pop(datafile))

#color symbols, pop names
pdf("Ahya2BarPal_76942_16samples_ColorPCA1v2.pdf")
Sytes<-c(rep("S2",5),rep("S1",9))
datafile$pop<-factor(datafile$pop, Sytes)
Colorz<-c('#2c7bb6','#fdae61')
names(Colorz)<-levels(datafile$pop)
Syms<-c(19,17)
names(Syms)<-levels(datafile$pop)
plot(pca1$li$Axis1, pca1$li$Axis2, col=Colorz[datafile$pop], pch=Syms[datafile$pop], xlim=c(min(pca1$li$Axis1),max(pca1$li$Axis1)), ylim=c(min(pca1$li$Axis2)-50,max(pca1$li$Axis2)+50), cex=3,xlab=paste("PC1, ",propvar[1],"% variance explained", sep=""), ylab=paste("PC2, ",propvar[2],"% variance explained)", sep=""), cex.axis=1.5, cex.lab=1.5)
abline(v=0,h=0,col="grey", lty=2)
legend("bottomright", c("S2","S1"), col=Colorz, pch=Syms, cex=1.5, pt.cex=3)
dev.off()


####Dissimilarity Distance####
#Create a diss.dist dissimilarity distance matrix to determine a threshhold based on clones, https://www.rdocumentation.org/packages/poppr/versions/2.8.5/topics/diss.dist
#ignores missing data and counts the shared genotypes 
distgenDISS <- diss.dist(datafile, percent = FALSE, mat = FALSE) 
#make percent = TRUE to get Prevosti distance
#By default, diss.dist() returns a distance reflecting the number of allelic differences between two individuals (Hamming's distance).

distgenDISS2 <- as.matrix(distgenDISS)


pdf("Ahya2BarPal_76942_16samples_distance-dendrogram.pdf")
#Cluster with hclust and plot
clust_tree<-hclust(distgenDISS, "ave")
plot(clust_tree, cex=0.4)+abline(h=50)
dev.off()
```

  * VCFing Pver

```bash
[dbarshis@coreV1-22-015 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData/mapping/BAMs
[dbarshis@coreV1-22-015 BAMs]$ cat PverBuitVCFing.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-08_PverBuitSNPing.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=PverBuitSNP

enable_lmod
module load container_env bcftools

crun bcftools mpileup -Ou -f /cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/ReferenceTranscriptome/Pver_transcriptome_v1.0_over500.fasta *_sorted.bam | crun bcftools call -Ou -mv | crun bcftools filter -s LowQual -e "QUAL<30 || DP>100" > Pver_2Buit_var.flt.vcf
[dbarshis@coreV1-22-015 BAMs]$ sbatch PverBuitVCFing.sh 
Submitted batch job 9811319
```

  * SortIndexing Mgris

```bash
[dbarshis@coreV1-22-015 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/2022-11_AmSam_BMKData/mapping/BAMs
[dbarshis@coreV1-22-015 BAMs]$ cat BamSortIdx.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-08_McapSortIdx.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=MacpSortIdx

enable_lmod

module load container_env samtools
for i in *.out.bam; do `crun samtools sort -O bam -o ${i%.out.bam}_sorted.bam $i`; done
for i in *_sorted.bam; do `crun samtools index $i`; done

[dbarshis@coreV1-22-015 BAMs]$ sbatch BamSortIdx.sh 
Submitted batch job 9811320
```

  * PlobPlut SortIdx

```bash
####Plob
[dbarshis@coreV1-22-015 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/2022-11_AmSam_BMKData/mapping/KenkPlob/BAMs
[dbarshis@coreV1-22-015 BAMs]$ cat KenkPlobSortIdx.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-08_PlobKenkSortIdx.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=PlobKenkSortIdx

enable_lmod

module load container_env samtools
for i in *.out.bam; do `crun samtools sort -O bam -o ${i%.out.bam}_sorted.bam $i`; done
for i in *_sorted.bam; do `crun samtools index $i`; done

[dbarshis@coreV1-22-015 BAMs]$ sbatch KenkPlobSortIdx.sh 
Submitted batch job 9811321

####Plut
[dbarshis@coreV1-22-015 VoolstraPlut]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/VoolstraPlut
[dbarshis@coreV1-22-015 VoolstraPlut]$ crun samtools faidx plut2v1.1.transcripts.fasta 
[dbarshis@coreV1-22-015 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/2022-11_AmSam_BMKData/mapping/VoolPlut/BAMs
[dbarshis@coreV1-22-015 BAMs]$ cat VoolPlutSortIdx.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-08_PlutVoolSortIdx.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=PlutVoolSortIdx

enable_lmod

module load container_env samtools
for i in *.out.bam; do `crun samtools sort -O bam -o ${i%.out.bam}_sorted.bam $i`; done
for i in *_sorted.bam; do `crun samtools index $i`; done

[dbarshis@coreV1-22-015 BAMs]$ sbatch VoolPlutSortIdx.sh 
Submitted batch job 9811322
```

  * VCFing Mgris

```bash
[dbarshis@coreV1-22-015 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/2022-11_AmSam_BMKData/mapping/BAMs
[dbarshis@coreV1-22-015 BAMs]$ cat Mgris2McapSteBhatVCF.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-08_MgrisSteBhaSNPing.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=MgrisSteBhaSNP

enable_lmod
module load container_env bcftools

crun bcftools mpileup -Ou -f /cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/ReferenceTranscriptome/M_capitata_Stephens-Bhattacharya_v3/Montipora_capitata_SteBhat_cds.fasta *_sorted.bam | crun bcftools call -Ou -mv | crun bcftools filter -s LowQual -e "QUAL<30 || DP>100" > Mgri_2SteBha_var.flt.vcf
[dbarshis@coreV1-22-015 BAMs]$ sbatch Mgris2McapSteBhatVCF.sh 
Submitted batch job 9811330
```

  * VCFing PlobPlut

```bash
[dbarshis@coreV1-22-015 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/2022-11_AmSam_BMKData/mapping/VoolPlut/BAMs
[dbarshis@coreV1-22-015 BAMs]$ cat PlobVoolPlutVCF.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-08_PlobVoolPlutSNPing.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=PlobVoolPlutSNP

enable_lmod
module load container_env bcftools

crun bcftools mpileup -Ou -f /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/VoolstraPlut/plut2v1.1.transcripts.fasta *_sorted.bam | crun bcftools call -Ou -mv | crun bcftools filter -s LowQual -e "QUAL<30 || DP>100" > Plob_2VoolPlut_var.flt.vcf

[dbarshis@coreV1-22-015 BAMs]$ sbatch PlobVoolPlutVCF.sh 
Submitted batch job 9811328

```

## 2022-11-09_MultiQCing species by species

  * Acropora hyacinthus

```bash
[dbarshis@coreV2-22-036 2022-11_AmSam_BMKData]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData
[dbarshis@coreV2-22-036 2022-11_AmSam_BMKData]$ crun multiqc ./

  /// MultiQC 🔍 | v1.13

|           multiqc | Search path : /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData
|         searching | ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 481/481  
|              star | Found 32 reports
|          cutadapt | Found 32 reports
|            fastqc | Found 32 reports
|           multiqc | Compressing plot data
|           multiqc | Report      : multiqc_report.html
|           multiqc | Data        : multiqc_data
|           multiqc | MultiQC complete
```

  * Pocillopora_verrucosa

```bash
[dbarshis@coreV2-22-036 2022-11_AmSamBMKData]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData
[dbarshis@coreV2-22-036 2022-11_AmSamBMKData]$ crun multiqc ./

  /// MultiQC 🔍 | v1.13

|           multiqc | Search path : /cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData
|         searching | ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 542/542  
|              star | Found 28 reports
|          cutadapt | Found 56 reports
|            fastqc | Found 56 reports
|           multiqc | Compressing plot data
|           multiqc | Report      : multiqc_report.html
|           multiqc | Data        : multiqc_data
|           multiqc | MultiQC complete
```

  * Porites_lobata

```bash
[dbarshis@coreV2-22-036 2022-11_AmSam_BMKData]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/2022-11_AmSam_BMKData
[dbarshis@coreV2-22-036 2022-11_AmSam_BMKData]$ crun multiqc ./

  /// MultiQC 🔍 | v1.13

|           multiqc | Search path : /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/2022-11_AmSam_BMKData
|         searching | ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 545/545  
|              star | Found 38 reports
|          cutadapt | Found 70 reports
|            fastqc | Found 38 reports
|           multiqc | Compressing plot data
|           multiqc | Report      : multiqc_report.html
|           multiqc | Data        : multiqc_data
|           multiqc | MultiQC complete
```

  * Montipora_grisea

```bash
[dbarshis@coreV2-22-036 2022-11_AmSam_BMKData]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/2022-11_AmSam_BMKData
[dbarshis@coreV2-22-036 2022-11_AmSam_BMKData]$ crun multiqc ./

  /// MultiQC 🔍 | v1.13

|           multiqc | Search path : /cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/2022-11_AmSam_BMKData
|         searching | ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 503/503  
|              star | Found 26 reports
|          cutadapt | Found 52 reports
|            fastqc | Found 52 reports
|           multiqc | Compressing plot data
|           multiqc | Report      : multiqc_report.html
|           multiqc | Data        : multiqc_data
|           multiqc | MultiQC complete
```

  * SNP results from M_gris

```bash
[dbarshis@coreV2-25-031 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/2022-11_AmSam_BMKData/mapping/BAMs
[dbarshis@coreV2-25-031 BAMs]$ vcftools --vcf Mgri_2SteBha_var.flt.vcf 

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf Mgri_2SteBha_var.flt.vcf

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 26 out of 26 Individuals
After filtering, kept 1611826 out of a possible 1611826 Sites
Run Time = 14.00 seconds

[dbarshis@coreV2-25-031 BAMs]$ vcftools --vcf Mgri_2SteBha_var.flt.vcf --max-missing 0.5 --mac 3 --minQ 30 --minDP 10 --max-alleles 2 --maf 0.015 --remove-indels --recode --recode-INFO-all --out 425841_Mgri_2SteBha_var

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf Mgri_2SteBha_var.flt.vcf
	--recode-INFO-all
	--mac 3
	--maf 0.015
	--max-alleles 2
	--minDP 10
	--minQ 30
	--max-missing 0.5
	--out 425841_Mgri_2SteBha_var
	--recode
	--remove-indels

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 26 out of 26 Individuals
Outputting VCF file...
After filtering, kept 425841 out of a possible 1611826 Sites
Run Time = 39.00 seconds

[dbarshis@coreV2-25-031 BAMs]$ vcftools --vcf 425841_Mgri_2SteBha_var.recode.vcf --max-missing 1 --recode --recode-INFO-all --out 89262_Mgri_2SteBha_all

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 425841_Mgri_2SteBha_var.recode.vcf
	--recode-INFO-all
	--max-missing 1
	--out 89262_Mgri_2SteBha_all
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 26 out of 26 Individuals
Outputting VCF file...
After filtering, kept 89262 out of a possible 425841 Sites
Run Time = 10.00 seconds

(base) danbarshis@BIOLLBB0 SNP_results % pwd    
/Users/danbarshis/dansstuff/Research/2021-2024_GlobalSearch/2022-AmSam/Sequencing/2022-11_BMKGenoTypingData/Montipora_grisea/SNP_results
(base) danbarshis@BIOLLBB0 SNP_results % python2 ~/scripts/vcftogenepop_advbioinf.py 89262_Mgri_2SteBha_all.recode.vcf popfile.txt 
Indivs with genotypes in vcf file: CBASS_AS_S2_Mgri_10	CBASS_AS_S2_Mgri_11	CBASS_AS_S2_Mgri_12	CBASS_AS_S2_Mgri_13	CBASS_AS_S2_Mgri_14	CBASS_AS_S2_Mgri_1	CBASS_AS_S2_Mgri_2	CBASS_AS_S2_Mgri_3	CBASS_AS_S2_Mgri_4	CBASS_AS_S2_Mgri_5	CBASS_AS_S2_Mgri_6	CBASS_AS_S2_Mgri_7	CBASS_AS_S2_Mgri_8	CBASS_AS_S2_Mgri_9	CBASS_AS_S1_Mgri_10	CBASS_AS_S1_Mgri_11	CBASS_AS_S1_Mgri_12	CBASS_AS_S1_Mgri_1	CBASS_AS_S1_Mgri_2	CBASS_AS_S1_Mgri_3	CBASS_AS_S1_Mgri_4	CBASS_AS_S1_Mgri_5	CBASS_AS_S1_Mgri_6	CBASS_AS_S1_Mgri_7	CBASS_AS_S1_Mgri_8	CBASS_AS_S1_Mgri_9
30 89262 89262 89262 89262 26
```

  * SNP results from Plob

```bash
[dbarshis@coreV2-25-031 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/2022-11_AmSam_BMKData/mapping/VoolPlut/BAMs
[dbarshis@coreV2-25-031 BAMs]$ vcftools --vcf Plob_2VoolPlut_var.flt.vcf 

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf Plob_2VoolPlut_var.flt.vcf

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 19 out of 19 Individuals
After filtering, kept 2448543 out of a possible 2448543 Sites
Run Time = 18.00 seconds

[dbarshis@coreV2-25-031 BAMs]$ vcftools --vcf Plob_2VoolPlut_var.flt.vcf --max-missing 0.5 --mac 3 --minQ 30 --minDP 10 --max-alleles 2 --maf 0.015 --remove-indels --recode --recode-INFO-all --out 880067_Plob_2VoolPlut

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf Plob_2VoolPlut_var.flt.vcf
	--recode-INFO-all
	--mac 3
	--maf 0.015
	--max-alleles 2
	--minDP 10
	--minQ 30
	--max-missing 0.5
	--out 880067_Plob_2VoolPlut
	--recode
	--remove-indels

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 19 out of 19 Individuals
Outputting VCF file...
After filtering, kept 880067 out of a possible 2448543 Sites
Run Time = 57.00 seconds

[dbarshis@coreV2-25-031 BAMs]$ vcftools --vcf 880067_Plob_2VoolPlut.recode.vcf --max-missing 1 --recode --recode-INFO-all --out 179099_Plob_2VoolPlut_all

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 880067_Plob_2VoolPlut.recode.vcf
	--recode-INFO-all
	--max-missing 1
	--out 179099_Plob_2VoolPlut_all
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 19 out of 19 Individuals
Outputting VCF file...
After filtering, kept 179099 out of a possible 880067 Sites
Run Time = 16.00 seconds
(base) danbarshis@BIOLLBB0 SNP_results % pwd   
/Users/danbarshis/dansstuff/Research/2021-2024_GlobalSearch/2022-AmSam/Sequencing/2022-11_BMKGenoTypingData/Porites_lobata/SNP_results
(base) danbarshis@BIOLLBB0 SNP_results % python2 ~/scripts/vcftogenepop_advbioinf.py 179099_Plob_2VoolPlut_all.recode.vcf popfile.txt 
Indivs with genotypes in vcf file: CBASS_AS_S2_Plob_10	CBASS_AS_S2_Plob_11	CBASS_AS_S2_Plob_12	CBASS_AS_S2_Plob_1	CBASS_AS_S2_Plob_2	CBASS_AS_S2_Plob_4	CBASS_AS_S2_Plob_5	CBASS_AS_S2_Plob_6	CBASS_AS_S2_Plob_7	CBASS_AS_S2_Plob_8	CBASS_AS_S2_Plob_9	CBASS_AS_S1_Plob_10	CBASS_AS_S1_Plob_11	CBASS_AS_S1_Plob_12	CBASS_AS_S1_Plob_4	CBASS_AS_S1_Plob_5	CBASS_AS_S1_Plob_6	CBASS_AS_S1_Plob_7	CBASS_AS_S1_Plob_9
23 179099 179099 179099 179099 19
```

## 2022-11-11_VCF of Pver

```bash
[dbarshis@turing1 BAMs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData/mapping/BAMs
[dbarshis@turing1 BAMs]$ vcftools --vcf Pver_2Buit_var.flt.vcf 

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf Pver_2Buit_var.flt.vcf

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 28 out of 28 Individuals
After filtering, kept 2030822 out of a possible 2030822 Sites
Run Time = 19.00 seconds

[dbarshis@coreV1-22-015 BAMs]$ vcftools --vcf Pver_2Buit_var.flt.vcf --max-missing 0.5 --mac 3 --minQ 30 --minDP 10 --max-alleles 2 --maf 0.015 --remove-indels --recode --recode-INFO-all --out 885656_Pver_2Buit_HEAFilt

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf Pver_2Buit_var.flt.vcf
	--recode-INFO-all
	--mac 3
	--maf 0.015
	--max-alleles 2
	--minDP 10
	--minQ 30
	--max-missing 0.5
	--out 885656_Pver_2Buit_HEAFilt
	--recode
	--remove-indels

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 28 out of 28 Individuals
Outputting VCF file...
After filtering, kept 885656 out of a possible 2030822 Sites
Run Time = 68.00 seconds

[dbarshis@coreV1-22-015 BAMs]$ vcftools --vcf 885656_Pver_2Buit_HEAFilt.recode.vcf --max-missing 1 --recode --recode-INFO-all --out 405068_Pver_2Buit_AllSamps

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 885656_Pver_2Buit_HEAFilt.recode.vcf
	--recode-INFO-all
	--max-missing 1
	--out 405068_Pver_2Buit_AllSamps
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 28 out of 28 Individuals
Outputting VCF file...
After filtering, kept 405068 out of a possible 885656 Sites
Run Time = 31.00 seconds
(base) danbarshis@BIOLLBB0 SNP_results % pwd
/Users/danbarshis/dansstuff/Research/2021-2024_GlobalSearch/2022-AmSam/Sequencing/2022-11_BMKGenoTypingData/Pocillopora_verrucosa/SNP_results
(base) danbarshis@BIOLLBB0 SNP_results % python2 ~/scripts/vcftogenepop_advbioinf.py 405068_Pver_2Buit_AllSamps.recode.vcf popfile.txt 
Indivs with genotypes in vcf file: CBASS_AS_S2_Pver_10	CBASS_AS_S2_Pver_11	CBASS_AS_S2_Pver_12	CBASS_AS_S2_Pver_13	CBASS_AS_S2_Pver_14	CBASS_AS_S2_Pver_1	CBASS_AS_S2_Pver_2	CBASS_AS_S2_Pver_3	CBASS_AS_S2_Pver_4	CBASS_AS_S2_Pver_5	CBASS_AS_S2_Pver_6	CBASS_AS_S2_Pver_7	CBASS_AS_S2_Pver_8	CBASS_AS_S2_Pver_9	CBASS_AS_S1_Pver_10	CBASS_AS_S1_Pver_11	CBASS_AS_S1_Pver_12	CBASS_AS_S1_Pver_13	CBASS_AS_S1_Pver_14	CBASS_AS_S1_Pver_1	CBASS_AS_S1_Pver_2	CBASS_AS_S1_Pver_3	CBASS_AS_S1_Pver_4	CBASS_AS_S1_Pver_5	CBASS_AS_S1_Pver_6	CBASS_AS_S1_Pver_7	CBASS_AS_S1_Pver_8	CBASS_AS_S1_Pver_9
32 405068 405068 405068 405068 28
```

## 2022-11-29_Re-do with hybrid refs and full pipeline

  * Plutea with C15 from reefgenomics

```bash
[dbarshis@coreV1-22-015 VoolstraPlutplusC15]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/VoolstraPlutplusC15
[dbarshis@coreV1-22-015 VoolstraPlutplusC15]$ ls
plut2v1.1.transcripts_suffixed.fasta  SymbC15_plutea_v2.1_suffixed.fasta
[dbarshis@coreV1-22-015 VoolstraPlutplusC15]$ cat plut2v1.1.transcripts_suffixed.fasta SymbC15_plutea_v2.1_suffixed.fasta > VoolstraPlut_C15_hybrid.fasta
[dbarshis@turing1 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/2022-11_AmSam_BMKData/qualtrimmedfastqs
[dbarshis@turing1 qualtrimmedfastqs]$ cat FullPipelineHybridRef.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-29_PlutFullPipeline.txt
#SBATCH -n 16
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=PlutFullPipeline

enable_lmod

module load container_env star
module load container_env samtools

#STAR make genome
STAR --runMode genomeGenerate --runThreadN 16 --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/VoolstraPlutplusC15/ --genomeFastaFiles /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/VoolstraPlutplusC15/VoolstraPlut_C15_hybrid.fasta --genomeChrBinNbits 16

#Fasta index
crun samtools faidx /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/VoolstraPlutplusC15/VoolstraPlut_C15_hybrid.fasta

#STAR alignment
for i in *_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/VoolstraPlutplusC15/ --runThreadN 16 --outSAMattributes All --outSAMattrRGline ID:${i%_TF_R1_val_1.fq.gz} --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2PlutC15`; done

#BAM sort and index
for i in *.out.bam; do `crun samtools sort -O bam -o ${i%.out.bam}_sorted.bam $i`; done
for i in *_sorted.bam; do `crun samtools index $i`; done

#BCFtools mpileup
module load container_env bcftools

crun bcftools mpileup -Ou -f /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/ReferenceTranscriptome/VoolstraPlutplusC15/VoolstraPlut_C15_hybrid.fasta *_sorted.bam | crun bcftools call -Ou -mv | crun bcftools filter -O z -s LowQual -e "QUAL<30 || DP>100" > Plut_2VoolPlutC15_var.flt.vcf.gz

[dbarshis@turing1 qualtrimmedfastqs]$ sbatch FullPipelineHybridRef.sh 
Submitted batch job 9813642
```

  * A hyacinthus and Cladocopium goreaui hybrid

```bash
[dbarshis@turing1 Lopez-NandamSymHybrid]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Lopez-NandamSymHybrid
[dbarshis@turing1 Lopez-NandamSymHybrid]$ cat Ahyacinthus_transcripts_suffixed.fasta SymbC1_goreaui.fasta > Ahyacinthus_symC1_hybrid.fasta
[dbarshis@turing1 qualtrimedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/qualtrimedfastqs
[dbarshis@turing1 qualtrimedfastqs]$ cat FullPipelineAhyacinthus.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-29_AhyaFullPipeline.txt
#SBATCH -n 16
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=AhyaFullPipeline

enable_lmod

module load container_env star
module load container_env samtools

#STAR make genome
STAR --runMode genomeGenerate --runThreadN 16 --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Lopez-NandamSymHybrid/ --genomeFastaFiles /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Lopez-NandamSymHybrid/Ahyacinthus_symC1_hybrid.fasta --genomeChrBinNbits 16

#Fasta index
crun samtools faidx /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Lopez-NandamSymHybrid/Ahyacinthus_symC1_hybrid.fasta

#STAR alignment
for i in *_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Lopez-NandamSymHybrid/ --runThreadN 16 --outSAMattributes All --outSAMattrRGline ID:${i%_TF_R1_val_1.fq.gz} --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2AhyaC1`; done

#BAM sort and index
for i in *.out.bam; do `crun samtools sort -O bam -o ${i%.out.bam}_sorted.bam $i`; done
for i in *_sorted.bam; do `crun samtools index $i`; done

#BCFtools mpileup
module load container_env bcftools

crun bcftools mpileup -Ou -f /cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/ReferenceTranscriptome/Lopez-NandamSymHybrid/Ahyacinthus_symC1_hybrid.fasta *_sorted.bam | crun bcftools call -Ou -mv | crun bcftools filter -O z -s LowQual -e "QUAL<30 || DP>100" > Plut_2LopezNandam_AhyaC1_var.flt.vcf.gz

[dbarshis@turing1 qualtrimedfastqs]$ sbatch FullPipelineAhyacinthus.sh 
Submitted batch job 9813672
```

  * Pver-Smic

```bash
[dbarshis@turing1 Buitrago-Smic]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/ReferenceTranscriptome/Buitrago-Smic
[dbarshis@turing1 Buitrago-Smic]$ cat Pver_transcriptome_v1_suffixed.fasta Smic_longest_sorted_suffixed.fasta > Pver-smic_hybrid.fasta
[dbarshis@turing1 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData/qualtrimmedfastqs
[dbarshis@turing1 qualtrimmedfastqs]$ cat FullPverPipeline.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-29_PverFullPipeline.txt
#SBATCH -n 16
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=PverFullPipeline

enable_lmod

module load container_env star
module load container_env samtools

#STAR make genome
STAR --runMode genomeGenerate --runThreadN 16 --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/ReferenceTranscriptome/Buitrago-Smic/ --genomeFastaFiles /cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/ReferenceTranscriptome/Buitrago-Smic/Pver-smic_hybrid.fasta --genomeChrBinNbits 16

#Fasta index
crun samtools faidx /cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/ReferenceTranscriptome/Buitrago-Smic/Pver-smic_hybrid.fasta

#STAR alignment
for i in *_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/ReferenceTranscriptome/Buitrago-Smic/ --runThreadN 16 --outSAMattributes All --outSAMattrRGline ID:${i%_TF_R1_val_1.fq.gz} --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2PverSmic`; done

#BAM sort and index
for i in *.out.bam; do `crun samtools sort -O bam -o ${i%.out.bam}_sorted.bam $i`; done
for i in *_sorted.bam; do `crun samtools index $i`; done

#BCFtools mpileup
module load container_env bcftools

crun bcftools mpileup -Ou -f /cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/ReferenceTranscriptome/Buitrago-Smic/Pver-smic_hybrid.fasta *_sorted.bam | crun bcftools call -Ou -mv | crun bcftools filter -O z -s LowQual -e "QUAL<30 || DP>100" > Pver_2Buit_PverSmic_var.flt.vcf.gz

[dbarshis@turing1 qualtrimmedfastqs]$ sbatch FullPverPipeline.sh 
Submitted batch job 9813674
```

  * TGing mesoMgris

```bash
[dbarshis@turing1 rawfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/2022-11_AmSam_BMKData/rawfastqs
[dbarshis@turing1 rawfastqs]$ cat TGMesoMgris.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-29_TrimGalore_Mgris.txt
#SBATCH -n 4
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_Mgris

enable_lmod

module load container_env trim_galore

for i in 20220302_Meso_Mgri_*_R1.fq.gz ; do `crun trim_galore --cores 4 --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done
[dbarshis@turing1 rawfastqs]$ sbatch TGMesoMgris.sh 
Submitted batch job 9813676
```

  * Mcap-SymC1Cladocopium goreaui hybrid

```bash
[dbarshis@turing1 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/2022-11_AmSam_BMKData/qualtrimmedfastqs
[dbarshis@turing1 qualtrimmedfastqs]$ cat MgrisFullPipeline.sh 
#!/bin/bash -l

#SBATCH -o 2022-11-29_MgrisrFullPipeline.txt
#SBATCH -n 16
#SBATCH -N 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=MgrisFullPipeline

enable_lmod

module load container_env star
module load container_env samtools

#STAR make genome
STAR --runMode genomeGenerate --runThreadN 16 --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/ReferenceTranscriptome/SteBha-C1Hybrid/ --genomeFastaFiles /cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/ReferenceTranscriptome/SteBha-C1Hybrid/McapSymC1Hybrid.fasta --genomeChrBinNbits 16

#Fasta index
crun samtools faidx /cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/ReferenceTranscriptome/SteBha-C1Hybrid/McapSymC1Hybrid.fasta

#STAR alignment
for i in *_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/ReferenceTranscriptome/SteBha-C1Hybrid/ --runThreadN 16 --outSAMattributes All --outSAMattrRGline ID:${i%_TF_R1_val_1.fq.gz} --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2McapCgor`; done

#BAM sort and index
for i in *.out.bam; do `crun samtools sort -O bam -o ${i%.out.bam}_sorted.bam $i`; done
for i in *_sorted.bam; do `crun samtools index $i`; done

#BCFtools mpileup
module load container_env bcftools

crun bcftools mpileup -Ou -f /cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/ReferenceTranscriptome/SteBha-C1Hybrid/McapSymC1Hybrid.fasta *_sorted.bam | crun bcftools call -Ou -mv | crun bcftools filter -O z -s LowQual -e "QUAL<30 || DP>100" > Mgris_2SteBhat_McapCgor_var.flt.vcf.gz

[dbarshis@turing1 qualtrimmedfastqs]$ sbatch MgrisFullPipeline.sh 
Submitted batch job 9813721
```

## 2022-12-02_vcffiltering

```bash
##Ahya
[dbarshis@coreV2-25-031 qualtrimedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/qualtrimedfastqs
[dbarshis@coreV2-25-031 qualtrimedfastqs]$ enable_lmod

The following have been reloaded with a version change:
  1) slurm/20.11.5 => slurm/21.08

[dbarshis@coreV2-25-031 qualtrimedfastqs]$ module load vcftools

The following have been reloaded with a version change:
  1) sge/2011.11p1 => sge/2011
[dbarshis@coreV2-25-031 qualtrimedfastqs]$ vcftools --gzvcf Ahya_2LopezNandam_AhyaC1_var.flt.vcf.gz

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--gzvcf Ahya_2LopezNandam_AhyaC1_var.flt.vcf.gz

Using zlib version: 1.2.3
Versions of zlib >= 1.2.4 will be *much* faster when reading zipped VCF files.
Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 16 out of 16 Individuals
After filtering, kept 809667 out of a possible 809667 Sites
Run Time = 20.00 seconds
[dbarshis@coreV2-25-031 qualtrimedfastqs]$ vcftools --gzvcf Ahya_2LopezNandam_AhyaC1_var.flt.vcf.gz --max-missing 0.5 --mac 3 --minQ 30 --minDP 10 --max-alleles 2 --maf 0.015 --remove-indels --recode --recode-INFO-all --out 253297_Ahya_2LopezNandam_AhyaC1_HEAFilt

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--gzvcf Ahya_2LopezNandam_AhyaC1_var.flt.vcf.gz
	--recode-INFO-all
	--mac 3
	--maf 0.015
	--max-alleles 2
	--minDP 10
	--minQ 30
	--max-missing 0.5
	--out 253297_Ahya_2LopezNandam_AhyaC1_HEAFilt
	--recode
	--remove-indels

Using zlib version: 1.2.3
Versions of zlib >= 1.2.4 will be *much* faster when reading zipped VCF files.
Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 16 out of 16 Individuals
Outputting VCF file...
After filtering, kept 253297 out of a possible 809667 Sites
Run Time = 30.00 seconds
[dbarshis@coreV2-22-036 qualtrimedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_hyacinthus/2022-11_AmSam_BMKData/qualtrimedfastqs
[dbarshis@coreV2-22-036 qualtrimedfastqs]$ vcftools --vcf 253297_Ahya_2LopezNandam_AhyaC1_HEAFilt.recode.vcf --max-missing 1 --recode --recode-INFO-all --out 90766__Ahya_2LopezNandam_AhyaC1_maxmiss1

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 253297_Ahya_2LopezNandam_AhyaC1_HEAFilt.recode.vcf
	--recode-INFO-all
	--max-missing 1
	--out 90766__Ahya_2LopezNandam_AhyaC1_maxmiss1
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 16 out of 16 Individuals
Outputting VCF file...
After filtering, kept 90766 out of a possible 253297 Sites
Run Time = 8.00 seconds
[dbarshis@coreV2-22-036 qualtrimedfastqs]$ vcftools --vcf 90766__Ahya_2LopezNandam_AhyaC1_maxmiss1.recode.vcf --exclude-positions SymSitesToExclude.txt --recode --recode-INFO-all --out 90757_Ahya_2LopezNandam_AhyaC1_maxmiss1_noSym

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 90766__Ahya_2LopezNandam_AhyaC1_maxmiss1.recode.vcf
	--exclude-positions SymSitesToExclude.txt
	--recode-INFO-all
	--out 90757_Ahya_2LopezNandam_AhyaC1_maxmiss1_noSym
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 16 out of 16 Individuals
Outputting VCF file...
After filtering, kept 90757 out of a possible 90766 Sites
Run Time = 5.00 seconds
(base) danbarshis@BIOLLBB0 Ahya2LopezC1 % pwd
/Users/danbarshis/dansstuff/Research/2021-2024_GlobalSearch/2022-AmSam/Sequencing/2022-11_BMKGenoTypingData/Acropora_hyacinthus/SNPTyping/Ahya2LopezC1
(base) danbarshis@BIOLLBB0 Ahya2LopezC1 % python2 ~/scripts/vcftogenepop_advbioinf.py 90757_Ahya_2LopezNandam_AhyaC1_maxmiss1_noSym.recode.vcf popfile.txt
Indivs with genotypes in vcf file: CBASS_AS_S2_Ahya_2_TF	CBASS_AS_S2_Ahya_5_TF	CBASS_AS_S2_Ahya_6_TF	CBASS_AS_S2_Ahya_7_TF	CBASS_AS_S2_Ahya_9_TF	CBASS_AS_S1_Ahya_10_TF	CBASS_AS_S1_Ahya_13_TF	CBASS_AS_S1_Ahya_14_TF	CBASS_AS_S1_Ahya_1_TF	CBASS_AS_S1_Ahya_2_TF	CBASS_AS_S1_Ahya_3_TF	CBASS_AS_S1_Ahya_4_TF	CBASS_AS_S1_Ahya_5_TF	CBASS_AS_S1_Ahya_6_TF	CBASS_AS_S1_Ahya_7_TF	CBASS_AS_S1_Ahya_9_TF
20 90757 90757 90757 90757 16

### Mgris
[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/2022-11_AmSam_BMKData/qualtrimmedfastqs
[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ vcftools --gzvcf Mgris_2SteBhat_McapCgor_var.flt.vcf.gz

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--gzvcf Mgris_2SteBhat_McapCgor_var.flt.vcf.gz

Using zlib version: 1.2.3
Versions of zlib >= 1.2.4 will be *much* faster when reading zipped VCF files.
Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 34 out of 34 Individuals
After filtering, kept 2204287 out of a possible 2204287 Sites
Run Time = 76.00 seconds
[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ vcftools --gzvcf Mgris_2SteBhat_McapCgor_var.flt.vcf.gz --max-missing 0.5 --mac 3 --minQ 30 --minDP 10 --max-alleles 2 --maf 0.015 --remove-indels --recode --recode-INFO-all --out 436043_Mgris_2SteBhat_McapCgor_HEAFilt

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--gzvcf Mgris_2SteBhat_McapCgor_var.flt.vcf.gz
	--recode-INFO-all
	--mac 3
	--maf 0.015
	--max-alleles 2
	--minDP 10
	--minQ 30
	--max-missing 0.5
	--out 436043_Mgris_2SteBhat_McapCgor_HEAFilt
	--recode
	--remove-indels

Using zlib version: 1.2.3
Versions of zlib >= 1.2.4 will be *much* faster when reading zipped VCF files.
Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 34 out of 34 Individuals
Outputting VCF file...
After filtering, kept 436043 out of a possible 2204287 Sites
Run Time = 109.00 seconds
[dbarshis@coreV2-22-036 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Montipora_grisea/2022-11_AmSam_BMKData/qualtrimmedfastqs
[dbarshis@coreV2-22-036 qualtrimmedfastqs]$ vcftools --vcf 436043_Mgris_2SteBhat_McapCgor_HEAFilt.recode.vcf --max-missing 1 --recode --recode-INFO-all --out 53064_Mgris_2SteBhat_McapCgor_maxmiss1

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 436043_Mgris_2SteBhat_McapCgor_HEAFilt.recode.vcf
	--recode-INFO-all
	--max-missing 1
	--out 53064_Mgris_2SteBhat_McapCgor_maxmiss1
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 34 out of 34 Individuals
Outputting VCF file...
After filtering, kept 53064 out of a possible 436043 Sites
Run Time = 14.00 seconds
[dbarshis@coreV2-22-036 qualtrimmedfastqs]$ vcftools --vcf 53064_Mgris_2SteBhat_McapCgor_maxmiss1.recode.vcf --exclude-positions SymsToRemove.txt --recode --recode-INFO-all --out 52798_Mgris_2SteBhat_McapCgor_maxmiss1_noSyms

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 53064_Mgris_2SteBhat_McapCgor_maxmiss1.recode.vcf
	--exclude-positions SymsToRemove.txt
	--recode-INFO-all
	--out 52798_Mgris_2SteBhat_McapCgor_maxmiss1_noSyms
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 34 out of 34 Individuals
Outputting VCF file...
After filtering, kept 52798 out of a possible 53064 Sites
Run Time = 6.00 seconds
(base) danbarshis@BIOLLBB0 McapCgorHybrid % pwd
/Users/danbarshis/dansstuff/Research/2021-2024_GlobalSearch/2022-AmSam/Sequencing/2022-11_BMKGenoTypingData/Montipora_grisea/SNP_results/McapCgorHybrid
(base) danbarshis@BIOLLBB0 McapCgorHybrid % python2 ~/scripts/vcftogenepop_advbioinf.py 52798_Mgris_2SteBhat_McapCgor_maxmiss1_noSyms.recode.vcf popfile.txt
Indivs with genotypes in vcf file: CBASS_AS_S2_Mgri_10_TF	CBASS_AS_S2_Mgri_11_TF	CBASS_AS_S2_Mgri_12_TF	CBASS_AS_S2_Mgri_13_TF	CBASS_AS_S2_Mgri_14_TF	CBASS_AS_S2_Mgri_1_TF	CBASS_AS_S2_Mgri_2_TF	CBASS_AS_S2_Mgri_3_TF	CBASS_AS_S2_Mgri_4_TF	CBASS_AS_S2_Mgri_5_TF	CBASS_AS_S2_Mgri_6_TF	CBASS_AS_S2_Mgri_7_TF	CBASS_AS_S2_Mgri_8_TF	CBASS_AS_S2_Mgri_9_TF	CBASS_AS_S1_Mgri_10_TF	CBASS_AS_S1_Mgri_11_TF	CBASS_AS_S1_Mgri_12_TF	CBASS_AS_S1_Mgri_1_TF	CBASS_AS_S1_Mgri_2_TF	CBASS_AS_S1_Mgri_3_TF	CBASS_AS_S1_Mgri_4_TF	CBASS_AS_S1_Mgri_5_TF	CBASS_AS_S1_Mgri_6_TF	CBASS_AS_S1_Mgri_7_TF	CBASS_AS_S1_Mgri_8_TF	CBASS_AS_S1_Mgri_9_TF	Meso_Mgri_1_TF	Meso_Mgri_2_TF	Meso_Mgri_3_TF	Meso_Mgri_4_C_T3	Meso_Mgri_5_TF	Meso_Mgri_6_TF	Meso_Mgri_7_TF	Meso_Mgri_8_TF
38 52798 52798 52798 52798 34

## Porites lobata ##
[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/2022-11_AmSam_BMKData/qualtrimmedfastqs
[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ vcftools --gzvcf Plut_2VoolPlutC15_var.flt.vcf.gz 

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--gzvcf Plut_2VoolPlutC15_var.flt.vcf.gz

Using zlib version: 1.2.3
Versions of zlib >= 1.2.4 will be *much* faster when reading zipped VCF files.
Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 19 out of 19 Individuals
After filtering, kept 2636028 out of a possible 2636028 Sites
Run Time = 69.00 seconds
[dbarshis@coreV2-25-031 qualtrimmedfastqs]$ vcftools --gzvcf Plut_2VoolPlutC15_var.flt.vcf.gz --max-missing 0.5 --mac 3 --minQ 30 --minDP 10 --max-alleles 2 --maf 0.015 --remove-indels --recode --recode-INFO-all --out 902201_Plut_2VoolPlutC15_HEAFilt

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--gzvcf Plut_2VoolPlutC15_var.flt.vcf.gz
	--recode-INFO-all
	--mac 3
	--maf 0.015
	--max-alleles 2
	--minDP 10
	--minQ 30
	--max-missing 0.5
	--out 902201_Plut_2VoolPlutC15_HEAFilt
	--recode
	--remove-indels

Using zlib version: 1.2.3
Versions of zlib >= 1.2.4 will be *much* faster when reading zipped VCF files.
Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 19 out of 19 Individuals
Outputting VCF file...
After filtering, kept 902201 out of a possible 2636028 Sites
Run Time = 110.00 seconds
[dbarshis@coreV2-22-036 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_lobata/2022-11_AmSam_BMKData/qualtrimmedfastqs
[dbarshis@coreV2-22-036 qualtrimmedfastqs]$ vcftools --vcf 902201_Plut_2VoolPlutC15_HEAFilt.recode.vcf --max-missing 1 --recode --recode-INFO-all --out 180829_Plut_2VoolPlutC15_maxmiss1

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 902201_Plut_2VoolPlutC15_HEAFilt.recode.vcf
	--recode-INFO-all
	--max-missing 1
	--out 180829_Plut_2VoolPlutC15_maxmiss1
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 19 out of 19 Individuals
Outputting VCF file...
After filtering, kept 180829 out of a possible 902201 Sites
Run Time = 22.00 seconds
[dbarshis@coreV2-22-036 qualtrimmedfastqs]$ vcftools --vcf 180829_Plut_2VoolPlutC15_maxmiss1.recode.vcf --exclude-positions SymsToRemove.txt --recode --recode-INFO-all --out 178972_Plut_2VoolPlutC15_maxmiss1_noSyms

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 180829_Plut_2VoolPlutC15_maxmiss1.recode.vcf
	--exclude-positions SymsToRemove.txt
	--recode-INFO-all
	--out 178972_Plut_2VoolPlutC15_maxmiss1_noSyms
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 19 out of 19 Individuals
Outputting VCF file...
After filtering, kept 178972 out of a possible 180829 Sites
Run Time = 11.00 seconds
(base) danbarshis@BIOLLBB0 VoolPlutC15 % pwd
/Users/danbarshis/dansstuff/Research/2021-2024_GlobalSearch/2022-AmSam/Sequencing/2022-11_BMKGenoTypingData/Porites_lobata/SNP_results/VoolPlutC15
(base) danbarshis@BIOLLBB0 VoolPlutC15 % python2 ~/scripts/vcftogenepop_advbioinf.py 178972_Plut_2VoolPlutC15_maxmiss1_noSyms.recode.vcf popfile.txt 
Indivs with genotypes in vcf file: CBASS_AS_S2_Plob_10	CBASS_AS_S2_Plob_11	CBASS_AS_S2_Plob_12	CBASS_AS_S2_Plob_1	CBASS_AS_S2_Plob_2	CBASS_AS_S2_Plob_4	CBASS_AS_S2_Plob_5	CBASS_AS_S2_Plob_6	CBASS_AS_S2_Plob_7	CBASS_AS_S2_Plob_8	CBASS_AS_S2_Plob_9	CBASS_AS_S1_Plob_10	CBASS_AS_S1_Plob_11	CBASS_AS_S1_Plob_12	CBASS_AS_S1_Plob_4	CBASS_AS_S1_Plob_5	CBASS_AS_S1_Plob_6	CBASS_AS_S1_Plob_7	CBASS_AS_S1_Plob_9
23 178972 178972 178972 178972 19

## Pocillopora verrucosa ##
[dbarshis@coreV2-22-036 qualtrimmedfastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Pocillopora_verrucosa/2022-11_AmSamBMKData/qualtrimmedfastqs
[dbarshis@coreV2-22-036 qualtrimmedfastqs]$ vcftools --gzvcf Pver_2Buit_PverSmic_var.flt.vcf.gz --max-missing 0.5 --mac 3 --minQ 30 --minDP 10 --max-alleles 2 --maf 0.015 --remove-indels  --recode --recode-INFO-all --out 941689_Pver_2Buit_PverSmic_HEAFilt

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--gzvcf Pver_2Buit_PverSmic_var.flt.vcf.gz
	--recode-INFO-all
	--mac 3
	--maf 0.015
	--max-alleles 2
	--minDP 10
	--minQ 30
	--max-missing 0.5
	--out 941689_Pver_2Buit_PverSmic_HEAFilt
	--recode
	--remove-indels

Using zlib version: 1.2.3
Versions of zlib >= 1.2.4 will be *much* faster when reading zipped VCF files.
Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 28 out of 28 Individuals
Outputting VCF file...
After filtering, kept 941689 out of a possible 2230723 Sites
Run Time = 166.00 seconds
[dbarshis@coreV2-22-036 qualtrimmedfastqs]$ vcftools --vcf 941689_Pver_2Buit_PverSmic_HEAFilt.recode.vcf --max-missing 1 --recode --recode-INFO-all --out 413012_Pver_2Buit_PverSmic_maxmiss1

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf 941689_Pver_2Buit_PverSmic_HEAFilt.recode.vcf
	--recode-INFO-all
	--max-missing 1
	--out 413012_Pver_2Buit_PverSmic_maxmiss1
	--recode

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 28 out of 28 Individuals
Outputting VCF file...
After filtering, kept 413012 out of a possible 941689 Sites
Run Time = 42.00 seconds
(base) danbarshis@BIOLLBB0 2BuitVolSmic % pwd
/Users/danbarshis/dansstuff/Research/2021-2024_GlobalSearch/2022-AmSam/Sequencing/2022-11_BMKGenoTypingData/Pocillopora_verrucosa/SNP_results/2BuitVolSmic
(base) danbarshis@BIOLLBB0 2BuitVolSmic % python2 ~/scripts/vcftogenepop_advbioinf.py 413012_Pver_2Buit_PverSmic_maxmiss1_noSym.recode.vcf popfile.txt 
Indivs with genotypes in vcf file: CBASS_AS_S2_Pver_10_TF	CBASS_AS_S2_Pver_11_TF	CBASS_AS_S2_Pver_12_TF	CBASS_AS_S2_Pver_13_TF	CBASS_AS_S2_Pver_14_TF	CBASS_AS_S2_Pver_1_TF	CBASS_AS_S2_Pver_2_TF	CBASS_AS_S2_Pver_3_TF	CBASS_AS_S2_Pver_4_TF	CBASS_AS_S2_Pver_5_TF	CBASS_AS_S2_Pver_6_TF	CBASS_AS_S2_Pver_7_TF	CBASS_AS_S2_Pver_8_TF	CBASS_AS_S2_Pver_9_TF	CBASS_AS_S1_Pver_10_TF	CBASS_AS_S1_Pver_11_TF	CBASS_AS_S1_Pver_12_TF	CBASS_AS_S1_Pver_13_TF	CBASS_AS_S1_Pver_14_TF	CBASS_AS_S1_Pver_1_TF	CBASS_AS_S1_Pver_2_TF	CBASS_AS_S1_Pver_3_TF	CBASS_AS_S1_Pver_4_TF	CBASS_AS_S1_Pver_5_TF	CBASS_AS_S1_Pver_6_TF	CBASS_AS_S1_Pver_7_TF	CBASS_AS_S1_Pver_8_TF	CBASS_AS_S1_Pver_9_TF
32 413007 413007 413007 413007 28
```



