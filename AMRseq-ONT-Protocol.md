### Requirements
#### bwa: https://github.com/lh3/bwa
#### samtools: http://www.htslib.org/
#### bcftools: http://www.htslib.org/

#### Reference genome
Herein, I have used Mycobacterium tuberculosis H37Rv genome as the reference genome
Download reference genome from NCBI for
genome assembly: ASM19595v2 R
efSeq assembly accession:     GCF_000195955.2
https://www.ncbi.nlm.nih.gov/assembly/GCF_000195955.2/
FTP link: https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/195/955/GCF_000195955.2_ASM19595v2/

#### Downlaod GCF_000195955.2_ASM19595v2_genomic.fna.gz  
wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/195/955/GCF_000195955.2_ASM19595v2/GCF_000195955.2_ASM19595v2_genomic.fna.gz
#### Unzip GCF_000195955.2_ASM19595v2_genomic.fna.gz as GCF_000195955.2_ASM19595v2_genomic.fna
gunzip GCF_000195955.2_ASM19595v2_genomic.fna.gz  

#### Give reference genome a short name like Reference genome (optional) like MTB2020.fasta
cp GCF_000195955.2_ASM19595v2_genomic.fna MTB2020.fasta

bwa index MTB2020.fasta
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
