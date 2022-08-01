###Question 2

Indexed the genome file 
>bwa index toy_genome.fa

I did the same analysis on baseline reads as well as other reads (that has copy gain)

##BaselineReads:
I used bwa-mem tool to map/align the baseline reads to mock genome, and used following command
>bwa mem toy_genome.fa baseline_reads_R1.fastq.gz baseline_reads_R2.fastq.gz > bwamemOutBaseLine.sam

The output from this was alignment file was in SAM format

Next I extracted those reads from the alignment file that overlapped the given annotation of genes in bed file using samtools
>samtools view -L toy_genome_genes.bed  bwamemOutBaseLine.sam > baselinetest_readsOverlappedWithGenes.sam

I then converted sam file to bam file using samtools because samtool -depth function (using next) requires files to be in bam format
>samtools import toy_genome.fa.fai baselinetest_readsOverlappedWithGenes.sam baselinetest_readsOverlappedWithGenes.bam

Sorted bam file next, using samtools again
>samtools sort baselinetest_readsOverlappedWithGenes.bam -o baselinetest_readsOverlappedWithGenes_sorted.bam

#computed the read depth at each position using samtools depth
>samtools depth -b toy_genome_genes.bed baselinetest_readsOverlappedWithGenes_sorted.bam > refGenes_BaselineBam.depth



#Did exact same steps with other reads
##sampleReads:
I used bwa-mem tool to map/align the reads to mock genome, and used following command
>bwa mem toy_genome.fa reads_R1.fastq.gz reads_R2.fastq.gz > bwamemOutSamples.sam

The output from this was alignment file was in SAM format
Next I extracted those reads from samples from the alignment file that overlapped the given annotation of genes in bed file using samtools
>samtools view -L toy_genome_genes.bed  bwamemOutSamples.sam > baselinetestSamples_readsOverlappedWithGenes.sam

I then converted sam file to bam file using samtools because samtool -depth function (using next) requires files to be in bam format
>samtools import toy_genome.fa.fai baselinetestSamples_readsOverlappedWithGenes.sam baselinetest_readsOverlappedWithGenes.bam

Sorted bam file next, using samtools again
>samtools sort testSample_readsOverlappedWithGenes.bam -o testSample_readsOverlappedWithGenes_sorted.bam

#computed the read depth at each position using samtools depth
>samtools depth -b toy_genome_genes.bed testSample_readsOverlappedWithGenes_sorted.bam > refGenes_samplesBam.depth


Once I had the depth information from both baseline and sample read files, I went ahead and imported data in R, to do some visualization and calculated copy number from this reads depth information
