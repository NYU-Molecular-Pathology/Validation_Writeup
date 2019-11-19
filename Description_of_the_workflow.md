# Description of the Workflow

![](data_workflow.png)

After DNA sequencing, data is transferred to NYU's bigpurple compute cluster where [automated programs](https://github.com/NYU-Molecular-Pathology/lyz) and [scripts](https://github.com/NYU-Molecular-Pathology/protocols) submit the sequence reads for demultiplexing. This is followed by analysis in the targeted exome pipeline using two programs; the [demux-nf](https://github.com/NYU-Molecular-Pathology/demux-nf) program which demultiplexes the sequence reads and the [NGS580-nf](https://github.com/NYU-Molecular-Pathology/NGS580-nf) which implements a standard exome variant calling pipeline tailored for bigpurple's software environment. It also includes extra analysis and reporting steps customized for usage in the NGS580 gene panel.

## Pipeline Description

![](pipline_workflow.png)

After demultiplexing, low quality bases are trimmed by Trimmomatic. Reads are then aligned to the hg19 reference genome using BWA MEM, and passed through Sambamba for quality filtering and deduplication. Reads are then analyzed for quality metrics with custom R scripts, and depth of coverage at target regions with GATK DepthOfCoverage. These reads are then used in copy number variant analysis using a custom pipeline built around CNVkit, and are passed through a standard GATK pipeline for recalibration and realignment before being used in variant calling with GATK HaplotypeCaller (unpaired samples) and MuTect2 (tumor-normal sample pairs), along with LoFreq for high sensitivity variant calling of unpaired samples.

Variant calls are annotated with ANNOVAR, and all results are aggregated in a custom report which delivers a summary of analysis results, metrics, and files via email for clinical review.
