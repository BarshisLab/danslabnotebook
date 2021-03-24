Dan's Lab Notebook
================
## 2021-03-15_Siderastrea and Porites astreoides BlastAnnotation
  * downloading new databases

```
####Sprot
[dbarshis@coreV2-22-007 databases]$ pwd
/cm/shared/apps/blast/databases
[dbarshis@coreV2-22-007 databases]$ wget https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_sprot.fasta.gz
--2021-03-15 23:21:23--  https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_sprot.fasta.gz
Resolving ftp.uniprot.org... 141.161.180.197
Connecting to ftp.uniprot.org|141.161.180.197|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 89862102 (86M) [application/x-gzip]
Saving to: “uniprot_sprot.fasta.gz”

100%[=====================================================================================>] 89,862,102  18.8M/s   in 4.7s    

2021-03-15 23:21:29 (18.4 MB/s) - “uniprot_sprot.fasta.gz” saved [89862102/89862102]

[dbarshis@coreV2-22-007 databases]$ ls -alh uniprot_sprot.fasta.gz 
-rw-r--r-- 1 dbarshis users 86M Feb 10 10:00 uniprot_sprot.fasta.gz
[dbarshis@coreV2-22-007 databases]$ mv uniprot_sprot.fasta.gz uniprot_sprot_Mar2021.fasta.gz
[dbarshis@coreV2-22-007 databases]$ gunzip -c uniprot_sprot_Mar2021.fasta.gz > uniprot_sprot_Mar2021.fasta
[dbarshis@coreV2-22-007 databases]$ ls -alh uniprot_sprot_Mar2021.fasta*
-rwxrwxrwx 1 dbarshis users 267M Mar 15 23:26 uniprot_sprot_Mar2021.fasta
-rw-r--r-- 1 dbarshis users  86M Feb 10 10:00 uniprot_sprot_Mar2021.fasta.gz

####TrEMBL
[dbarshis@coreV2-22-007 databases]$ wget https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_trembl.fasta.gz
--2021-03-15 23:27:54--  https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_trembl.fasta.gz
Resolving ftp.uniprot.org... 141.161.180.197
Connecting to ftp.uniprot.org|141.161.180.197|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 49680333407 (46G) [application/x-gzip]
Saving to: “uniprot_trembl.fasta.gz”

100%[==================================================================================>] 49,680,333,407 18.3M/s   in 44m 25s 

2021-03-16 00:12:19 (17.8 MB/s) - “uniprot_trembl.fasta.gz” saved [49680333407/49680333407]
[dbarshis@coreV2-22-007 databases]$ mv uniprot_trembl.fasta.gz uniprot_trembl_Mar2021.fasta.gz
[dbarshis@coreV2-22-007 databases]$ cat djbUnzipTrembl.sh 
#!/bin/bash -l

#SBATCH -o TrEMBLUnzip.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=TrEMBLUnzip

gunzip -c uniprot_trembl_Mar2021.fasta.gz > uniprot_trembl_Mar2021.fasta
[dbarshis@coreV2-22-007 databases]$ sbatch djbUnzipTrembl.sh 
Submitted batch job 9287069

####nr
#testing
[dbarshis@coreV3-23-035 databases]$ curl "https://ftp.ncbi.nlm.nih.gov/blast/db/nr.0[0-2].tar.gz" -o "nr.0#1.tar.gz"
#full-on
[dbarshis@coreV3-23-035 databases]$ pwd
/cm/shared/apps/blast/databases
[dbarshis@coreV3-23-035 databases]$ cat djbNRdownload.sh 
#!/bin/bash -l

#SBATCH -o NRDownload.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=NRDownload

curl "https://ftp.ncbi.nlm.nih.gov/blast/db/nr.[0-4][0-9].tar.gz" -o "nr.#1#2.tar.gz"
[dbarshis@coreV3-23-035 databases]$ sbatch djbNRdownload.sh 
```

  * making new databases
  
```
###Sprot
[dbarshis@coreV3-23-035 databases]$ pwd
/cm/shared/apps/blast/databases
[dbarshis@coreV3-23-035 databases]$ enable_lmod

The following have been reloaded with a version change:
  1) slurm/20.02.2 => slurm/20.02
[dbarshis@coreV3-23-035 databases]$ module load blast

[dbarshis@coreV3-23-035 databases]$ makeblastdb -in uniprot_sprot_Mar2021.fasta -dbtype prot -title uniprot_sprot_Mar2021 -out uniprot_sprot_Mar2021


Building a new DB, current time: 03/16/2021 00:12:43
New DB name:   /cm/shared/apps/blast/databases/uniprot_sprot_Mar2021
New DB title:  uniprot_sprot_Mar2021
Sequence type: Protein
Keep MBits: T
Maximum file size: 1000000000B
Adding sequences from FASTA; added 564277 sequences in 26.6943 seconds.
```

##2021-03-23_Continuing database building
  * untar-ing nr
  
```
[dbarshis@coreV2-22-007 nr_Mar2021]$ pwd
/cm/shared/apps/blast/databases/nr_Mar2021
[dbarshis@coreV2-22-007 nr_Mar2021]$ cat nrUntar.sh 
#!/bin/bash -l

#SBATCH -o NRUntar.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=NRUnTar

for i in *.tar.gz; do tar -zxvf $i ; done
[dbarshis@coreV2-22-007 nr_Mar2021]$ sbatch nrUntar.sh 
Submitted batch job 9288616
```
  * making TrEMBL

```
[dbarshis@coreV2-22-007 databases]$ pwd
/cm/shared/apps/blast/databases
[dbarshis@coreV2-22-007 databases]$ cat djbMakeTrembl.sh 
#!/bin/bash -l

#SBATCH -o MakeTrembl.txt
#SBATCH -n 1
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=MakeTrembl

enable_lmod
module load blast

makeblastdb -in uniprot_trembl_Mar2021.fasta -dbtype prot -title uniprot_trembl_Mar2021 -out uniprot_trembl_Mar2021
[dbarshis@coreV2-22-007 databases]$ sbatch djbMakeTrembl.sh 
Submitted batch job 9288618
```
  * making submission scripts

```
[dbarshis@coreV4-21-006 TotalAnnotation]$ pwd
/cm/shared/courses/dbarshis/barshislab/VRad/taxons/Porites_astreoides/Paytan/P_ast/refassembly/hybridref/TotalAnnotation
[dbarshis@coreV4-21-006 TotalAnnotation]$ ~/scripts/splitfasta_cluster_batchblast2dbs_blastx_SLURM.py hybridreference.fasta 1500 dbparams_djb.txt
```
  * submitting sprot jobs
  
```
[dbarshis@coreV4-21-006 TotalAnnotation]$ pwd
/cm/shared/courses/dbarshis/barshislab/VRad/taxons/Porites_astreoides/Paytan/P_ast/refassembly/hybridref/TotalAnnotation
[dbarshis@coreV4-21-006 TotalAnnotation]$ for i in ./submissionscripts/qsubsbatch_4/*.sh; do sbatch $i ; done
Submitted batch job 9288639
Submitted batch job 9288640
Submitted batch job 9288641
Submitted batch job 9288642
Submitted batch job 9288643
Submitted batch job 9288644
Submitted batch job 9288645
Submitted batch job 9288646
Submitted batch job 9288647
Submitted batch job 9288648
Submitted batch job 9288649
Submitted batch job 9288650
Submitted batch job 9288651
Submitted batch job 9288652
Submitted batch job 9288653
Submitted batch job 9288654
Submitted batch job 9288655
Submitted batch job 9288656
Submitted batch job 9288657
Submitted batch job 9288658
Submitted batch job 9288659
Submitted batch job 9288660
Submitted batch job 9288661
Submitted batch job 9288662
Submitted batch job 9288663
Submitted batch job 9288664
Submitted batch job 9288665
Submitted batch job 9288666
Submitted batch job 9288667
Submitted batch job 9288668
Submitted batch job 9288669
[dbarshis@coreV4-21-006 TotalAnnotation]$ for i in submissionscripts/qsubsbatch_5/*.sh ; do sbatch $i ; done
Submitted batch job 9288670
Submitted batch job 9288671
Submitted batch job 9288672
Submitted batch job 9288673
Submitted batch job 9288674
Submitted batch job 9288675
Submitted batch job 9288676
Submitted batch job 9288677
Submitted batch job 9288678
Submitted batch job 9288679
Submitted batch job 9288680
Submitted batch job 9288681
Submitted batch job 9288682
Submitted batch job 9288683
Submitted batch job 9288684
Submitted batch job 9288685
Submitted batch job 9288686
Submitted batch job 9288687
```