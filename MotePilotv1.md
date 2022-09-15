Dan's Lab Notebook
================
## 2022-01-05_Unzipping Mote Pilot v1 125 sample data
  * unzipping raw data

```bash
[dbarshis@turing1 2021-12_MotePilotV1]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_cervicornis/2021-12_MotePilotV1
[dbarshis@turing1 2021-12_MotePilotV1]$ cat Unzip.sh 
#!/bin/bash -l

#SBATCH -o 2021-01-05_UnZip.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=UnZip

unzip X204SC21081158-Z01-F003.zip

[dbarshis@turing1 2021-12_MotePilotV1]$ sbatch Unzip.sh 
Submitted batch job 9675766
Slurm Job_id=9675766 Name=UnZip Ended, Run time 02:44:34, COMPLETED, ExitCode 0
```
  * md5 check summing to confirm integrity of the raw .zip file after download

```bash
[dbarshis@coreV2-22-032 2021-12_MotePilotV1]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Acropora_cervicornis/2021-12_MotePilotV1
[dbarshis@coreV2-22-032 2021-12_MotePilotV1]$ cat ChekSum.sh 
#!/bin/bash -l

#SBATCH -o 2021-01-05_md5.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=md5

md5sum X204SC21081158-Z01-F003.zip

[dbarshis@coreV2-22-032 2021-12_MotePilotV1]$ sbatch ChekSum.sh 
Submitted batch job 9675788
[dbarshis@coreV2-22-032 2021-12_MotePilotV1]$ cat MD5.txt
cac5b1b35f5f772668b3a6a6fa2aac3d  X204SC21081158-Z01-F003.zip
[dbarshis@coreV2-22-032 2021-12_MotePilotV1]$ cat 2021-01-05_md5.txt
cac5b1b35f5f772668b3a6a6fa2aac3d  X204SC21081158-Z01-F003.zip
Looks good!
```
## 2022-01-12_Downloading Mote Pilot v1 270 others sample data
  * Wget didn't work, trying labmac browser to RC drive
  
## 2022-01-15_Download finished, cp'd to /cm/share, now checksumming
```bash
[dbarshis@turing1 2021-12_MotePilotV1]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1
[dbarshis@turing1 2021-12_MotePilotV1]$ cat ChekSum.sh 
#!/bin/bash -l

#SBATCH -o 2021-01-15_md5.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=md5

md5sum X204SC21081158-Z01-F002.zip

[dbarshis@turing1 2021-12_MotePilotV1]$ sbatch ChekSum.sh 
Submitted batch job 9688134
### looks good
[dbarshis@turing1 2021-12_MotePilotV1]$ ls
[dbarshis@turing1 2021-12_MotePilotV1]$ cp /RC/group/rc_barshis_lab/taxonarchive/Porites_astreoides/
md5.txt*                     X204SC21081158-Z01-F002.zip*
[dbarshis@turing1 2021-12_MotePilotV1]$ cp /RC/group/rc_barshis_lab/taxonarchive/Porites_astreoides/X204SC21081158-Z01-F002.zip ./

```
  * Unzipping raw data
  
```bash
[dbarshis@turing1 2021-12_MotePilotV1]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1
[dbarshis@turing1 2021-12_MotePilotV1]$ cat Unzip.sh 
#!/bin/bash -l

#SBATCH -o 2021-01-05_UnZip.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=UnZip

unzip X204SC21081158-Z01-F002.zip
[dbarshis@turing1 2021-12_MotePilotV1]$ sbatch Unzip.sh 
Submitted batch job 9688145
```


## 2022-02-01_Testing TrimGalore

  * generating trimGalore command

```bash
[dbarshis@coreV2-22-031 test_w_fastqc]$ pwd
/cm/shared/courses/dbarshis/barshislab/KatieP/taxons/Porites_astreoides/2021-12_MotePilotV1/raw_data_fastqs/djbtest/test_w_fastqc
enable_lmod

module load container_env trim_galore

crun trim_galore --fastqc --paired R1FILE.fq.gz R2FILE.fq.gz
```

## 2022-02-02_TrimGaloring
```bash
dbarshis@coreV2-22-029 Acer]$ pwd
/cm/shared/courses/dbarshis/barshislab/KatieP/taxons/Porites_astreoides/2021-12_MotePilotV1/raw_data_fastqs/Acer

####CM5####
[dbarshis@coreV2-22-029 Acer]$ cat TrimGalore_CM5.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-02_TrimGalore_CM5.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_CM5

enable_lmod

module load container_env trim_galore

for i in 20210611T1400_CBASS_US_MoteOffN_Acer_CM5*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-029 Acer]$ sbatch TrimGalore_CM5.sh 

####7####
[dbarshis@coreV2-22-029 Acer]$ cat TrimGalore_7.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-02_TrimGalore_7.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_7

enable_lmod

module load container_env trim_galore

for i in 20210611T1400_CBASS_US_MoteOffN_Acer_7_*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-029 Acer]$ sbatch TrimGalore_7.sh 
Submitted batch job 9693797

#####62####
[dbarshis@coreV2-22-029 Acer]$ cat TrimGalore_62.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-02_TrimGalore_62.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_62

enable_lmod

module load container_env trim_galore

for i in 20210611T1400_CBASS_US_MoteOffN_Acer_62_*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-029 Acer]$ sbatch TrimGalore_62.sh 
Submitted batch job 9693798

####50####
[dbarshis@coreV2-22-029 Acer]$ cat TrimGalore_50.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-02_TrimGalore_50.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_50

enable_lmod

module load container_env trim_galore

for i in 20210611T1400_CBASS_US_MoteOffN_Acer_50_*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-029 Acer]$ sbatch TrimGalore_50.sh 
Submitted batch job 9693799

####48####
[dbarshis@coreV2-22-029 Acer]$ cat TrimGalore_48.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-02_TrimGalore_48.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_48

enable_lmod

module load container_env trim_galore

for i in 20210611T1400_CBASS_US_MoteOffN_Acer_48_*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-029 Acer]$ sbatch TrimGalore_48.sh 
Submitted batch job 9693800

####41####
[dbarshis@coreV2-22-029 Acer]$ cat TrimGalore_41.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-02_TrimGalore_41.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_41

enable_lmod

module load container_env trim_galore

for i in 20210611T1400_CBASS_US_MoteOffN_Acer_41_*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-029 Acer]$ sbatch TrimGalore_41.sh 
Submitted batch job 9693801

####34####
[dbarshis@coreV2-22-029 Acer]$ cat TrimGalore_34.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-02_TrimGalore_34.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_34

enable_lmod

module load container_env trim_galore

for i in 20210611T1400_CBASS_US_MoteOffN_Acer_34_*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-029 Acer]$ sbatch TrimGalore_34.sh 
Submitted batch job 9693802

####31####
[dbarshis@coreV2-22-029 Acer]$ cat TrimGalore_31.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-02_TrimGalore_34.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_34

enable_lmod

module load container_env trim_galore

for i in 20210611T1400_CBASS_US_MoteOffN_Acer_31_*_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@coreV2-22-029 Acer]$ sbatch TrimGalore_31.sh 
Submitted batch job 9693803
```


```bash
[dbarshis@turing1 Acer]$ pwd
/cm/shared/courses/dbarshis/barshislab/KatieP/taxons/Porites_astreoides/2021-12_MotePilotV1/raw_data_fastqs/Acer
[dbarshis@turing1 Acer]$ cat TarAcer.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-03_AcerTar.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=AcerTar


tar -czvf MoreAcerData.tar.gz *_val*.fq.gz
[dbarshis@turing1 Acer]$ sbatch TarAcer.sh
Submitted batch job 9693977

[dbarshis@turing1 Acer]$ pwd
/cm/shared/courses/dbarshis/barshislab/KatieP/taxons/Porites_astreoides/2021-12_MotePilotV1/raw_data_fastqs/Acer
[dbarshis@turing1 Acer]$ cat TarAcer_raw.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-03_AcerTar_raw.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=AcerTar


tar -czvf MoreAcerData_raw.tar.gz *_R[12].fq.gz
[dbarshis@turing1 Acer]$ sbatch TarAcer_raw.sh 
Submitted batch job 9693986
```

## 2022-02-07_TrimGaloring
```bash
[dbarshis@turing1 raw_data_fastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/KatieP/taxons/Porites_astreoides/2021-12_MotePilotV1/raw_data_fastqs
### Past Off 1
[dbarshis@turing1 raw_data_fastqs]$ cat TrimGalore_Off_1.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-07_TrimGalore_Off_4-5.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_Off4-5

enable_lmod

module load container_env trim_galore

for i in 20210610T1400_CBASS_US_MoteOff_Past_1_*_RNASeq_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done
[dbarshis@turing1 raw_data_fastqs]$ sbatch TrimGalore_Off_1.sh 
Submitted batch job 9694816
Slurm Job_id=9694816 Name=TG_Off4-5 Ended, Run time 21:21:22, FAILED, ExitCode 127

### Past Off 2-3
[dbarshis@turing1 raw_data_fastqs]$ cat TrimGalore_Off_2-3.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-07_TrimGalore_Off_4-5.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_Off4-5

enable_lmod

module load container_env trim_galore

for i in 20210610T1400_CBASS_US_MoteOff_Past_[23]_*_RNASeq_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done
[dbarshis@turing1 raw_data_fastqs]$ sbatch TrimGalore_Off_2-3.sh 
Submitted batch job 9694815
Slurm Job_id=9694815 Name=TG_Off4-5 Ended, Run time 1-18:21:55, FAILED, ExitCode 127

### Past Off 4-5
[dbarshis@turing1 raw_data_fastqs]$ cat TrimGalore_Off_4-5.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-07_TrimGalore_Off_4-5.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_Off4-5

enable_lmod

module load container_env trim_galore

for i in 20210610T1400_CBASS_US_MoteOff_Past_[45]_*_RNASeq_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done
[dbarshis@turing1 raw_data_fastqs]$ sbatch TrimGalore_Off_4-5.sh 
Submitted batch job 9694814
Slurm Job_id=9694814 Name=TG_Off4-5 Ended, Run time 1-20:04:38, FAILED, ExitCode 127

### Past In 6-7
[dbarshis@turing1 raw_data_fastqs]$ cat TrimGalore_In_6-7.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-07_TrimGalore_In_6-7.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_In_6-7

enable_lmod

module load container_env trim_galore

for i in 20210608T1400_CBASS_US_MoteIn_Past_[67]_*_RNASeq_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done
[dbarshis@turing1 raw_data_fastqs]$ sbatch TrimGalore_In_6-7.sh 
Submitted batch job 9694817
Slurm Job_id=9694817 Name=TG_In_6-7 Ended, Run time 1-21:42:12, FAILED, ExitCode 127

### Past In 8-9
[dbarshis@turing1 raw_data_fastqs]$ cat TrimGalore_In_8-9.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-07_TrimGalore_In_8-9.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_In_8-9

enable_lmod

module load container_env trim_galore

for i in 20210608T1400_CBASS_US_MoteIn_Past_[89]_*_RNASeq_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done
[dbarshis@turing1 raw_data_fastqs]$ sbatch TrimGalore_In_8-9.sh 
Submitted batch job 9694818
Slurm Job_id=9694818 Name=TG_In_8-9 Ended, Run time 1-21:16:43, FAILED, ExitCode 127

### Past In 10
[dbarshis@turing1 raw_data_fastqs]$ cat TrimGalore_In_10.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-07_TrimGalore_In_10.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TG_In_10

enable_lmod

module load container_env trim_galore

for i in 20210608T1400_CBASS_US_MoteIn_Past_10_*_RNASeq_R1.fq.gz ; do `crun trim_galore --fastqc --paired $i ${i%_R1.fq.gz}_R2.fq.gz`; done

[dbarshis@turing1 raw_data_fastqs]$ sbatch TrimGalore_In_10.sh 
Submitted batch job 9694819
Slurm Job_id=9694819 Name=TG_In_10 Ended, Run time 23:15:33, FAILED, ExitCode 127
```

  * formatting Past transcriptomes for STAR

```bash
###PutnamPast
[dbarshis@coreV2-25-024 putnamPast]$ pwd
/cm/shared/courses/dbarshis/barshislab/KatieP/taxons/Porites_astreoides/2021-12_MotePilotV1/raw_data_fastqs/mapping/putnamPast
[dbarshis@coreV2-25-024 putnamPast]$ cat MakeGenome.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-07_PutnamgenomeGenerate.txt
#SBATCH -n 4
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=genomegenerate

enable_lmod

module load container_env star

STAR --runMode genomeGenerate --runThreadN 4 --genomeDir ./ --genomeFastaFiles Pastreoides_transcripts_v1_suffixed.fasta --genomeChrBinNbits 16
[dbarshis@coreV2-25-024 putnamPast]$ sbatch MakeGenome.sh 
Submitted batch job 9694821

Slurm Job_id=9694821 Name=genomegenerate Ended, Run time 00:03:31, COMPLETED, ExitCode 0

####KenkelPast
[dbarshis@coreV2-25-024 kenkelPast]$ pwd
/cm/shared/courses/dbarshis/barshislab/KatieP/taxons/Porites_astreoides/2021-12_MotePilotV1/raw_data_fastqs/mapping/kenkelPast
[dbarshis@coreV2-25-024 kenkelPast]$ cat MakeGenomeKenk.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-07_KenkelgenomeGenerate.txt
#SBATCH -n 4
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=Kenkgenomegenerate

enable_lmod

module load container_env star

STAR --runMode genomeGenerate --runThreadN 4 --genomeDir ./ --genomeFastaFiles Porites_astreoides_LongestIsoform_suffixed.fasta --genomeChrBinNbits 16

[dbarshis@coreV2-25-024 kenkelPast]$ sbatch MakeGenomeKenk.sh 
Submitted batch job 9694822

Slurm Job_id=9694822 Name=Kenkgenomegenerate Ended, Run time 00:01:16, COMPLETED, ExitCode 0
```

## 2022-02-09_Running MultiQC and STAR

```bash
[dbarshis@coreV2-25-024 Acer]$ pwd
/cm/shared/courses/dbarshis/barshislab/KatieP/taxons/Porites_astreoides/2021-12_MotePilotV1/raw_data_fastqs/Acer
[dbarshis@coreV2-25-024 Acer]$ module load container_env multiqc
[dbarshis@coreV2-25-024 Acer]$ crun multiqc ./
[WARNING]         multiqc : MultiQC Version v1.12 now available!
[INFO   ]         multiqc : This is MultiQC v1.9
[INFO   ]         multiqc : Template    : default
[INFO   ]         multiqc : Searching   : /cm/shared/courses/dbarshis/barshislab/KatieP/taxons/Porites_astreoides/2021-12_MotePilotV1/raw_data_fastqs/Acer
Searching 531 files..  [####################################]  100%          
[INFO   ]        cutadapt : Found 102 reports
[INFO   ]          fastqc : Found 102 reports
[INFO   ]         multiqc : Compressing plot data
[INFO   ]         multiqc : Report      : multiqc_report.html
[INFO   ]         multiqc : Data        : multiqc_data
[INFO   ]         multiqc : MultiQC complete

[dbarshis@coreV2-25-024 Fastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs
```

  * now running STAR
  * Past Inshore 10

```bash
[dbarshis@turing1 QCFastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs/QCFastqs
[dbarshis@turing1 QCFastqs]$ cat StarMap2Kenkel_10.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-09_STARMapping.txt
#SBATCH -n 4
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STAR10Kenk

enable_lmod

module load container_env star

for i in 20210608T1400_CBASS_US_MoteIn_Past_10_*_RNASeq_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs/mapping/kenkelPast/ --runThreadN 16 --outSAMattributes All --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2kenkel`; done
[dbarshis@turing1 QCFastqs]$ sbatch StarMap2Kenkel_10.sh 
Submitted batch job 9695166

Slurm Job_id=9695166 Name=STAR10Kenk Ended, Run time 08:44:30, FAILED, ExitCode 127
```

  * Star Map 10 2 Putnam

```bash
[dbarshis@turing1 QCFastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs/QCFastqs
[dbarshis@turing1 QCFastqs]$ cat StarMap2Putnam_10.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-09_STARMapping10Putnam.txt
#SBATCH -n 4
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STAR10Putnam

enable_lmod

module load container_env star

for i in 20210608T1400_CBASS_US_MoteIn_Past_10_*_RNASeq_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs/mapping/putnamPast/ --runThreadN 16 --outSAMattributes All --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2putnam`; done

[dbarshis@turing1 QCFastqs]$ sbatch StarMap2Putnam_10.sh 
Submitted batch job 9695167

Slurm Job_id=9695167 Name=STAR10Putnam Ended, Run time 13:34:30, FAILED, ExitCode 127
```

  * StarMap 01 to kenkel

```bash
[dbarshis@turing1 QCFastqs]$ cat StarMap2Kenkel_01.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-09_STARMapping01_Kenkel.txt
#SBATCH -n 4
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STAR01Kenkel

enable_lmod

module load container_env star

for i in 20210610T1400_CBASS_US_MoteOff_Past_1_*_RNASeq_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs/mapping/kenkelPast/ --runThreadN 16 --outSAMattributes All --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2kenkel`; done

[dbarshis@turing1 QCFastqs]$ sbatch StarMap2Kenkel_01.sh 
Submitted batch job 9695169

Slurm Job_id=9695169 Name=STAR01Kenkel Ended, Run time 08:59:39, FAILED, ExitCode 127
```

  * StarMap 01 to putnam

```bash
[dbarshis@turing1 QCFastqs]$ cat StarMap2Putnam_01.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-09_STARMapping01Putnam.txt
#SBATCH -n 4
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STAR01Putnam

enable_lmod

module load container_env star

for i in 20210610T1400_CBASS_US_MoteOff_Past_1_*_RNASeq_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs/mapping/putnamPast/ --runThreadN 16 --outSAMattributes All --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2putnam`; done

[dbarshis@turing1 QCFastqs]$ sbatch StarMap2Putnam_01.sh 
Submitted batch job 9695168

Slurm Job_id=9695168 Name=STAR01Putnam Ended, Run time 13:26:30, FAILED, ExitCode 127
```

  * StarMap 02 to kenkel

```bash
[dbarshis@turing1 QCFastqs]$ cat StarMap2Kenkel_02.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-09_STARMapping02_Kenkel.txt
#SBATCH -n 4
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STAR02Kenkel

enable_lmod

module load container_env star

for i in 20210610T1400_CBASS_US_MoteOff_Past_2_*_RNASeq_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs/mapping/kenkelPast/ --runThreadN 16 --outSAMattributes All --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2kenkel`; done

[dbarshis@turing1 QCFastqs]$ sbatch StarMap2Kenkel_02.sh 
Submitted batch job 9695170

Slurm Job_id=9695170 Name=STAR02Kenkel Ended, Run time 09:20:36, FAILED, ExitCode 127
```

  * StarMap 01 to putnam

```bash
[dbarshis@turing1 QCFastqs]$ cat StarMap2Putnam_02.sh 
[dbarshis@turing1 QCFastqs]$ cat StarMap2Putnam_02.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-09_STARMapping02Putnam.txt
#SBATCH -n 16
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STAR01Putnam

enable_lmod

module load container_env star

for i in 20210610T1400_CBASS_US_MoteOff_Past_2_*_RNASeq_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs/mapping/putnamPast/ --runThreadN 16 --outSAMattributes All --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2putnam`; done

[dbarshis@turing1 QCFastqs]$ sbatch StarMap2Putnam_02.sh
Submitted batch job 9695171

Slurm Job_id=9695171 Name=STAR01Putnam Ended, Run time 06:23:55, FAILED, ExitCode 127
```

  * Past Inshore 10

```bash
[dbarshis@turing1 QCFastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs/QCFastqs
[dbarshis@turing1 QCFastqs]$ cat StarMap2Kenkel_09.sh 
[dbarshis@turing1 QCFastqs]$ cat StarMap2Kenkel_09.sh 
#!/bin/bash -l

#SBATCH -o 2022-02-09_STARMapping09Kenkel.txt
#SBATCH -n 4
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STAR09Kenk

enable_lmod

module load container_env star

for i in 20210608T1400_CBASS_US_MoteIn_Past_9_*_RNASeq_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs/mapping/kenkelPast/ --runThreadN 16 --outSAMattributes All --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2kenkel`; done

[dbarshis@turing1 QCFastqs]$ sbatch StarMap2Kenkel_09.sh 
Submitted batch job 9695173

Slurm Job_id=9695173 Name=STAR09Kenk Ended, Run time 08:41:24, FAILED, ExitCode 127
```

  * Star Map 09 2 Putnam

```bash
[dbarshis@turing1 QCFastqs]$ cat StarMap2Putnam_09.sh
#!/bin/bash -l

#SBATCH -o 2022-02-09_STARMapping09Putnam.txt
#SBATCH -n 4
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=STAR09Putnam

enable_lmod

module load container_env star

for i in 20210608T1400_CBASS_US_MoteIn_Past_9_*_RNASeq_R1_val_1.fq.gz ; do `STAR --genomeDir /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs/mapping/putnamPast/ --runThreadN 16 --outSAMattributes All --genomeLoad LoadAndRemove --outFilterType Normal  --outFilterMismatchNoverLmax 0.03 --outSAMstrandField intronMotif --outFilterIntronMotifs RemoveNoncanonical --outSAMtype BAM Unsorted --limitBAMsortRAM 5784458574 --readFilesCommand zcat --outReadsUnmapped Fastx --outFilterMatchNminOverLread 0.2 --outFilterScoreMinOverLread 0.2 --readFilesIn $i ${i%_R1_val_1.fq.gz}_R2_val_2.fq.gz --outFileNamePrefix ${i%_R1_val_1.fq.gz}_2putnam`; done

[dbarshis@turing1 QCFastqs]$ sbatch StarMap2Putnam_09.sh
Submitted batch job 9695172

Slurm Job_id=9695172 Name=STAR09Putnam Ended, Run time 14:39:27, FAILED, ExitCode 127
```

## Running multiqc on trimgalore and STAR output

```bash
[dbarshis@coreV2-25-024 Fastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs
[dbarshis@coreV2-25-024 Fastqs]$ enable_lmod

The following have been reloaded with a version change:
  1) slurm/20.11.5 => slurm/20.11

[dbarshis@coreV2-25-024 Fastqs]$ module load container_env multiqc

The following have been reloaded with a version change:
  1) sge/2011.11p1 => sge/2011

[dbarshis@coreV2-25-024 Fastqs]$ crun multiqc ./
[WARNING]         multiqc : MultiQC Version v1.12 now available!
[INFO   ]         multiqc : This is MultiQC v1.9
[INFO   ]         multiqc : Template    : default
[INFO   ]         multiqc : Searching   : /cm/shared/courses/dbarshis/barshislab/danb/taxons/Porites_astreoides/2021-12_MotePilotV1/Fastqs
```
* test update