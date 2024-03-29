### Filtering Illumina sequences

The following pipeline was used to filter Illumina sequences of two sets of data: (1) whole genomic DNA (gDNA); and (2) probe of entire chromosome (obtained by flow sorting and fragmented by a DOP-PCR reaction). Both samples from the species *Holochilus nanus* (2n = 56, NF = 56), a Neotropical rodent of tribe Oryzomyini.

**Softwares used:**

[FastQC v0.10.1](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/).

[BBMap_38.49](https://jgi.doe.gov/data-and-tools/software-tools/bbtools/). Bushnell B, Rood J, Singer E (2017) BBMerge–accurate paired shotgun read merging via overlap. PloS one, 12(10),e0185056.

[Trimmomatic-0.39](http://www.usadellab.org/cms/?page=trimmomatic). Bolger AM, Lohse M, Usadel B (2014) Trimmomatic: A flexible trimmer for Illumina Sequence Data. Bioinformatics, 30(15):2114–2120.

**Whole genomic DNA:**

The filtering was performed using Trimmomatic with the set of Illumina adapters from BBMap. FastQC was used to verify the libraries quality and set the parameters to be used.

`java -jar Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 read_r1.fastq read_r2.fastq out_r1_filtered_paired.fq.gz out_r1_filtered_unpaired.fq.gz out_r2_filtered_paired.fq.gz out_r2_filtered_unpaired.fq.gz ILLUMINACLIP:/bbmap/resources/adapters.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 HEADCROP:15 MINLEN:95`

**Probe of entire chromosome:**

This library was process by a DOP-PCR reaction prior to sequencing, resulting in more fragmented sequences harbouring DOP-PCR primer sequence (6MW - 5' CCGACTCGAGNNNNNNATGTGG 3'). The filtering was also performed using Trimmomatic with the set of Illumina adapters from BBMap, but was performed in three steps: (1) filtering of Illumina adapters; (2) filtering of 6MW sequence and quality patterns; (3) cut of the first thirty bases from the start of the read. In order to filter 6MW sequence, a file containing this sequence was included in the directory Trimmomatic-0.39/adapters/. FastQC was used to verify the libraries quality and set the parameters to be used.

- Step 1:

`java -jar Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 read_r1.fastq read_r2.fastq out1_r1_filtered_paired.fq.gz out1_r1_filtered_unpaired.fq.gz out1_r2_filtered_paired.fq.gz out1_r2_filtered_unpaired.fq.gz ILLUMINACLIP:/bbmap/resources/adapters.fa:2:30:10`

- Step 2:

`java -jar Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 out1_r1_filtered_paired.fq out1_r2_paired.fq out2_r1_filtered_paired.fq.gz out2_r1_filtered_unpaired.fq.gz out2_r2_filtered_paired.fq.gz out2_r2_filtered_unpaired.fq.gz ILLUMINACLIP:Trimmomatic-0.39/adapters/6MW-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:95`

- Step 3:

`java -jar Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 out2_r1_filtered_paired.fq out2_r2_filtered_paired.fq out3_r1_filtered_paired.fq out3_r1_filtered_unpaired.fq out3_r2_filtered_paired.fq out3_r2_filtered_unpaired.fq HEADCROP:30 MINLEN:95`

**Parameters used:**

- phred33: specifies the base quality encoding.
- ILLUMINACLIP: will cut the adapters and other illumina-specific sequences from the read.
- LEADING: will cut bases off the start of a read, if below a threshold quality.
- TRAILING: will cut bases off the end of a read, if below a threshold quality.
- SLIDINGWINDOW: will performs a sliding window trimming approach. It starts scanning at the 5‟ end and clips the read once the average quality within the window falls below a threshold.
- HEADCROP: will cut the specified number of bases from the start of the read.
- MINLEN: will drop the read if it is below a specified length.

For more details see [supplementary information](http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/TrimmomaticManual_V0.32.pdf).
