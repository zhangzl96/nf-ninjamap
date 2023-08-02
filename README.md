Sample scripts
====================

The simple scripts shows how to run ninjamap jobs under the Nextflow framework.

## Command line example for a single sample in local mode
```{bash}
nextflow run \
main-local.nf \
--reads1 's3://nextflow-pipelines/nf-ninjamap/data/read1.fastq.gz' \
--reads2 's3://nextflow-pipelines/nf-ninjamap/data/read2.fastq.gz' \
--db HCom2_20221117 \
--db_prefix HCom2 \
--output_path 's3://genomics-workflow-core/Results/Ninjamap/local' \
-profile docker
```

## Test dataset
The test dataset is available in the data directory
```{bash}
data/read1.fastq.gz
data/read2.fastq.gz
```

## An example of ninjaMap Index (HCom2) is available at
```{bash}
https://zenodo.org/record/7872423/files/hCom2_20221117.ninjaIndex.tar.gz
```

## Run a ninjaMap docker container with a sorted BAM file (a sample aligned to the concatenated reference) and an HCom2 binmap file
```{bash}
docker container run \
    -v /host/path/to/indata/:/input_data/ \
    -v /host/path/to/outdata/:/output_data/ \
    fischbachlab/nf-ninjamap \
    python /work/scripts/ninjaMap_parallel_5.py \
    -bin /input_data/db/HCom2.ninjaIndex.binmap.csv \
    -bam /input_data/bam/sample.sortedByCoord.bam \
    -outdir /output_data/summary \
    -prefix HCom2
```

## Seedfile example
### Note that the seedfile is a CSV (comma-separated values) file with header
### The format of the seedfile is sample_name,short_R1,short_R2

```{bash}
sampleName,R1,R2
sample_name,s3://nextflow-pipelines/nf-ninjamap/data/read1.fastq.gz,s3://nextflow-pipelines/nf-ninjamap/data/read2.fastq.gz
```

## Example 1: aws batch job
```{bash}
aws batch submit-job \
  --job-name nf-ninjamap \
  --job-queue priority-maf-pipelines \
  --job-definition nextflow-production \
  --container-overrides command="fischbachlab/nf-ninjamap, \
"--seedfile", "s3://genomics-workflow-core/Results/ninjamap/example_seedfile.csv", \
"--db","SCv2_4_20210212", \
"--db_prefix", "SCv2_4", \
"--db_path", "s3://maf-versioned/ninjamap/Index", \
"--output_path", "s3://genomics-workflow-core/Results/Ninjamap/biohub" "
```

## Example 2: aws batch job
```{bash}
aws batch submit-job \
    --job-name nf-ninjamap \
    --job-queue priority-maf-pipelines \
    --job-definition nextflow-production \
    --container-overrides command="fischbachlab/nf-ninjamap, \
"--seedfile", "s3://genomics-workflow-core/Results/ninjamap/example_seedfile.csv", \
"--db","SCv2_6_20210518", \
"--db_prefix", "SCv2_6", \
"--db_path", "s3://maf-versioned/ninjamap/Index", \
"--output_path", "s3://genomics-workflow-core/Results/Ninjamap/" "
```

## Example 3: aws batch job against the latest HCom2 database using a seedfile
### Example s3 db path: s3://maf-versioned/ninjamap/Index/
### External user can supply the following URL if running the latest hCom2 index

```{bash}
 https://zenodo.org/record/7872423/files/hCom2_20221117.ninjaIndex.tar.gz
```{bash}

```{bash}
aws batch submit-job \
    --job-name nf-ninjamap \
    --job-queue priority-maf-pipelines \
    --job-definition nextflow-production \
    --container-overrides command="fischbachlab/nf-ninjamap, \
"--seedfile", "s3://genomics-workflow-core/Results/Ninjamap/20221018/seedfile2.csv", \
"--db","HCom2_20221117", \
"--db_prefix", "HCom2", \
"--db_path", "https://zenodo.org/record/7872423/files/hCom2_20221117.ninjaIndex.tar.gz", \
"--output_path", "s3://genomics-workflow-core/Results/Ninjamap/HCom2/20221018", \
"--sampleRate", "0.5" "
```


Output files for each sample
====================

The output files are organized into 4 folders.

## bowtie2 folder

The alignment file of all input reads aligned the defined community database in the bam format

## Logs folder

The running logs of various scripts

## ninjaMap folder

1. **\*.ninjaMap.abundance.csv**: this file shows the statistics of the abundance, coverage and depth of each strain in the defined community

+ Strain_Name: strain name<br>
+ Read_Fraction: the abundance in the defined community in percentage<br>
+ Percent_Coverage: the average coverage per strain in percentage<br>
+ Coverage_Depth: the average coverage depth<br>

Sample abundance output
```{bash}
Strain_Name                                        Read_Fraction       Percent_Coverage  Coverage_Depth
Acidaminococcus-fermentans-DSM-20731-MAF-2         0.0                 0                 0
Acidaminococcus-sp-D21-MAF-2                       0.0                 0                 0
Adlercreutzia-equolifaciens-DSM-19450              0.0                 0                 0
Akkermansia-muciniphila-ATCC-BAA-835-MAF-2         6.521739130434782   0.0376527         0.0004
Alistipes-finegoldii-DSM-17242                     0.0                 0                 0
Alistipes-ihumii-AP11-MAF-2                        0.0                 0                 0
Alistipes-indistinctus-YIT-12060-DSM-22520-MAF-2   0.0                 0                 0
Alistipes-onderdonkii-DSM-19147-MAF-2              1.4492753623188406  0.00439328        0.0001
Alistipes-putredinis-DSM-17216-MAF-2               0.0                 0                 0
Alistipes-senegalensis-JC50-DSM-25460-MAF-2        0.0                 0                 0
Alistipes-shahii-WAL-8301-DSM-19121-MAF-2          0.0                 0                 0
Anaerofustis-stercorihominis-DSM-17244             0.0                 0                 0
Anaerostipes-caccae-DSM-14662-MAF-2                0.0                 0                 0
Anaerotruncus-colihominis-DSM-17241-MAF-2          0.0                 0                 0
Bacteroides-caccae-ATCC-43185-MAF-2                6.521739130434782   0.0387897         0.0004
Bacteroides-cellulosilyticus-DSM-14838-MAF-2       22.463768115942027  0.0529593         0.000599636
Bacteroides-coprocola-DSM-17136-MAF-2              2.898550724637681   0.0109683         9.75984e-05

```


2. **\*.ninjaMap.read_stats.csv**: this file shows the statistics of input reads

+ File_Name: sample name <br>
+ Reads_Aligned: the number of aligned reads<br>
+ Reads_wPerfect_Aln: the number of perfectly aligned reads<br>
+ Reads_wSingular_Votes: the number of reads voted as singular<br>
+ Reads_wEscrowed_Votes: the number of reads voted as escrow<br>
+ Discarded_Reads_w_Perfect_Aln: the number of discarded perfectly aligned reads

3. **\*.ninjaMap.strain_stats.csv**: this file shows the various statistics of each strains

4. **\*.ninjaMap.votes.csv.gz**: the statistics of reads voting (singular or escrow)

## Stats folder
1. **adapter_trimming_stats_per_ref.txt**: this file shows the statistics of adapter trimming

2. **read_accounting.csv**: this file shows the statistics shows the total number of reads, the number of reads after trimming and the number of aligned reads


Aggregated output files for each study
====================

The aggregated output files are organized into 6 files.

1. **\*.covDepth.csv**: this file shows the average coverage depth per strain by samples
2. **\*.host_contaminants.csv**: this file shows the detected host contaminants (Human or Mouse) by samples if the unalignment rate is over 5%
3. **\*.long.csv**: this is the long format of three files (\*.readFraction.csv, \*.covDepth.csv and \*.percCoverage.csv)
4. **\*.percCoverage.csv**: this file shows the average coverage per strain in percentage by samples
5. **\*.reads_stats.csv**: this file shows the reads statistics in read numbers by samples
6. **\*.readFraction.csv**: this file shows the abundance in the defined community in percentage by samples
