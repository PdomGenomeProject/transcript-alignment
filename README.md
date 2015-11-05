# Transcript alignment

This data set is part of the [*Polistes dominula* genome project][pdomproj], and provides details regarding the alignment of assembled transcripts to the *P. dominula* genome, as described in (Standage *et al.*, manuscript in preparation).
Included in this data set are the alignments themselves (in GeneSeqer format), alignment structures suitable for genome annotation workflows (in GFF3 format), and documentation providing complete disclosure of the alignment procedure.

## Data access

The *P. dominula* reference genome assembly is available for download at the DOI [10.6084/m9.figshare.1593187][maskgenomedoi].
Assembled *P. dominula* transcripts are available for download at the DOI [10.6084/m9.figshare.1593168][transdoi]
Transcripts for *Polistes metricus* and *Polistes canadensis* have been included in this data set for convenience.

## Synopsis

*De novo* assembled *P. dominula* transcripts were spliced aligned at high stringency to the repeat-masked genome sequence using [GeneSeqer][] version [2-26-2014][].
*Polistes canadensis* and *P. metricus* transcripts were also aligned with GeneSeqer using less stringent parameters.

## Procedure

First, we set the `GSQDir` variable to the path of the GeneSeqer source code.

```bash
GSQDir=/usr/local/src/GENESEQER
```

Next, we aligned the *P. dominula* transcripts with stringent parameters.

```bash
# P. dominula alignments
MakeArray pdom-tsa-r1.2.fa
GeneSeqerL -s Arabidopsis \
           -L pdom-scaffolds-masked-r1.2.fa \
           -D pdom-tsa-r1.2.fa \
           -O pdom-tsa-r1.2-masked.gsq \
           -p $GSQDIR/data/prmfileHQ \
           -x 30 -y 45 -z 60 -w 0.8 -m 1000000 \
           > pdom-tsa-r1.2-masked-gsq.log 2>&1
```

Then we aligned the *P. canadensis* and *P. metricus* transcripts with less stringent parameters.

```bash
# P. metricus alignments
MakeArray pmet-tsa.fa
GeneSeqerL -s Arabidopsis \
           -L pdom-scaffolds-masked-r1.2.fa \
           -d pmet-tsa.fa \
           -O pmet-tsa-masked.gsq \
           -p $GSQDIR/data/prmfile \
           -x 16 -y 24 -z 48 -w 0.8 -m 1000000 \
           > pmet-tsa-r1.2-masked-gsq.log 2>&1

# P. canadensis alignments
MakeArray pcan-tsa.fa
GeneSeqerL -s Arabidopsis \
           -L pdom-scaffolds-masked-r1.2.fa \
           -d pcan-tsa.fa \
           -O pcan-tsa-masked.gsq \
           -p $GSQDIR/data/prmfile \
           -x 16 -y 24 -z 48 -w 0.8 -m 1000000 \
           > pcan-tsa-masked-gsq.log 2>&1
```

Finally, we converted all of the alignments into GFF3 format, filtering out single-exon alignments and alignments with GeneSeqer similarity or coverage scores < 0.8.

```bash
./gsq2makergff3.py < pdom-tsa-r1.2-masked.gsq > pdom-tsa-masked.gff3
./gsq2makergff3.py < pmet-tsa-masked.gsq > pmet-tsa-masked.gff3
./gsq2makergff3.py < pcan-tsa-masked.gsq > pcan-tsa-masked.gff3
```

------

[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/88x31.png)][ccby4]  
This work is licensed under a [Creative Commons Attribution 4.0 International License][ccby4].


[pdomproj]: https://github.com/PdomGenomeProject
[GeneSeqer]: http://brendelgroup.org/bioinformatics2go/GeneSeqer.php
[2-26-2014]: http://www.brendelgroup.org/bioinformatics2go/Download/GeneSeqer-2-26-2014.tar.gz
[maskgenomedoi]: http://dx.doi.org/10.6084/m9.figshare.1593187
[transdoi]: http://dx.doi.org/10.6084/m9.figshare.1593168
[ccby4]: http://creativecommons.org/licenses/by/4.0/
