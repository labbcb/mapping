Practical Mapping RNA-Seq Data
====================

```reference=dmel56.fa```
bowtie-build ${reference} ${reference_index}

config_dir=~/Mapping/config
reference_index=${config_dir}/dmel56
transcript_annotation=${config_dir}/Drosophila_melanogaster.BDGP5.75.gtf
reads1=data/S2-DRSC-14-PE/081121_S2-DRSC-14-1_1_sequence.txt
reads2=data/S2-DRSC-14-PE/081121_S2-DRSC-14-1_2_sequence.txt
output=sample1

du -h ${reads1}

tophat -p 2 -G ${transcript_annotation} --transcriptome-index ${config_dir}  -o ${output} --no-novel-juncs ${reference_index} ${reads1},${reads2}

samtools sort ${output}/accepted_hits.bam ${output}/accepted_hits.sorted
samtools index ${output}/accepted_hits.sorted.bam

## transcript
reference=${config_dir}/dmeltran56.fa

## if index is needed
bwa index ${reference}

## will reuse the reads and output dir
bwa mem ${reference} ${reads1} ${reads2} > ${output}.sam

samtools import ${reference}.fai ${output}.sam ${output}.unsorted.bam
samtools sort ${output}.unsorted.bam ${output}
samtools index ${output}.bam
