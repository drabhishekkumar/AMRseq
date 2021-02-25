### Requirements
#### bwa: https://github.com/lh3/bwa
#### samtools
#### bcftools

#### Reference genome

#### Sequencing reads

### BWA mapping of reads to reference genome - MTB2020.fasta
bwa mem -t 8 -x ont2d MTB2020.fasta ONT_1.fastq>ONT_1.sam

### SAM to BAM conversion
samtools view -hSbo ONT_0.bam ONT_0.sam

### BAM file sorting
samtools sort ONT_1.bam -o ONT_1.sorted.bam

### BAM file indexing
samtools index ONT_1.sorted.bam

### Variant calling using bcftools
~/BioBin/bcftools-1.6/bcftools mpileup -Ovu  -f MTB2020.fasta ONT_1.sorted.bam|~/BioBin/bcftools-1.6/bcftools call --ploidy 1 -vm -Ov>ONT_1.vcf
