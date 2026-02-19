

**SNP-calling and mutaiton deteciton from conitgs and reads**

Tool: snippy
Verion: 4.4.5
Github: https://github.com/tseemann/snippy

Command run:

```bash
cat V_para.tab | parallel -j 10 --colsep '\t' 'snippy --ctgs {2} --outdir snippy/{1} --ref contigs/reference.fa’ 
```
Notes on command run: V_para.tab input file in the format of #strainID and #path_to_contigs

```bash
cat V_para_reads.tab | parallel -j 10 --colsep '\t' 'snippy --R1 {2} --R2 {3} --outdir snippy/{1} --ref contigs/reference.fa’ 
```
Notes on command run: V_para_reads.tab input file in the format of #strainID and #path_to_reads1, #path_to_reads2

```bash
snippy-core * --ref contigs/reference.fa 
```

**Phylogenetic analysis using IQ-Tree**

Tool: IQtree2
Github: https://github.com/iqtree/iqtree2

```bash
iqtree2 -s core.aln -m GTR+F+G4 -bb 1000 -nt 26 --redo
```
Notes on command run: this is the first tree in the analysis which is used as input into gubbins

```bash
iqtree2 -s core_recomb_removed.fasta -m GTR+F+G4 -bb 1000 -nt 26 --date dates.tab --date-ci 1000 --redo
```
Notes on command run: this is the second tree in the analyiss whihc is used after gubbins and snp-sites see below

**Recombiantion detection and removal using Gubbins**

Tool: Gubbins
Github: https://github.com/nickjcroucher/gubbins

```bash
run_gubbins.py core.full.aln -s core.aln.treefile -i 20 -m 5 -p gubbins -c 50 -r GTRGAMMA 
```
```bash
snp-sites gubbins.filtered_polymorphic_sites.fasta -c -o core_recomb_removed.fasta
```
**Time tree reconstruction using Treetime**

Tool: Treetime 
Github: https://github.com/neherlab/treetime

```bash
treetime --tree core_recomb_removed.fasta.treefile --aln core_recomb_removed.fasta --dates dates2.tsv 
```
Notes on command run: this is basic treetime 


```bash
treetime --tree core_recomb_removed.fasta.treefile --aln core_recomb_removed.fasta --dates dates.tsv --covariation --stochastic-resolve --coalescent skyline --confidence
```
Notes on command run: —skyline plots adds the skyline plots with confidence intervals 


```bash
treetime mugration --tree core_recomb_removed.fasta.treefile --states meta.tsv --attribute Location --confidence
```
Notes on command run: —location data estimates of nodes 


**Genome annotaiton with Bakta**

Github: https://github.com/oschwengers/bakta
```bash
cat list.txt | parallel -j 10 --colsep '\t' 'bakta {2} --db /home/laceyj1/db-light -p {1} -o {1} --genus Vibrio --species parahaemolyticus --threads 16' 
```

**Pan-genome analysis using Panaroo**

Github: https://github.com/gtonkinhill/panaroo

```bash
panaroo -i *.gff3 -o panaroo_results --clean-mode strict --remove-invalid-genes
```
