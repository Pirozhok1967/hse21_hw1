# Домашнее задание №1
<h3>Пирожкина Мария группа 4</h3>
<h2>Задание 1</h2>
<h4>Список команд выполненных на сервере</h4>
ls -1 /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}

seqtk sample -s515 oil_R1.fastq 5000000 > sub_R1.fq
seqtk sample -s515 oil_R2.fastq 5000000 > sub_R2.fq
seqtk sample -s515 oilMP_S4_L001_R1_001.fastq 1500000 > sub_MP_R1.fq
seqtk sample -s515 oilMP_S4_L001_R2_001.fastq 1500000 > sub_MP_R2.fq

mkdir fastqc
ls sub* | xargs -tI{} fastqc -o fastqc {}
mkdir multiqc
multiqc -o multiqc fastqc

platanus_trim sub_R1.fq sub_R2.fq
platanus_internal_trim sub_MP_R1.fq sub_MP_R2.fq

rm sub_R1.fq
rm sub_R2.fq
rm sub_MP_R1.fq
rm sub_MP_R2.fq

mkdir fastqc_trimmed
ls sub* | xargs -tI{} fastqc -o fastqc_trimmed {}
mkdir multiqc_trimmed
multiqc -o multiqc_trimmed fastqc_trimmed

platanus assemble -f sub_R1.fq.trimmed sub_R2.fq.trimmed 2> logfile.log
platanus scaffold -c out_contig.fa -IP1 sub_R1.fq.trimmed sub_R2.fq.trimmed -OP2 sub_MP_R1.fq.int_trimmed sub_MP_R2.fq.int_trimmed 2> scaffold.log
platanus gap_close -c out_scaffold.fa -IP1 sub_R1.fq.trimmed sub_R2.fq.trimmed -OP2 sub_MP_R1.fq.int_trimmed sub_MP_R2.fq.int_trimmed 2> gapclose.log

head out_gapClosed.fa
echo scaffold1_cov231 > seq_names.lst
seqtk subseq out_gapClosed.fa seq_names.lst > longest.fa

<h2>Задание 2</h2>
<h4>Список команд выполненных на сервере</h4>
seqtk sample -s515 oil_R1.fastq 3750000 > sub_75_R1.fq
seqtk sample -s515 oil_R1.fastq 2500000 > sub_50_R1.fq
seqtk sample -s515 oil_R1.fastq 1250000 > sub_25_R1.fq
seqtk sample -s515 oil_R2.fastq 3750000 > sub_75_R2.fq
seqtk sample -s515 oil_R2.fastq 2500000 > sub_50_R2.fq
seqtk sample -s515 oil_R2.fastq 1250000 > sub_25_R2.fq
seqtk sample -s515 oilMP_S4_L001_R1_001.fastq 1125000 > sub_MP_75_R1.fq
seqtk sample -s515 oilMP_S4_L001_R1_001.fastq 750000 > sub_MP_50_R1.fq
seqtk sample -s515 oilMP_S4_L001_R1_001.fastq 375000 > sub_MP_25_R1.fq
seqtk sample -s515 oilMP_S4_L001_R2_001.fastq 1125000 > sub_MP_75_R2.fq
seqtk sample -s515 oilMP_S4_L001_R2_001.fastq 750000 > sub_MP_50_R2.fq
seqtk sample -s515 oilMP_S4_L001_R2_001.fastq 375000 > sub_MP_25_R2.fq

platanus_trim sub_75_R1.fq sub_75_R2.fq
platanus_internal_trim sub_MP_75_R1.fq sub_MP_75_R2.fq
platanus_trim sub_50_R1.fq sub_50_R2.fq
platanus_internal_trim sub_MP_50_R1.fq sub_MP_50_R2.fq
platanus_trim sub_25_R1.fq sub_25_R2.fq
platanus_internal_trim sub_MP_25_R1.fq sub_MP_25_R2.fq
platanus assemble -o 75 -f sub_75_R1.fq.trimmed sub_75_R2.fq.trimmed 2> logfile.log
platanus assemble -o 50 -f sub_50_R1.fq.trimmed sub_50_R2.fq.trimmed 2> logfile.log
platanus assemble -o 25 -f sub_25_R1.fq.trimmed sub_25_R2.fq.trimmed 2> logfile.log
platanus scaffold -o 75 -c 75_out_contig.fa -IP1 sub_75_R1.fq.trimmed sub_75_R2.fq.trimmed -OP2 sub_MP_75_R1.fq.int_trimmed sub_MP_75_R2.fq.int_trimmed 2> scaffold.log
platanus scaffold -o 50 -c 50_out_contig.fa -IP1 sub_50_R1.fq.trimmed sub_50_R2.fq.trimmed -OP2 sub_MP_50_R1.fq.int_trimmed sub_MP_50_R2.fq.int_trimmed 2> scaffold.log
platanus scaffold -o 25 -c 25_out_contig.fa -IP1 sub_25_R1.fq.trimmed sub_25_R2.fq.trimmed -OP2 sub_MP_25_R1.fq.int_trimmed sub_MP_25_R2.fq.int_trimmed 2> scaffold.log
platanus gap_close -o 75 -c 75_out_scaffold.fa -IP1 sub_75_R1.fq.trimmed sub_75_R2.fq.trimmed -OP2 sub_MP_75_R1.fq.int_trimmed sub_MP_75_R2.fq.int_trimmed 2> gapclose.log
platanus gap_close -o 50 -c 50_out_scaffold.fa -IP1 sub_50_R1.fq.trimmed sub_50_R2.fq.trimmed -OP2 sub_MP_50_R1.fq.int_trimmed sub_MP_50_R2.fq.int_trimmed 2> gapclose.log
platanus gap_close -o 25 -c 25_out_scaffold.fa -IP1 sub_25_R1.fq.trimmed sub_25_R2.fq.trimmed -OP2 sub_MP_25_R1.fq.int_trimmed sub_MP_25_R2.fq.int_trimmed 2> gapclose.log
