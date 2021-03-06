# BarcodeAsm -  local read cloud assembler

This program uses `fermi-lite` and bead barcode information from the 10X
sequencing protocol to create local assemblies of target regions. We attempt to
identify unmapped/mismapped reads that are relevant to the local region by
enumerating the barcodes of the locally mapped reads.

To compile `BarcodeAsm` use the following:

```
git clone --recursive https://github.com/cgroza/BarcodeAsm.git
cd BarcodeAsm
./autogen.sh
./configure
make
make install
```

The executable should be in `bin`.

To run `BarcodeAsm`, the following arguments are needed:
+ -b : path to the indexed BAM file produced by the `longranger` pipeline
+ -B : path the barcode sorted and indexed BAM file from above
+ -r : path to BED file containing the start and end of local
  assembly windows
+ -F : path to FASTA file listing sequences of interest to be checked in the
  newly assembled contigs (optional)
+ -g : path to the genome FASTA file
+ -G : output GFA for each assembly window
+ -o : minimum required read overlap during assembly `fermi-lite`
+ -P : pop small bubbles in heterozygous regions (optional). Keeps the larger bubbles.
+ -S : separate reads by phase before assembly (optional)
+ -o : minimum overlap between reads (default 30)
+ -a : import all reads belonging to the barcodes in the local assembly window
  (optional)
+ -t : number of threads (default is 1, needs 2GB memory per thread)


The outputs are:
+ `contigs.fa` : FASTA file containing all assembled contigs. Names describe the
  local assembly window and the phase (p1/2 is first/second phase and p0 is
  unphased)
+ `alignments.tsv`: file describing the alignment of assembled contigs to the
  local window
+ `hits.tsv` : TE library (provided by -F) alignment hits against the assembled contigs for each window
+ local_window.gfa : GFA file for each local window that contains the assembly graph
