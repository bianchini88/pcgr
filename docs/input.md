## Input

The PCGR workflow accepts two types of input files:

  * An unannotated, single-sample VCF file (>= v4.2) with called somatic variants (SNVs/InDels)
  * A copy number segment file


PCGR can be run with either or both of the two input files present.

### VCF

* We __strongly__ recommend that the input VCF is compressed and indexed using [bgzip](http://www.htslib.org/doc/tabix.html) and [tabix](http://www.htslib.org/doc/tabix.html)
* If the input VCF contains multi-allelic sites, these will be subject to [decomposition](http://genome.sph.umich.edu/wiki/Vt#Decompose)
* Variants used for reporting should be designated as 'PASS' in the VCF FILTER column

__IMPORTANT NOTE 1__: Considering the VCF output for the [numerous somatic SNV/InDel callers](https://www.biostars.org/p/19104/) that have been developed, we have a experienced a general lack of uniformity and robustness for the representation of somatic variant genotype data (e.g. variant allelic depths (tumor/normal), genotype quality etc.). Variant genotype data found as INFO tags in the input VCF can be specified as optional arguments to the PCGR workflow, which in turn can be used for interactive exploration in the tumor report.

__IMPORTANT NOTE 2__: PCGR generates a number of VCF INFO annotation tags that is appended to the query VCF. We will therefore encourage the users to submit query VCF files that have not been subject to annotations by other means, but rather a VCF file that comes directly from variant calling. If not, there are likely to be INFO tags in the query VCF file that coincide with those produced by PCGR.

### Copy number segments

The tab-separated values file with copy number aberrations __MUST__ contain the following four columns:

* Chromosome
* Start
* End
* Segment_Mean

Here, _Chromosome_, _Start_, and _End_ denote the chromosomal segment, and __Segment_Mean__ denotes the log(2) ratio for a particular segment, which is a common output of somatic copy number alteration callers. Note that coordinates must be **one-based** (i.e. chromosomes start at 1, not 0). Below shows the initial part of a copy number segment file that is formatted correctly according to PCGR's requirements:

      Chromosome	Start	End	Segment_Mean
      1 3218329 3550598 0.0024
      1 3552451 4593614 0.1995
      1 4593663 6433129 -1.0277

### CPSR report

One may feed the contents of a CPSR analysis (https://github.com/sigven/cpsr) with PCGR through the `--cpsr_report` option. This will result in a dedicated germline section of the PCGR report. Note that one should here input the compressed JSON file that is output by CPSR.
