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

  * maybe worked this time, trying a test blastx2nr job with the updated database

```
[dbarshis@coreV2-25-002 nronly]$ pwd
/cm/shared/courses/dbarshis/barshislab/VRad/taxons/Porites_astreoides/Paytan/P_ast/refassembly/hybridref/TotalAnnotation/nronly
[dbarshis@coreV2-25-002 nronly]$ cat ./submissionscripts/qsubsbatch_1/hybridreference_00001-01500_blastsub_nr.sh 
#!/bin/bash -l

#SBATCH -o blastx2nr.txt
#SBATCH -n 2
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=splitblast_djb
enable_lmod
module load blast
blastx -db /cm/shared/apps/blast/databases/nr -query ./splitfastas/hybridreference_00001-01500.fasta -out ./blastoutputs/nr/hybridreference_00001-01500_blastx2nr.xml -evalue 0.05 -outfmt 5 -gapopen 11 -gapextend 1 -word_size 3 -matrix BLOSUM62 -max_target_seqs 20 -num_threads 2
[dbarshis@coreV2-25-002 nronly]$ sbatch ./submissionscripts/qsubsbatch_1/hybridreference_00001-01500_blastsub_nr.sh
Submitted batch job 9296213
```

  * working, trying again
  
```
[dbarshis@coreV2-25-002 nronly]$ pwd
/cm/shared/courses/dbarshis/barshislab/VRad/taxons/Porites_astreoides/Paytan/P_ast/refassembly/hybridref/TotalAnnotation/nronly
[dbarshis@coreV2-25-002 nronly]$ ~/scripts/splitfasta_cluster_batchblast2dbs_blastx_SLURM.py hybridreference.fasta 1500 dbparams_djbnronly.txt
[dbarshis@coreV2-25-002 nronly]$ pwd
/cm/shared/courses/dbarshis/barshislab/VRad/taxons/Porites_astreoides/Paytan/P_ast/refassembly/hybridref/TotalAnnotation/nronly
[dbarshis@coreV2-25-002 nronly]$ bash

The following have been reloaded with a version change:
  1) sge/2011.11p1 => sge/2011

[dbarshis@coreV2-25-002 nronly]$ for i in ./submissionscripts/qsubsbatch_*/*.sh ; do sbatch $i ; done
Submitted batch job 9296333
Submitted batch job 9296334
Submitted batch job 9296335
Submitted batch job 9296336
Submitted batch job 9296337
Submitted batch job 9296338
Submitted batch job 9296339
Submitted batch job 9296340
Submitted batch job 9296341
Submitted batch job 9296342
Submitted batch job 9296343
Submitted batch job 9296344
Submitted batch job 9296345
Submitted batch job 9296346
Submitted batch job 9296347
Submitted batch job 9296348
Submitted batch job 9296349
Submitted batch job 9296350
Submitted batch job 9296351
Submitted batch job 9296352
Submitted batch job 9296353
Submitted batch job 9296354
Submitted batch job 9296355
Submitted batch job 9296356
Submitted batch job 9296357
Submitted batch job 9296358
Submitted batch job 9296359
Submitted batch job 9296360
Submitted batch job 9296361
Submitted batch job 9296362
Submitted batch job 9296363
Submitted batch job 9296364
Submitted batch job 9296365
Submitted batch job 9296366
Submitted batch job 9296367
Submitted batch job 9296368
Submitted batch job 9296369
Submitted batch job 9296370
Submitted batch job 9296371
Submitted batch job 9296372
Submitted batch job 9296373
Submitted batch job 9296374
Submitted batch job 9296375
Submitted batch job 9296376
Submitted batch job 9296377
Submitted batch job 9296378
Submitted batch job 9296379
Submitted batch job 9296380
Submitted batch job 9296381
```

  * Working on Sid
  
```
[dbarshis@turing1 TotalAnnotation]$ pwd
/cm/shared/courses/dbarshis/barshislab/VRad/taxons/Siderastrea_siderea/hybridref_final/TotalAnnotation
[dbarshis@turing1 TotalAnnotation]$ ~/scripts/splitfasta_cluster_batchblast2dbs_blastx_SLURM.py Sid_hybridref_final.fasta 1500 dbparams_djb.txt 
[dbarshis@turing1 TotalAnnotation]$ bash
[dbarshis@turing1 TotalAnnotation]$ for i in ./submissionscripts/qsubsbatch_*/*.sh ; do sbatch $i; done
Submitted batch job 9296382
Submitted batch job 9296383
Submitted batch job 9296384
Submitted batch job 9296385
Submitted batch job 9296386
Submitted batch job 9296387
Submitted batch job 9296388
Submitted batch job 9296389
Submitted batch job 9296390
Submitted batch job 9296391
Submitted batch job 9296392
Submitted batch job 9296393
Submitted batch job 9296394
Submitted batch job 9296395
Submitted batch job 9296396
Submitted batch job 9296397
Submitted batch job 9296398
Submitted batch job 9296399
Submitted batch job 9296400
Submitted batch job 9296401
Submitted batch job 9296402
Submitted batch job 9296403
Submitted batch job 9296404
Submitted batch job 9296405
Submitted batch job 9296406
Submitted batch job 9296407
Submitted batch job 9296408
Submitted batch job 9296409
Submitted batch job 9296410
Submitted batch job 9296411
Submitted batch job 9296412
Submitted batch job 9296413
Submitted batch job 9296414
Submitted batch job 9296415
Submitted batch job 9296416
Submitted batch job 9296417
Submitted batch job 9296418
Submitted batch job 9296419
Submitted batch job 9296420
Submitted batch job 9296421
Submitted batch job 9296422
Submitted batch job 9296423
Submitted batch job 9296424
Submitted batch job 9296425
Submitted batch job 9296426
Submitted batch job 9296427
Submitted batch job 9296428
Submitted batch job 9296429
Submitted batch job 9296430
Submitted batch job 9296431
Submitted batch job 9296432
Submitted batch job 9296433
Submitted batch job 9296434
Submitted batch job 9296435
Submitted batch job 9296436
Submitted batch job 9296437
Submitted batch job 9296438
Submitted batch job 9296439
Submitted batch job 9296440
Submitted batch job 9296441
Submitted batch job 9296442
Submitted batch job 9296443
Submitted batch job 9296444
Submitted batch job 9296445
Submitted batch job 9296446
Submitted batch job 9296447
Submitted batch job 9296448
Submitted batch job 9296449
Submitted batch job 9296450
Submitted batch job 9296451
Submitted batch job 9296452
Submitted batch job 9296453
Submitted batch job 9296454
Submitted batch job 9296455
Submitted batch job 9296456
Submitted batch job 9296457
Submitted batch job 9296458
Submitted batch job 9296459
Submitted batch job 9296460
Submitted batch job 9296461
Submitted batch job 9296462
Submitted batch job 9296463
Submitted batch job 9296464
Submitted batch job 9296465
Submitted batch job 9296466
Submitted batch job 9296467
Submitted batch job 9296468
Submitted batch job 9296469
Submitted batch job 9296470
Submitted batch job 9296471
Submitted batch job 9296472
Submitted batch job 9296473
Submitted batch job 9296474
Submitted batch job 9296475
Submitted batch job 9296476
Submitted batch job 9296477
Submitted batch job 9296478
Submitted batch job 9296479
Submitted batch job 9296480
Submitted batch job 9296481
Submitted batch job 9296482
Submitted batch job 9296483
Submitted batch job 9296484
Submitted batch job 9296485
Submitted batch job 9296486
Submitted batch job 9296487
Submitted batch job 9296488
Submitted batch job 9296489
Submitted batch job 9296490
Submitted batch job 9296491
Submitted batch job 9296492
Submitted batch job 9296493
Submitted batch job 9296494
Submitted batch job 9296495
Submitted batch job 9296496
Submitted batch job 9296497
Submitted batch job 9296498
Submitted batch job 9296499
Submitted batch job 9296500
Submitted batch job 9296501
Submitted batch job 9296502
Submitted batch job 9296503
Submitted batch job 9296504
Submitted batch job 9296505
Submitted batch job 9296506
Submitted batch job 9296507
Submitted batch job 9296508
Submitted batch job 9296509
Submitted batch job 9296510
Submitted batch job 9296511
Submitted batch job 9296512
Submitted batch job 9296513
Submitted batch job 9296514
Submitted batch job 9296515
Submitted batch job 9296516
Submitted batch job 9296517
Submitted batch job 9296518
Submitted batch job 9296519
Submitted batch job 9296520
Submitted batch job 9296521
Submitted batch job 9296522
Submitted batch job 9296523
Submitted batch job 9296524
Submitted batch job 9296525
Submitted batch job 9296526
Submitted batch job 9296527
Submitted batch job 9296528
Submitted batch job 9296529
Submitted batch job 9296530
Submitted batch job 9296531
Submitted batch job 9296532
Submitted batch job 9296533
Submitted batch job 9296534
Submitted batch job 9296535
Submitted batch job 9296536
Submitted batch job 9296537
Submitted batch job 9296538
Submitted batch job 9296539
Submitted batch job 9296540
Submitted batch job 9296541
Submitted batch job 9296542
Submitted batch job 9296543
Submitted batch job 9296544
Submitted batch job 9296545
Submitted batch job 9296546
Submitted batch job 9296547
Submitted batch job 9296548
Submitted batch job 9296549
Submitted batch job 9296550
Submitted batch job 9296551
Submitted batch job 9296552
Submitted batch job 9296553
Submitted batch job 9296554
Submitted batch job 9296555
Submitted batch job 9296556
Submitted batch job 9296557
Submitted batch job 9296558
Submitted batch job 9296559
Submitted batch job 9296560
Submitted batch job 9296561
Submitted batch job 9296562
Submitted batch job 9296563
Submitted batch job 9296564
Submitted batch job 9296565
Submitted batch job 9296566
Submitted batch job 9296567
Submitted batch job 9296568
Submitted batch job 9296569
Submitted batch job 9296570
Submitted batch job 9296571
Submitted batch job 9296572
Submitted batch job 9296573
Submitted batch job 9296574
Submitted batch job 9296575
Submitted batch job 9296576
Submitted batch job 9296577
Submitted batch job 9296578
Submitted batch job 9296579
Submitted batch job 9296580
Submitted batch job 9296581
Submitted batch job 9296582
Submitted batch job 9296583
Submitted batch job 9296584
Submitted batch job 9296585
Submitted batch job 9296586
Submitted batch job 9296587
Submitted batch job 9296588
Submitted batch job 9296589
Submitted batch job 9296590
Submitted batch job 9296591
Submitted batch job 9296592
Submitted batch job 9296593
Submitted batch job 9296594
Submitted batch job 9296595
Submitted batch job 9296596
Submitted batch job 9296597
Submitted batch job 9296598
Submitted batch job 9296599
Submitted batch job 9296600
Submitted batch job 9296601
Submitted batch job 9296602
Submitted batch job 9296603
Submitted batch job 9296604
Submitted batch job 9296605
Submitted batch job 9296606
Submitted batch job 9296607
Submitted batch job 9296608
Submitted batch job 9296609
Submitted batch job 9296610
Submitted batch job 9296611
Submitted batch job 9296612
Submitted batch job 9296613
Submitted batch job 9296614
Submitted batch job 9296615
Submitted batch job 9296616
Submitted batch job 9296617
Submitted batch job 9296618
Submitted batch job 9296619
Submitted batch job 9296620
Submitted batch job 9296621
```

## 2021-05-06_CBASS84SNPs

```
[dbarshis@coreV3-23-032 Stylophora_pistillata]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Stylophora_pistillata
[dbarshis@coreV3-23-032 Stylophora_pistillata]$ tar -zxvf coral_snp_calls.tar.gz
[dbarshis@coreV3-23-032 coral_snp_calls]$ gunzip *.gz

[dbarshis@coreV3-23-032 coral_snp_calls]$ enable_lmod

The following have been reloaded with a version change:
  1) slurm/20.11.5 => slurm/20.11

[dbarshis@coreV3-23-032 coral_snp_calls]$ module load vcftools
###Testing how to merge individual .vcf files
[dbarshis@coreV3-23-032 coral_snp_calls]$ module load samtools
[dbarshis@coreV3-23-032 coral_snp_calls]$ bcftools
[dbarshis@coreV3-23-032 coral_snp_calls]$ bcftools index ES1-33A_samtools_final.vcf.gz 
[dbarshis@coreV3-23-032 coral_snp_calls]$ bcftools index ES1-36A_samtools_final.vcf.gz 
[dbarshis@coreV3-23-032 coral_snp_calls]$ bcftools merge ES1-30A_samtools_final.vcf.gz ES1-33A_samtools_final.vcf.gz ES1-36A_samtools_final.vcf.gz -o ES1Mergedtest.vcf
###it worked, now trying on the full set
[dbarshis@coreV3-23-032 coral_snp_calls]$ for i in *.gz; do bcftools index $i; done
[dbarshis@turing1 coral_snp_calls]$ cat Mergevcf.sh 
#!/bin/bash -l

#SBATCH -o mergevcf.txt
#SBATCH -n 2
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=mergevcf_djb

enable_lmod
module load samtools

bcftools merge *.vcf.gz -o CBASS84Merged.vcf
[dbarshis@turing1 coral_snp_calls]$ sbatch Mergevcf.sh 
Submitted batch job 9306946
```
 * VCF filtering
 
```
[dbarshis@coreV2-25-knc-001 coral_snp_calls]$ vcftools --vcf CBASS84Merged.vcf 

VCFtools - 0.1.16
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf CBASS84Merged.vcf

Warning: Expected at least 2 parts in INFO entry: ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes for each ALT allele, in the same order as listed">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
Warning: Expected at least 2 parts in INFO entry: ID=DP4,Number=4,Type=Integer,Description="Number of high-quality ref-forward , ref-reverse, alt-forward and alt-reverse bases">
After filtering, kept 83 out of 83 Individuals
After filtering, kept 4572089 out of a possible 4572089 Sites
Run Time = 36.00 seconds

```
  * something up with serdar's vcfs, think it's his ploidy 1 argument, cp-ing over the trimmed fqs to do it myself

```
[dbarshis@turing1 fastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Stylophora_pistillata/DansSNPing/fastqs
[dbarshis@turing1 fastqs]$ cat cpfastqs.sh 
#!/bin/bash -l

#SBATCH -o cpfastqs.txt
#SBATCH -n 2
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=cpfastqs_djb

cp /cm/shared/courses/dbarshis/barshislab/danb/taxons/Stylophora_pistillata/data/CBASS84_RNASeq_Final/*/trimmed/*.fq.gz ./

[dbarshis@turing1 fastqs]$ sbatch cpfastqs.sh 
Submitted batch job 9307010

##checking that R1 and R2 are still paired
[dbarshis@coreV2-25-knc-001 fastqs]$ grep -c "@K00235" M_18_3789_ES1-30A_D704-D506_L005_R1_001_val_1.fastq
31962140
[dbarshis@coreV2-25-knc-001 fastqs]$ wc -l M_18_3789_ES1-30A_D704-D506_L005_R1_001_val_1.fastq 
127848560 M_18_3789_ES1-30A_D704-D506_L005_R1_001_val_1.fastq
[dbarshis@coreV2-25-knc-001 fastqs]$ echo "127848560/4" | bc -l
31962140.00000000000000000000
[dbarshis@coreV2-25-knc-001 fastqs]$ head M_18_3789_ES1-30A_D704-D506_L005_R1_001_val_1.fastq
@K00235:152:HWNJ7BBXX:5:1101:2940:1349 1:N:0:GAGATTCC+TAAGATTA
CCTCCACCAGAGTTTCCTCTGGCTTCACCCTATTCAGGCATAGTTCACCATCTTTCGGGTCCCAACAGACACGCTCTCACTCAAACCTTCCTAAGGGTAGGATAGGGCGGTCGATGATGCGCCGAC
+
AAFFFJJJJJJJFJJJJJJJJFJJJJJFFJJJJJ<JJJJJ<7JFJJJJFJJJJJJJJJJFJJFJJJJJJJAFJ7JFJJFJJJJJJJJJFAJJJ<<FF<AJJFJJJJ-7-7-FJFFFA7AJJAA77A
@K00235:152:HWNJ7BBXX:5:1101:3082:1349 1:N:0:GAGATTCC+TAAGATTA
CAAAAATGGCCCACTAGGCACTCGCATTCCAAGTCCGGCTTCGTTCGGGCAAGTCGGACATCTTACCAATTTAAAGTTTGAGAATAGGTTAAGGTCCTTTTGACCCGAAGGCCTCTAATCATTCGCTTTACCTGATAAAACTGCACTTCCG
+
AAFFFJJJJJJJFJJFFJJFJJJJJJJJFFJJJ<A<JFJJJJJJJJJFJFFJJJJJJJJF-A77<FFJA<JAAJJFA<FJJJJFFA-F<<<AFFJ-A<-FJ7AJFJ-7FJFJJ<JJJ-AFFJ-AAFA--7<FF<<-7F<AJ<AF<JFFFFF
@K00235:152:HWNJ7BBXX:5:1101:5518:1349 1:N:0:GAGATTCC+TAAGATTA
CACGTTCCCTATTGGTGGGTGAACAATCCATCACTTGGCGAATTCTGCTTCGCAATGATAGGAAGAGCCGACATCGAAGGATCAAAAAGCAACGTCGCTATGAACGCTTGGCTGCCACAAGCCAGTTATCCCTGTGGTAACTTTTCTGAC
[dbarshis@coreV2-25-knc-001 fastqs]$ tail M_18_3789_ES1-30A_D704-D506_L005_R1_001_val_1.fastq
+
AAFAFJJJJJJJFJJJFFJFJJJJJJJJJJJFF<FFJJFJFJJJJJJJJJFJJ<AJJJJJ-FAJ7AAJJJJFFJJ7AJJJFFJFJFJJJJJJJJJ77JFJJJJJJFFFJAAAJFJJJJJJFJJJJJJFFFJJFJJJAJAFFJJJAJFJJJJ
@K00235:152:HWNJ7BBXX:5:2228:31822:49001 1:N:0:GAGATTCC+TAAGATTA
TTTACCTGATAAAACTGCACTTCCGAGTTCCAGCTATCCTGAGGGAAACTTCGGAGGGAACCAGCTACTAGACAGTTCGATTAGTCTTTCGCCCCTATACCCAAATTCGACGATCGATTTGCACGTCAGAATCGCTACGAGCCTCCACC
+
AAAFFJJJJJJJJJJJJJJJJJJJJJFFJJJFJJJJJJAJJJJFFJJJJJJ-FJJJJJFJJ-F-7AJJJJJJAJJFFJFFJJJJJJJJJJJJJFJJJJJFJJJJJJJJJAAAJJFFJJJJJFA7FJJJJFJJFJJJAAF7AFJJJJFFF
@K00235:152:HWNJ7BBXX:5:2228:31943:49001 1:N:0:GAGATTCC+TAAGATTA
CTCACTAAACCATTCAATCGGTAGTAGCGACGGGCGGTGTGTACAAAGGGCAGGGACGTAATCAACGCGAGCTGATGACTCGCGCTTACTAGGAATTCCTCGTTCAAGATCAATAATTGCAAGGATCTATCC
+
AAFFFJJJJJJFJJFFFJFJJJJJJJJFJJFFJFJJJFJJJJJJJJJJJJFJJFF7JJFJJJJFJJFJJFJJJJJJJJJJJJJJJJJJJFFJJJFFJFFAF-FAAJJJJ<AAJJFJJJJJJFJJFJJJJJJJ
[dbarshis@coreV2-25-knc-001 fastqs]$ grep -c "@K00235" M_18_3789_ES1-30A_D704-D506_L005_R2_001_val_2.fastq 
31962140
[dbarshis@coreV2-25-knc-001 fastqs]$ wc -l M_18_3789_ES1-30A_D704-D506_L005_R2_001_val_2.fastq
127848560 M_18_3789_ES1-30A_D704-D506_L005_R2_001_val_2.fastq
[dbarshis@coreV2-25-knc-001 fastqs]$ echo "127848560/4" | bc -l
31962140.00000000000000000000
[dbarshis@coreV2-25-knc-001 fastqs]$ head M_18_3789_ES1-30A_D704-D506_L005_R2_001_val_2.fastq
@K00235:152:HWNJ7BBXX:5:1101:2940:1349 2:N:0:GAGATTCC+TAAGATTA
GTCGGCGCATCATCGACCGACCTATCCTACCCTTAGGAAGGTTTGAGTGAGAGCGTGTCTGTTGGGACCCGAAAGATGGTGAACTATGCCTGAATAGGGTGACGCCAGAGGAAACTCTGGTGGAGG
+
AAFFFJJJJFJJ<F<JJJJJJJJJJJJFJJFJAJFFJAAAJ-7FFJJAFJJJFJJJJAJ7AAFJJJJJJJFJJJJJJJJ<JJJJ<FJJJF-FJJ<JFAF-JF-AFA7FFJJJJJJFFAFFFFFFFF
@K00235:152:HWNJ7BBXX:5:1101:3082:1349 2:N:0:GAGATTCC+TAAGATTA
CCGAAGTTTCCCTCAGGATAGCTGGAACTCGGAAGTGCAGTTTTATCAGGTAAAGCGAATGATTAGAGGCCTTAGGGTCAAAAGGACCTTAACCTATTCTCAAACTTTAAATTGGTAAGATGTCCGACTCGCCCGACCGAAGCCGGACGTG
+
AAFFFJFJJJJJA<JFFJ<<<FFFJAJJJJAA7JJAFF7<<-<A--<<<A-7-F<7-AF-<<<F-7F7FJ7<-FJ<J-FJFJ<FAJJJ-7F-AF-A-<F7FAAAAAAJJF-<FAJFJJ7--FJJJJ-FA---AF-7)--)AJFJA<AJ)<A
@K00235:152:HWNJ7BBXX:5:1101:5518:1349 2:N:0:GAGATTCC+TAAGATTA
GCGTGGCCTATCGATCCTTTAGTCTTTAGGAGTTTTAAGCTAGAGGTGTCAGAAAAGTTACCACAGGGATAACTGGCTTGTGGCAGCCAAGCGTTCATAGCGACGGCGCTTTTTGATCCTTCGATGTCGGCTCTTCCTATCATTGCGCAGC
[dbarshis@coreV2-25-knc-001 fastqs]$ tail M_18_3789_ES1-30A_D704-D506_L005_R2_001_val_2.fastq
+
AAFFFJJJJFJJJFJJJJJFJJJJJJJJJJJJJJJJJJJFJJFJJJ<AAJ<JAJJJJJJJ-AAJJFFFFAJFFA-FJJJJJJJJJA<JJJJJJJFJJJJJJJJJJJJ<FFFFJJJ-FAAFJJJJJ<F-AA-<-<FAJJA7AAFJFJ<FA
@K00235:152:HWNJ7BBXX:5:2228:31822:49001 2:N:0:GAGATTCC+TAAGATTA
TGTTGGGACCCGAAAGATGGTGAACTATGCCTGAATAGGGTGAAGCCAGAGGAAACTCTGGTGGAGGCTCGTAGCGATTCTGACGTGCAAATCGATCGTCGAATTTGGGTATAGGGGCGAAAGACTAATCGAACTGTCTAGTAGCTGGTTC
+
AAFFFJJ<7FJA7-AJFFFFFJJJJJJJJJA7J<FJFJJJ-AJFJFJ-FJJJFJFFFFJFF<FJJJJFJJF7FJJ<JJJFJJJJJJJJJJAJAJFFJJ-AJAJ7<JJJJFAFFFJJJJJF<FJAFAFJJJJJJFFAFJJFJFJA<FFJJJA
@K00235:152:HWNJ7BBXX:5:2228:31943:49001 2:N:0:GAGATTCC+TAAGATTA
GGATAGATCCTTGCAATTATTGATCTTGAACGAGGAATTCCTAGTAAGCGCGAGTCATCAGCTCGCGTTGATTACGTCCCTGCCCTTTGTACACACCGCCCGTCGCTACTACCGATTGAATGGTTTAGTGAG
+
AAAFFJJJJFJFJJJJJJJJJJFJFJJJ<FJF<FJFJJJJAA<FJJFAFFFFFJJJFJJJJJFJJJJJJJ7FJJJFAJJJJJJJF<JA<JJJFJJJJFJAJJ<-AJJAJFFFFJ<AFFF<F<JFFJJFJJFF

##it would appear so!
[dbarshis@coreV2-25-knc-001 fastqs]$ pwd
/cm/shared/courses/dbarshis/barshislab/danb/taxons/Stylophora_pistillata/DansSNPing/fastqs
[dbarshis@coreV2-25-knc-001 fastqs]$ cat gunziprenamefastqs.sh 
#!/bin/bash -l

#SBATCH -o gunzipfastqs.txt
#SBATCH -n 2
#SBATCH --mail-user=dbarshis@odu.edu
#SBATCH --mail-type=END
#SBATCH --job-name=gunzipfastqs_djb

gunzip -c M_18_3789_ES1-30A_D704-D506_L005_R1_001_val_1.fq.gz > ES1-30A_R1.fastq
gunzip -c M_18_3789_ES1-30A_D704-D506_L005_R2_001_val_2.fq.gz > ES1-30A_R2.fastq
gunzip -c M_18_3793_ES1-33A_D711-D506_L006_R1_001_val_1.fq.gz > ES1-33A_R1.fastq
gunzip -c M_18_3793_ES1-33A_D711-D506_L006_R2_001_val_2.fq.gz > ES1-33A_R2.fastq
gunzip -c M_18_3798_ES2-36A_D707-D507_L006_R1_001_val_1.fq.gz > ES2-36A_R1.fastq
gunzip -c M_18_3798_ES2-36A_D707-D507_L006_R2_001_val_2.fq.gz > ES2-36A_R2.fastq
gunzip -c M_18_3790_ES2-30A_D705-D506_L006_R1_001_val_1.fq.gz > ES2-30A_R1.fastq
gunzip -c M_18_3790_ES2-30A_D705-D506_L006_R2_001_val_2.fq.gz > ES2-30A_R2.fastq
gunzip -c M_18_3813_ES2-33A_D712-D506_L008_R1_001_val_1.fq.gz > ES2-33A_R1.fastq
gunzip -c M_18_3813_ES2-33A_D712-D506_L008_R2_001_val_2.fq.gz > ES2-33A_R2.fastq
gunzip -c M_18_3816_ES3-36A_D708-D507_L008_R1_001_val_1.fq.gz > ES3-36A_R1.fastq
gunzip -c M_18_3816_ES3-36A_D708-D507_L008_R2_001_val_2.fq.gz > ES3-36A_R2.fastq
gunzip -c M_18_3810_ES3-30A_D706-D506_L007_R1_001_val_1.fq.gz > ES3-30A_R1.fastq
gunzip -c M_18_3810_ES3-30A_D706-D506_L007_R2_001_val_2.fq.gz > ES3-30A_R2.fastq
gunzip -c M_18_3814_ES3-33A_D701-D507_L008_R1_001_val_1.fq.gz > ES3-33A_R1.fastq
gunzip -c M_18_3814_ES3-33A_D701-D507_L008_R2_001_val_2.fq.gz > ES3-33A_R2.fastq
gunzip -c M_18_3817_ES4-36A_D709-D507_L008_R1_001_val_1.fq.gz > ES4-36A_R1.fastq
gunzip -c M_18_3817_ES4-36A_D709-D507_L008_R2_001_val_2.fq.gz > ES4-36A_R2.fastq
gunzip -c M_18_3791_ES4-30A_D707-D506_L006_R1_001_val_1.fq.gz > ES4-30A_R1.fastq
gunzip -c M_18_3791_ES4-30A_D707-D506_L006_R2_001_val_2.fq.gz > ES4-30A_R2.fastq
gunzip -c M_18_3794_ES4-33A_D702-D507_L006_R1_001_val_1.fq.gz > ES4-33A_R1.fastq
gunzip -c M_18_3794_ES4-33A_D702-D507_L006_R2_001_val_2.fq.gz > ES4-33A_R2.fastq
gunzip -c M_18_3811_ES5-30A_D708-D506_L008_R1_001_val_1.fq.gz > ES5-30A_R1.fastq
gunzip -c M_18_3811_ES5-30A_D708-D506_L008_R2_001_val_2.fq.gz > ES5-30A_R2.fastq
gunzip -c M_18_3818_ES5-36A_D710-D507_L008_R1_001_val_1.fq.gz > ES5-36A_R1.fastq
gunzip -c M_18_3818_ES5-36A_D710-D507_L008_R2_001_val_2.fq.gz > ES5-36A_R2.fastq
gunzip -c M_18_3795_ES5-33A_D703-D507_L006_R1_001_val_1.fq.gz > ES5-33A_R1.fastq
gunzip -c M_18_3795_ES5-33A_D703-D507_L006_R2_001_val_2.fq.gz > ES5-33A_R2.fastq
gunzip -c M_18_3819_ES6-36A_D711-D507_L008_R1_001_val_1.fq.gz > ES6-36A_R1.fastq
gunzip -c M_18_3819_ES6-36A_D711-D507_L008_R2_001_val_2.fq.gz > ES6-36A_R2.fastq
gunzip -c M_18_3796_ES6-33A_D704-D507_L006_R1_001_val_1.fq.gz > ES6-33A_R1.fastq
gunzip -c M_18_3796_ES6-33A_D704-D507_L006_R2_001_val_2.fq.gz > ES6-33A_R2.fastq
gunzip -c M_18_3820_ES7-36A_D712-D507_L008_R1_001_val_1.fq.gz > ES7-36A_R1.fastq
gunzip -c M_18_3820_ES7-36A_D712-D507_L008_R2_001_val_2.fq.gz > ES7-36A_R2.fastq
gunzip -c M_18_3812_ES7-30A_D710-D506_L008_R1_001_val_1.fq.gz > ES7-30A_R1.fastq
gunzip -c M_18_3812_ES7-30A_D710-D506_L008_R2_001_val_2.fq.gz > ES7-30A_R2.fastq
gunzip -c M_18_3797_ES7-33A_D705-D507_L006_R1_001_val_1.fq.gz > ES7-33A_R1.fastq
gunzip -c M_18_3797_ES7-33A_D705-D507_L006_R2_001_val_2.fq.gz > ES7-33A_R2.fastq
gunzip -c M_18_3815_ES1-36A_D706-D507_L008_R1_001_val_1.fq.gz > ES1-36A_R1.fastq
gunzip -c M_18_3815_ES1-36A_D706-D507_L008_R2_001_val_2.fq.gz > ES1-36A_R2.fastq
gunzip -c M_18_3735_KS1-30A_D701-D501_L001_R1_001_val_1.fq.gz > KS1-30A_R1.fastq
gunzip -c M_18_3735_KS1-30A_D701-D501_L001_R2_001_val_2.fq.gz > KS1-30A_R2.fastq
gunzip -c M_18_3739_KS1-33A_D708-D501_L001_R1_001_val_1.fq.gz > KS1-33A_R1.fastq
gunzip -c M_18_3739_KS1-33A_D708-D501_L001_R2_001_val_2.fq.gz > KS1-33A_R2.fastq
gunzip -c M_18_3745_KS1-36A_D703-D502_L001_R1_001_val_1.fq.gz > KS1-36A_R1.fastq
gunzip -c M_18_3745_KS1-36A_D703-D502_L001_R2_001_val_2.fq.gz > KS1-36A_R2.fastq
gunzip -c M_18_3736_KS2-30A_D702-D501_L001_R1_001_val_1.fq.gz > KS2-30A_R1.fastq
gunzip -c M_18_3736_KS2-30A_D702-D501_L001_R2_001_val_2.fq.gz > KS2-30A_R2.fastq
gunzip -c M_18_3804_KS2-33A_D709-D501_L007_R1_001_val_1.fq.gz > KS2-33A_R1.fastq
gunzip -c M_18_3804_KS2-33A_D709-D501_L007_R2_001_val_2.fq.gz > KS2-33A_R2.fastq
gunzip -c M_18_3746_KS2-36A_D704-D502_L002_R1_001_val_1.fq.gz > KS2-36A_R1.fastq
gunzip -c M_18_3746_KS2-36A_D704-D502_L002_R2_001_val_2.fq.gz > KS2-36A_R2.fastq
gunzip -c M_18_3737_KS3-30A_D703-D501_L001_R1_001_val_1.fq.gz > KS3-30A_R1.fastq
gunzip -c M_18_3737_KS3-30A_D703-D501_L001_R2_001_val_2.fq.gz > KS3-30A_R2.fastq
gunzip -c M_18_3740_KS3-33A_D710-D501_L001_R1_001_val_1.fq.gz > KS3-33A_R1.fastq
gunzip -c M_18_3740_KS3-33A_D710-D501_L001_R2_001_val_2.fq.gz > KS3-33A_R2.fastq
gunzip -c M_18_3805_KS3-36A_D705-D502_L007_R1_001_val_1.fq.gz > KS3-36A_R1.fastq
gunzip -c M_18_3805_KS3-36A_D705-D502_L007_R2_001_val_2.fq.gz > KS3-36A_R2.fastq
gunzip -c M_18_3801_KS4-30A_D704-D501_L007_R1_001_val_1.fq.gz > KS4-30A_R1.fastq
gunzip -c M_18_3801_KS4-30A_D704-D501_L007_R2_001_val_2.fq.gz > KS4-30A_R2.fastq
gunzip -c M_18_3741_KS4-33A_D711-D501_L001_R1_001_val_1.fq.gz > KS4-33A_R1.fastq
gunzip -c M_18_3741_KS4-33A_D711-D501_L001_R2_001_val_2.fq.gz > KS4-33A_R2.fastq
gunzip -c M_18_3747_KS4-36A_D706-D502_L002_R1_001_val_1.fq.gz > KS4-36A_R1.fastq
gunzip -c M_18_3747_KS4-36A_D706-D502_L002_R2_001_val_2.fq.gz > KS4-36A_R2.fastq
gunzip -c M_18_3738_KS5-30A_D705-D501_L001_R1_001_val_1.fq.gz > KS5-30A_R1.fastq
gunzip -c M_18_3738_KS5-30A_D705-D501_L001_R2_001_val_2.fq.gz > KS5-30A_R2.fastq
gunzip -c M_18_3742_KS5-33A_D712-D501_L001_R1_001_val_1.fq.gz > KS5-33A_R1.fastq
gunzip -c M_18_3742_KS5-33A_D712-D501_L001_R2_001_val_2.fq.gz > KS5-33A_R2.fastq
gunzip -c M_18_3748_KS5-36A_D707-D502_L002_R1_001_val_1.fq.gz > KS5-36A_R1.fastq
gunzip -c M_18_3748_KS5-36A_D707-D502_L002_R2_001_val_2.fq.gz > KS5-36A_R2.fastq
gunzip -c M_18_3802_KS6-30A_D706-D501_L007_R1_001_val_1.fq.gz > KS6-30A_R1.fastq
gunzip -c M_18_3802_KS6-30A_D706-D501_L007_R2_001_val_2.fq.gz > KS6-30A_R2.fastq
gunzip -c M_18_3743_KS6-33A_D701-D502_L001_R1_001_val_1.fq.gz > KS6-33A_R1.fastq
gunzip -c M_18_3743_KS6-33A_D701-D502_L001_R2_001_val_2.fq.gz > KS6-33A_R2.fastq
gunzip -c M_18_3749_KS6-36A_D708-D502_L002_R1_001_val_1.fq.gz > KS6-36A_R1.fastq
gunzip -c M_18_3749_KS6-36A_D708-D502_L002_R2_001_val_2.fq.gz > KS6-36A_R2.fastq
gunzip -c M_18_3803_KS7-30A_D707-D501_L007_R1_001_val_1.fq.gz > KS7-30A_R1.fastq
gunzip -c M_18_3803_KS7-30A_D707-D501_L007_R2_001_val_2.fq.gz > KS7-30A_R2.fastq
gunzip -c M_18_3744_KS7-33A_D702-D502_L001_R1_001_val_1.fq.gz > KS7-33A_R1.fastq
gunzip -c M_18_3744_KS7-33A_D702-D502_L001_R2_001_val_2.fq.gz > KS7-33A_R2.fastq
gunzip -c M_18_3806_KS7-36A_D709-D502_L007_R1_001_val_1.fq.gz > KS7-36A_R1.fastq
gunzip -c M_18_3806_KS7-36A_D709-D502_L007_R2_001_val_2.fq.gz > KS7-36A_R2.fastq
gunzip -c M_18_3750_S1-E-ST-30B_D710-D502_L002_R1_001_val_1.fq.gz > S1-E-ST-30B_R1.fastq
gunzip -c M_18_3750_S1-E-ST-30B_D710-D502_L002_R2_001_val_2.fq.gz > S1-E-ST-30B_R2.fastq
gunzip -c M_18_3757_S1-E-ST-33B_D705-D503_L003_R1_001_val_1.fq.gz > S1-E-ST-33B_R1.fastq
gunzip -c M_18_3757_S1-E-ST-33B_D705-D503_L003_R2_001_val_2.fq.gz > S1-E-ST-33B_R2.fastq
gunzip -c M_18_3764_S1-E-ST-36B_D712-D503_L003_R1_001_val_1.fq.gz > S1-E-ST-36B_R1.fastq
gunzip -c M_18_3764_S1-E-ST-36B_D712-D503_L003_R2_001_val_2.fq.gz > S1-E-ST-36B_R2.fastq
gunzip -c M_18_3771_S1-P-ST-30B_D707-D504_L004_R1_001_val_1.fq.gz > S1-P-ST-30B_R1.fastq
gunzip -c M_18_3771_S1-P-ST-30B_D707-D504_L004_R2_001_val_2.fq.gz > S1-P-ST-30B_R2.fastq
gunzip -c M_18_3777_S1-P-ST-33B_D702-D505_L004_R1_001_val_1.fq.gz > S1-P-ST-33B_R1.fastq
gunzip -c M_18_3777_S1-P-ST-33B_D702-D505_L004_R2_001_val_2.fq.gz > S1-P-ST-33B_R2.fastq
gunzip -c M_18_3784_S1-P-ST-36B_D709-D505_L005_R1_001_val_1.fq.gz > S1-P-ST-36B_R1.fastq
gunzip -c M_18_3784_S1-P-ST-36B_D709-D505_L005_R2_001_val_2.fq.gz > S1-P-ST-36B_R2.fastq
gunzip -c M_18_3751_S2-E-ST-30B_D711-D502_L002_R1_001_val_1.fq.gz > S2-E-ST-30B_R1.fastq
gunzip -c M_18_3751_S2-E-ST-30B_D711-D502_L002_R2_001_val_2.fq.gz > S2-E-ST-30B_R2.fastq
gunzip -c M_18_3758_S2-E-ST-33B_D706-D503_L003_R1_001_val_1.fq.gz > S2-E-ST-33B_R1.fastq
gunzip -c M_18_3758_S2-E-ST-33B_D706-D503_L003_R2_001_val_2.fq.gz > S2-E-ST-33B_R2.fastq
gunzip -c M_18_3765_S2-E-ST-36B_D701-D504_L003_R1_001_val_1.fq.gz > S2-E-ST-36B_R1.fastq
gunzip -c M_18_3765_S2-E-ST-36B_D701-D504_L003_R2_001_val_2.fq.gz > S2-E-ST-36B_R2.fastq
gunzip -c M_18_3772_S2-P-ST-30B_D708-D504_L004_R1_001_val_1.fq.gz > S2-P-ST-30B_R1.fastq
gunzip -c M_18_3772_S2-P-ST-30B_D708-D504_L004_R2_001_val_2.fq.gz > S2-P-ST-30B_R2.fastq
gunzip -c M_18_3778_S2-P-ST-33B_D703-D505_L004_R1_001_val_1.fq.gz > S2-P-ST-33B_R1.fastq
gunzip -c M_18_3778_S2-P-ST-33B_D703-D505_L004_R2_001_val_2.fq.gz > S2-P-ST-33B_R2.fastq
gunzip -c M_18_3785_S2-P-ST-36B_D710-D505_L005_R1_001_val_1.fq.gz > S2-P-ST-36B_R1.fastq
gunzip -c M_18_3785_S2-P-ST-36B_D710-D505_L005_R2_001_val_2.fq.gz > S2-P-ST-36B_R2.fastq
gunzip -c M_18_3752_S3-E-ST-30B_D712-D502_L002_R1_001_val_1.fq.gz > S3-E-ST-30B_R1.fastq
gunzip -c M_18_3752_S3-E-ST-30B_D712-D502_L002_R2_001_val_2.fq.gz > S3-E-ST-30B_R2.fastq
gunzip -c M_18_3759_S3-E-ST-33B_D707-D503_L003_R1_001_val_1.fq.gz > S3-E-ST-33B_R1.fastq
gunzip -c M_18_3759_S3-E-ST-33B_D707-D503_L003_R2_001_val_2.fq.gz > S3-E-ST-33B_R2.fastq
gunzip -c M_18_3766_S3-E-ST-36B_D702-D504_L003_R1_001_val_1.fq.gz > S3-E-ST-36B_R1.fastq
gunzip -c M_18_3766_S3-E-ST-36B_D702-D504_L003_R2_001_val_2.fq.gz > S3-E-ST-36B_R2.fastq
gunzip -c M_18_3773_S3-P-ST-30B_D709-D504_L004_R1_001_val_1.fq.gz > S3-P-ST-30B_R1.fastq
gunzip -c M_18_3773_S3-P-ST-30B_D709-D504_L004_R2_001_val_2.fq.gz > S3-P-ST-30B_R2.fastq
gunzip -c M_18_3779_S3-P-ST-33B_D704-D505_L005_R1_001_val_1.fq.gz > S3-P-ST-33B_R1.fastq
gunzip -c M_18_3779_S3-P-ST-33B_D704-D505_L005_R2_001_val_2.fq.gz > S3-P-ST-33B_R2.fastq
gunzip -c M_18_3786_S3-P-ST-36B_D711-D505_L005_R1_001_val_1.fq.gz > S3-P-ST-36B_R1.fastq
gunzip -c M_18_3786_S3-P-ST-36B_D711-D505_L005_R2_001_val_2.fq.gz > S3-P-ST-36B_R2.fastq
gunzip -c M_18_3753_S4-E-ST-30B_D701-D503_L002_R1_001_val_1.fq.gz > S4-E-ST-30B_R1.fastq
gunzip -c M_18_3753_S4-E-ST-30B_D701-D503_L002_R2_001_val_2.fq.gz > S4-E-ST-30B_R2.fastq
gunzip -c M_18_3760_S4-E-ST-33B_D708-D503_L003_R1_001_val_1.fq.gz > S4-E-ST-33B_R1.fastq
gunzip -c M_18_3760_S4-E-ST-33B_D708-D503_L003_R2_001_val_2.fq.gz > S4-E-ST-33B_R2.fastq
gunzip -c M_18_3767_S4-E-ST-36B_D703-D504_L003_R1_001_val_1.fq.gz > S4-E-ST-36B_R1.fastq
gunzip -c M_18_3767_S4-E-ST-36B_D703-D504_L003_R2_001_val_2.fq.gz > S4-E-ST-36B_R2.fastq
gunzip -c M_18_3774_S4-P-ST-30B_D710-D504_L004_R1_001_val_1.fq.gz > S4-P-ST-30B_R1.fastq
gunzip -c M_18_3774_S4-P-ST-30B_D710-D504_L004_R2_001_val_2.fq.gz > S4-P-ST-30B_R2.fastq
gunzip -c M_18_3780_S4-P-ST-33B_D705-D505_L005_R1_001_val_1.fq.gz > S4-P-ST-33B_R1.fastq
gunzip -c M_18_3780_S4-P-ST-33B_D705-D505_L005_R2_001_val_2.fq.gz > S4-P-ST-33B_R2.fastq
gunzip -c M_18_3808_S4-P-ST-36B_D712-D505_L007_R1_001_val_1.fq.gz > S4-P-ST-36B_R1.fastq
gunzip -c M_18_3808_S4-P-ST-36B_D712-D505_L007_R2_001_val_2.fq.gz > S4-P-ST-36B_R2.fastq
gunzip -c M_18_3754_S5-E-ST-30B_D702-D503_L002_R1_001_val_1.fq.gz > S5-E-ST-30B_R1.fastq
gunzip -c M_18_3754_S5-E-ST-30B_D702-D503_L002_R2_001_val_2.fq.gz > S5-E-ST-30B_R2.fastq
gunzip -c M_18_3761_S5-E-ST-33B_D709-D503_L003_R1_001_val_1.fq.gz > S5-E-ST-33B_R1.fastq
gunzip -c M_18_3761_S5-E-ST-33B_D709-D503_L003_R2_001_val_2.fq.gz > S5-E-ST-33B_R2.fastq
gunzip -c M_18_3768_S5-E-ST-36B_D704-D504_L004_R1_001_val_1.fq.gz > S5-E-ST-36B_R1.fastq
gunzip -c M_18_3768_S5-E-ST-36B_D704-D504_L004_R2_001_val_2.fq.gz > S5-E-ST-36B_R2.fastq
gunzip -c M_18_3807_S5-P-ST-30B_D711-D504_L007_R1_001_val_1.fq.gz > S5-P-ST-30B_R1.fastq
gunzip -c M_18_3807_S5-P-ST-30B_D711-D504_L007_R2_001_val_2.fq.gz > S5-P-ST-30B_R2.fastq
gunzip -c M_18_3781_S5-P-ST-33B_D706-D505_L005_R1_001_val_1.fq.gz > S5-P-ST-33B_R1.fastq
gunzip -c M_18_3781_S5-P-ST-33B_D706-D505_L005_R2_001_val_2.fq.gz > S5-P-ST-33B_R2.fastq
gunzip -c M_18_3787_S5-P-ST-36B_D701-D506_L005_R1_001_val_1.fq.gz > S5-P-ST-36B_R1.fastq
gunzip -c M_18_3787_S5-P-ST-36B_D701-D506_L005_R2_001_val_2.fq.gz > S5-P-ST-36B_R2.fastq
gunzip -c M_18_3755_S6-E-ST-30B_D703-D503_L002_R1_001_val_1.fq.gz > S6-E-ST-30B_R1.fastq
gunzip -c M_18_3755_S6-E-ST-30B_D703-D503_L002_R2_001_val_2.fq.gz > S6-E-ST-30B_R2.fastq
gunzip -c M_18_3762_S6-E-ST-33B_D710-D503_L003_R1_001_val_1.fq.gz > S6-E-ST-33B_R1.fastq
gunzip -c M_18_3762_S6-E-ST-33B_D710-D503_L003_R2_001_val_2.fq.gz > S6-E-ST-33B_R2.fastq
gunzip -c M_18_3769_S6-E-ST-36B_D705-D504_L004_R1_001_val_1.fq.gz > S6-E-ST-36B_R1.fastq
gunzip -c M_18_3769_S6-E-ST-36B_D705-D504_L004_R2_001_val_2.fq.gz > S6-E-ST-36B_R2.fastq
gunzip -c M_18_3775_S6-P-ST-30B_D712-D504_L004_R1_001_val_1.fq.gz > S6-P-ST-30B_R1.fastq
gunzip -c M_18_3775_S6-P-ST-30B_D712-D504_L004_R2_001_val_2.fq.gz > S6-P-ST-30B_R2.fastq
gunzip -c M_18_3782_S6-P-ST-33B_D707-D505_L005_R1_001_val_1.fq.gz > S6-P-ST-33B_R1.fastq
gunzip -c M_18_3782_S6-P-ST-33B_D707-D505_L005_R2_001_val_2.fq.gz > S6-P-ST-33B_R2.fastq
gunzip -c M_18_3809_S6-P-ST-36B_D702-D506_L007_R1_001_val_1.fq.gz > S6-P-ST-36B_R1.fastq
gunzip -c M_18_3809_S6-P-ST-36B_D702-D506_L007_R2_001_val_2.fq.gz > S6-P-ST-36B_R2.fastq
gunzip -c M_18_3756_S7-E-ST-30B_D704-D503_L002_R1_001_val_1.fq.gz > S7-E-ST-30B_R1.fastq
gunzip -c M_18_3756_S7-E-ST-30B_D704-D503_L002_R2_001_val_2.fq.gz > S7-E-ST-30B_R2.fastq
gunzip -c M_18_3763_S7-E-ST-33B_D711-D503_L003_R1_001_val_1.fq.gz > S7-E-ST-33B_R1.fastq
gunzip -c M_18_3763_S7-E-ST-33B_D711-D503_L003_R2_001_val_2.fq.gz > S7-E-ST-33B_R2.fastq
gunzip -c M_18_3770_S7-E-ST-36B_D706-D504_L004_R1_001_val_1.fq.gz > S7-E-ST-36B_R1.fastq
gunzip -c M_18_3770_S7-E-ST-36B_D706-D504_L004_R2_001_val_2.fq.gz > S7-E-ST-36B_R2.fastq
gunzip -c M_18_3776_S7-P-ST-30B_D701-D505_L004_R1_001_val_1.fq.gz > S7-P-ST-30B_R1.fastq
gunzip -c M_18_3776_S7-P-ST-30B_D701-D505_L004_R2_001_val_2.fq.gz > S7-P-ST-30B_R2.fastq
gunzip -c M_18_3783_S7-P-ST-33B_D708-D505_L005_R1_001_val_1.fq.gz > S7-P-ST-33B_R1.fastq
gunzip -c M_18_3783_S7-P-ST-33B_D708-D505_L005_R2_001_val_2.fq.gz > S7-P-ST-33B_R2.fastq
gunzip -c M_18_3788_S7-P-ST-36B_D703-D506_L005_R1_001_val_1.fq.gz > S7-P-ST-36B_R1.fastq
gunzip -c M_18_3788_S7-P-ST-36B_D703-D506_L005_R2_001_val_2.fq.gz > S7-P-ST-36B_R2.fastq

```