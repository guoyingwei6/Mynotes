---
tags:
  - snp
---
## 设置变量

```shell
# /bin/bash

# define variable
export sm=${1}
export fq1=${2}
export fq2=${3}
export ref=Oar4.0_add_CPY.fa
```

##  mapping and sort

```shell
# mapping and sort 
bwa mem \
-t 4 \
-R '@RG\tID:'${sm}'\tLB:'${sm}'\tPL:ILLUMINA\tSM:'${sm} \
${ref} \
${fq1} \
${fq2} \
| samtools sort -@ 4 -o ${sm}.sort.bam -

```

## 去除PCR重复

```shell
# dedup
gatk MarkDuplicates \
--spark-runner LOCAL \
-I ${sm}.sort.bam \
-O ${sm}.rmdup.bam \
-M ${sm}.dup_metrics.txt \
--CREATE_INDEX true \
--VALIDATION_STRINGENCY SILENT \
--REMOVE_DUPLICATES true
```

或者用`picard`也可以：
```shell
# dedup
/stor9000/apps/appsoftware/BioSoftware/bin/java \
-Xmx10g \
-Djava.io.tmpdir=/stor9000/apps/users/NWSUAF/2015010726/files/tmp \
-Dpicard.useLegacyParser=false \
-jar /stor9000/apps/users/NWSUAF/2015060152/bin/picard_2.18.17.jar \
MarkDuplicates \
-I ${sm}.sort.bam \
-O ${sm}.dedup.bam \
-ASSUME_SORT_ORDER coordinate \
-METRICS_FILE ${sm}.dedup.txt \
-VALIDATION_STRINGENCY LENIENT

# index bam
/stor9000/apps/users/NWSUAF/2014010784/software/samtools/samtools-1.12/samtools index ${sm}.dedup.bam


```