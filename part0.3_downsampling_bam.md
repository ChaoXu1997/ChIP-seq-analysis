### Downsampling reads to a certain number

When compare different ChIP-seq data sets or analyze a set of ChIP-seq data sets together (e.g. ChromHMM analysis), it is desirable 
to subsample the deeply sequenced ones to a certain number of reads (say 15million or 30 million).

In paper [Integrative analysis of 111 reference human epigenomes](http://www.nature.com/nature/journal/v518/n7539/full/nature14248.html):

>To avoid artificial differences in signal strength due to differences in sequencing
depth, all consolidated histone mark data sets (except the additional histone marks
the seven deeply profiled epigenomes, Fig. 2j) were uniformly subsampled to a
**maximum depth of 30 million reads** (the median read depth over all consolidated
samples). For the seven deeply profiled reference epigenomes (Fig. 2j), histone mark
data sets were subsampled to a maximum of 45 million reads (median depth). The
consolidated DNase-seq data sets were subsampled to a maximum depth of 50
million reads (median depth). **These uniformly subsampled data sets were then used
for all further processing steps (peak calling, signal coverage tracks, chromatin states)**.

After reading several posts [here](https://www.biostars.org/p/76791/) and [here](https://groups.google.com/forum/#!topic/bedtools-discuss/gf0KeAJN2Cw).
It seems `samtools` and `sambamba` are the tools to use, but they are both output a proportion number of reads. 

```bash
time samtools view -s 3.6 -b my.bam -o subsample.bam
time sambamba view -f bam -t 10 --subsampling-seed=3 -s 0.6 my.bam -o subsample.bam
```
`-s 3.6` set seed of 3 and %60 of the reads by samtools.

If one wants to get say 15 million reads, one needs to do `samtools flag stat` or `samtools idxstats` to get the total number of reads,
and then calculate the proportion by:  `15 million/total = proportion`. Finally, feed the proportion to `-s` flag.

