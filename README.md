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