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

## 2021-03-23_Continuing database building
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

## 2021-03-30 Submitting nr and TrEMBL blasts for Past

```
[dbarshis@turing1 TotalAnnotation]$ pwd
/cm/shared/courses/dbarshis/barshislab/VRad/taxons/Porites_astreoides/Paytan/P_ast/refassembly/hybridref/TotalAnnotation
[dbarshis@turing1 TotalAnnotation]$ for i in ./submissionscripts/qsubsbatch_1/*.sh; do sbatch $i ; done
Submitted batch job 9295238
Submitted batch job 9295239
Submitted batch job 9295240
Submitted batch job 9295241
Submitted batch job 9295242
Submitted batch job 9295243
Submitted batch job 9295244
Submitted batch job 9295245
Submitted batch job 9295246
Submitted batch job 9295247
Submitted batch job 9295248
Submitted batch job 9295249
Submitted batch job 9295250
Submitted batch job 9295251
Submitted batch job 9295252
Submitted batch job 9295253
Submitted batch job 9295254
Submitted batch job 9295255
Submitted batch job 9295256
Submitted batch job 9295257
[dbarshis@turing1 TotalAnnotation]$ for i in ./submissionscripts/qsubsbatch_2/*.sh; do sbatch $i ; done
Submitted batch job 9295258
Submitted batch job 9295259
Submitted batch job 9295260
Submitted batch job 9295261
Submitted batch job 9295262
Submitted batch job 9295263
Submitted batch job 9295264
Submitted batch job 9295265
Submitted batch job 9295266
Submitted batch job 9295267
Submitted batch job 9295268
Submitted batch job 9295269
Submitted batch job 9295270
Submitted batch job 9295271
Submitted batch job 9295272
Submitted batch job 9295273
Submitted batch job 9295274
Submitted batch job 9295275
Submitted batch job 9295276
Submitted batch job 9295277
[dbarshis@turing1 TotalAnnotation]$ for i in ./submissionscripts/qsubsbatch_3/*.sh; do sbatch $i ; done
Submitted batch job 9295278
Submitted batch job 9295279
Submitted batch job 9295280
Submitted batch job 9295281
Submitted batch job 9295282
Submitted batch job 9295283
Submitted batch job 9295284
Submitted batch job 9295285
Submitted batch job 9295286
[dbarshis@turing1 TotalAnnotation]$ for i in ./submissionscripts/qsubsbatch_6/*.sh; do sbatch $i ; done
Submitted batch job 9295287
Submitted batch job 9295288
Submitted batch job 9295289
Submitted batch job 9295290
Submitted batch job 9295291
Submitted batch job 9295292
Submitted batch job 9295293
Submitted batch job 9295294
Submitted batch job 9295295
Submitted batch job 9295296
Submitted batch job 9295297
Submitted batch job 9295298
Submitted batch job 9295299
Submitted batch job 9295300
Submitted batch job 9295301
Submitted batch job 9295302
Submitted batch job 9295303
Submitted batch job 9295304
Submitted batch job 9295305
Submitted batch job 9295306
Submitted batch job 9295307
Submitted batch job 9295308
[dbarshis@turing1 TotalAnnotation]$ for i in ./submissionscripts/qsubsbatch_7/*.sh; do sbatch $i ; done
Submitted batch job 9295309
Submitted batch job 9295310
Submitted batch job 9295311
Submitted batch job 9295312
Submitted batch job 9295313
Submitted batch job 9295314
Submitted batch job 9295315
Submitted batch job 9295316
Submitted batch job 9295317
Submitted batch job 9295318
Submitted batch job 9295319
Submitted batch job 9295320
Submitted batch job 9295321
Submitted batch job 9295322
Submitted batch job 9295323
Submitted batch job 9295324
Submitted batch job 9295325
Submitted batch job 9295326
Submitted batch job 9295327
Submitted batch job 9295328
[dbarshis@turing1 TotalAnnotation]$ ls ./submissionscripts/
qsubsbatch_1  qsubsbatch_3  qsubsbatch_5  qsubsbatch_7
qsubsbatch_2  qsubsbatch_4  qsubsbatch_6  qsubsbatch_8
[dbarshis@turing1 TotalAnnotation]$ for i in ./submissionscripts/qsubsbatch_8/*.sh; do sbatch $i ; done
Submitted batch job 9295329
Submitted batch job 9295330
Submitted batch job 9295331
Submitted batch job 9295332
Submitted batch job 9295333
Submitted batch job 9295334
Submitted batch job 9295335
```

## 2021-03-31 Re-do NRonly

```
[dbarshis@turing1 TotalAnnotation]$ pwd
/cm/shared/courses/dbarshis/barshislab/VRad/taxons/Porites_astreoides/Paytan/P_ast/refassembly/hybridref/TotalAnnotation
[dbarshis@turing1 TotalAnnotation]$ mv dbparams_djbnronly.txt ./nronly/
[dbarshis@turing1 TotalAnnotation]$ cp hybridreference.fasta ./nronly/
[dbarshis@turing1 nronly]$ pwd
/cm/shared/courses/dbarshis/barshislab/VRad/taxons/Porites_astreoides/Paytan/P_ast/refassembly/hybridref/TotalAnnotation/nronly
[dbarshis@turing1 nronly]$ ~/scripts/splitfasta_cluster_batchblast2dbs_blastx_SLURM.py hybridreference.fasta 1500 dbparams_djbnronly.txt 
[dbarshis@turing1 nronly]$ bash
[dbarshis@turing1 nronly]$ pwd
/cm/shared/courses/dbarshis/barshislab/VRad/taxons/Porites_astreoides/Paytan/P_ast/refassembly/hybridref/TotalAnnotation/nronly
[dbarshis@turing1 nronly]$ for i in submissionscripts/qsubsbatch_*/*.sh ; do sbatch $i ; done
Submitted batch job 9295885
Submitted batch job 9295886
Submitted batch job 9295887
Submitted batch job 9295888
Submitted batch job 9295889
Submitted batch job 9295890
Submitted batch job 9295891
Submitted batch job 9295892
Submitted batch job 9295893
Submitted batch job 9295894
Submitted batch job 9295895
Submitted batch job 9295896
Submitted batch job 9295897
Submitted batch job 9295898
Submitted batch job 9295899
Submitted batch job 9295900
Submitted batch job 9295901
Submitted batch job 9295902
Submitted batch job 9295903
Submitted batch job 9295904
Submitted batch job 9295905
Submitted batch job 9295906
Submitted batch job 9295907
Submitted batch job 9295908
Submitted batch job 9295909
Submitted batch job 9295910
Submitted batch job 9295911
Submitted batch job 9295912
Submitted batch job 9295913
Submitted batch job 9295914
Submitted batch job 9295915
Submitted batch job 9295916
Submitted batch job 9295917
Submitted batch job 9295918
Submitted batch job 9295919
Submitted batch job 9295920
Submitted batch job 9295921
Submitted batch job 9295922
Submitted batch job 9295923
Submitted batch job 9295924
Submitted batch job 9295925
Submitted batch job 9295926
Submitted batch job 9295927
Submitted batch job 9295928
Submitted batch job 9295929
Submitted batch job 9295930
Submitted batch job 9295931
Submitted batch job 9295932
Submitted batch job 9295933
```

  * error, looks like error with nr database

```
[dbarshis@turing1 nronly]$ tail blastx2nr.txt 
BLAST Database error: Input db vol size does not match lmdb vol size
```

  * re-downloading nr with update_blastdb.pl
  
```
[dbarshis@coreV2-25-002 databases]$ pwd
/cm/shared/apps/blast/databases
[dbarshis@coreV2-25-002 databases]$ cat djbNRdownloadv2.sh 
#!/bin/bash -l

#SBATCH -o NRDownload.txt
#SBATCH -n 2
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=NRDownload

enable_lmod
module load blast
update_blastdb.pl --decompress nr
[dbarshis@coreV2-25-002 databases]$ sbatch djbNRdownloadv2.sh 
Submitted batch job 9295943
```

  * failed again
  
```
[dbarshis@coreV2-25-002 databases]$ tail NRDownload.txt
Unknown option: num_cores
Failed to parse command line options

The following have been reloaded with a version change:
  1) sge/2011.11p1 => sge/2011

Connected to NCBI
Downloading nr (44 volumes) ...
Downloading nr.00.tar.gz... [OK]
Downloading nr.01.tar.gz...Failed to download nr.01.tar.gz.md5!
[dbarshis@coreV2-25-002 databases]$ sbatch djbNR
djbNRdownload.sh*   djbNRdownloadv2.sh* 
[dbarshis@coreV2-25-002 databases]$ sbatch djbNRdownloadv2.sh 
Submitted batch job 9295944
```