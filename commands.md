# RNA Seq data processing

Initial steps including reference building, alignment and normalization

## Indexing Reference file

`python hisat2/hisat2-build GRCh38_latest_genomic.fna ./index/ref`

## Aligning

`../hisat2/hisat2 -x ref -1 ../reads/MCF7_FRG1_Ex_R1_S3_R1_001.fastq -2 ../reads/MCF7_FRG1_Ex_R1_S3_R2_001.fastq -S aligned_FRG1.sam`
`../hisat2/hisat2 -x ref -1 ../reads/MCF7_PLX_R1_S6_R1_001.fastq -2 ../reads/MCF7_PLX_R1_S6_R2_001.fastq -S aligned_PLX.sam`

## Converting .sam to .bam

We use samtools for this task
`../samtools-1.19.2/samtools view -Sb -o aligned_PLX.bam aligned_PLX.sam`
`../samtools-1.19.2/samtools view -Sb -o aligned_FRG1.bam aligned_FRG1.sam`

## Sorting .bam file

`../samtools-1.19.2/samtools sort aligned_PLX.bam -o aligned_PLX_sorted.bam`
`../samtools-1.19.2/samtools sort aligned_FRG1.bam -o aligned_FRG1_sorted.bam`

## Normalization

`./stringtie/stringtie ./aligned/aligned_FRG1_sorted.bam -o stringtie_FRG1_output.gtf -eG GRCh38_latest_genomic.gff`
`./stringtie/stringtie ./aligned/aligned_PLX_sorted.bam -o stringtie_PLX_output.gtf -eG GRCh38_latest_genomic.gff`

### Merging the gtfs

`stringtie/stringtie --merge -G GRCh38_latest_genomic.gff -o stringtie_merged.gtf GTFList.txt`
