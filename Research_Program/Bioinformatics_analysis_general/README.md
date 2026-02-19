List of commands and running of scripts for reproducibility of research for Vibrio parahaemolytics

**Vibrio parahaemolytics QC assessment and genome assembly (Bohra)** 

Tool : Bohra  

Github/Web: https://github.com/MDU-PHL/bohra

Commands run: 
```bash 
bohra run -i reads.tab -p default -j V_para_QC_run --cpus 100 
```
Notes on commands run: V_para_ATB.tab input file in the format of #strainID and #path_to_contigs


**Vibrio parahaemolytics serotyping (Kaptive3)** 
Tool : Kaptive3 

Database:  Vp_O and Vp_K database from PMID: 37130055

Github/Web: https://kaptive.readthedocs.io/en/latest/  and https://github.com/aldertzomer/vibrio_parahaemolyticus_genomoserotyping

Commands run: 

```bash
cat V_para.tab | parallel -j 20 --colsep '\t' 'kaptive assembly /kaptive_db/vibrio_parahaemolyticus_genomoserotyping/VibrioPara_Kaptivedb_O.gbk  /home/shared/db/all-the-bacteria/batch/{3} -o kaptive/{1}_kaptive_Vp_k.tsv 
```

```bash
cat V_para.tab | parallel -j 20 --colsep '\t' 'kaptive assembly /kaptive_db/vibrio_parahaemolyticus_genomoserotyping/VibrioPara_Kaptivedb_O.gbk /home/shared/db/all-the-bacteria/batch/{3} -o kaptive/{1}_kaptive_Vp_O.tsv
```

Notes on commands run: V_para_ATB.tab input file in the format of #strainID and #path_to_contigs
Manuscripts/PMID: PMID: 40553506; PMID: 37130055


**Virulence Factor Screening (Abricate with VFDB)**

Tool: Abricate 

Database: VFDB2024 (https://www.mgc.ac.cn/VFs/) 

Github: https://github.com/tseemann/abricate

Command run: 

```bash
cat V_para.tab | parallel -j 100 --colsep '\t' 'abricate {2} --db vfdb --minid 80 --mincov 80 > vfdb/{1}.tab

```

```bash
abricate --summary *.tab > virulome.tab
```
Notes on command run: abricate was run with 80% identity and 80% coverage. abricate also only screens based on nucleotide sequence not amino acid (this may change in the future) V_para.tab input file in the format of #strainID and #path_to_contigs

Manuscripts/PMID: PMID: 39470738; PMID: 15608208


**Multi-locus seqeunce typing (MLST))**

Tool: MLST, MLSTDB 

Database: pubMLST and pasteurMLST databases for all schemes available.

Github: https://github.com/tseemann/mlst; https://github.com/MDU-PHL/mlstdb 

Command run: 

```bash
cat V_para.tab | parallel -j 50 --colsep '\t' 'mlst --quiet --full --blastdb /home/shared/db/mlst/db_all_schemes_v20260203/blast/mlst.fa --datadir /home/shared/db/mlst/db_all_schemes_v20260203/pubmlst {2} > mlst/{1}.mlst' 
```

Notes on command run: mlst does auto-detecion of best fit scheme for typing. 