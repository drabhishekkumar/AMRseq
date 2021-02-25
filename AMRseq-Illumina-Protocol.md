
# Illumina reads-based AMRseq pipeline

Post quality control and assurance, Illumina reads are mapped to reference pathogenic bacterial genome for example
Mycobacterium tuberculosis H37Rv genome.

### Software Requirements

#### bwa: https://github.com/lh3/bwa
#### samtools: http://www.htslib.org/
#### bcftools: http://www.htslib.org/

### Software Installation using Anaconda

conda install -y bwa samtools bcftools

### Reference genome

Herein, I have used Mycobacterium tuberculosis H37Rv genome as the reference genome
Download reference genome from NCBI for genome assembly: ASM19595v2
RefSeq assembly accession: GCF_000195955.2
https://www.ncbi.nlm.nih.gov/assembly/GCF_000195955.2/

FTP link: https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/195/955/GCF_000195955.2_ASM19595v2/

### Downlaod GCF_000195955.2_ASM19595v2_genomic.fna.gz  

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/195/955/GCF_000195955.2_ASM19595v2/GCF_000195955.2_ASM19595v2_genomic.fna.gz

### Unzip GCF_000195955.2_ASM19595v2_genomic.fna.gz as GCF_000195955.2_ASM19595v2_genomic.fna

gunzip GCF_000195955.2_ASM19595v2_genomic.fna.gz  

### Give reference genome a short name like reference genome (optional) like MTB2020.fasta

cp GCF_000195955.2_ASM19595v2_genomic.fna MTB2020.fasta

### Indexing reference genome - MTB2020.fasta for BWA-based mapping

bwa index MTB2020.fasta

### BWA mapping of Illumina paired reads to reference genome - MTB2020.fasta

bwa mem -t 8 MTB2020.fasta Illumina_R1.fastq.gz Illumina_R2.fastq.gz>Illumina.sam

### Illumina-based SAM to BAM conversion

samtools view -hSbo Illumina.bam Illumina.sam

### Illumina-based BAM file sorting

samtools sort Illumina.bam -o Illumina.sorted.bam

### Illumina-based BAM file indexing

samtools index Illumina.sorted.bam

### Variant calling using bcftools

/Users/abhishek/BioBin/bcftools-1.6/bcftools mpileup -Ovu -f ../MTB2020.fasta Illumina.sorted.bam | /Users/abhishek/BioBin/bcftools-1.6/bcftools call --ploidy 1 -vm -Ov > Illumina_variants.vcf
