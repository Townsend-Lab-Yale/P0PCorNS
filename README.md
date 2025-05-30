# P0PCorNS
**P0PCorNS (<ins>P</ins>erturbation to <ins>0</ins> to <ins>P</ins>redict <ins>Cor</ins>related <ins>N</ins>etwork <ins>S</ins>tability)** performs informative gene knockouts (One gene was *in silico* knocked out each time ), and uses [BNW (Ziebarth et al. Bioinformatics, 2013.)](https://academic.oup.com/bioinformatics/article/29/21/2801/195868) to generate the Bayesian Networks, then calculates the **Jensen-Shannon divergence (JSD)** between networks. Finally, P0PCorNS can provide an ordered list of gene impacts.

A gene with a higher informative impact would play a more important role in the gene regulatory networks (potentially function more upstream in a linear regulatory order or be the hub gene) , and will be ranked higher for gene manipulation verification experiments. 
## INSTALLATION

P0PCorNS has been tested in **Red Hat Enterprise Linux release 8.8** and is based on **Python 3**.
To install P0PCorNS, just download this repository.


To test and get familiar with P0PCorNS, you can copy the demo files we provide under the **'example/input_demo'** directory to a folder(for example, the "input" folder), then run the following codes:
```
python P0PCorNS.py <csv_file_path> [k]
```
You can also compare your output to the results in the directory **'example/output_demo'**.

## INPUT
### CSV file
- demo_short.csv

**demo_short.csv** contains the foldchange information of 4 genes.

This file is just for a quick test. The running time of 4 genes and 3 stages in both **Gene&Stage** mode and **Gene_only** mode is less than 1 min on the testing server.
```
st01,st12,st23,rel,fmf,pp1,pna
1,1,1,1,1,1,1
1,2,2,1.95934,12.2566,0.970340666,1.80615
1,2,3,1.976248755,11.80867992,1.088135298,1.256936979
1,2,3,2.218305097,10.73544157,1.17096892,2.582973777
```
The first row is variable names.

The second row describes the type of each variable: 1 is for each continuous variable.

The remaining rows are the foldchange information of the variable (stage-specific expression data collected from real experiments).

Note: P0PCorNS also provides a **demo_long.csv** as an example of more complicated cases.

The **demo_long.csv** contains the foldchange information of 11 genes.

The running time of 11 genes and 5 stages in both **Gene&Stage** mode and **Gene_only** mode is about 40 min in the testing server.
```
st01,st12,st23,st34,st45,rel,fmf,pp1,pna,c837,adv,asm,vad,MT1,MT2,bk1
1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1
1,2,2,2,2,1.95934,12.2566,0.970340666,1.80615,3.48704,0.864781922,1.10295,1.39447,1.750521515,1.98014,0.32091
1,2,3,3,3,1.976248755,11.80867992,1.088135298,1.256936979,10.50862851,1.101398661,1.302777735,1.515060619,1.190752282,1.526687561,0.559462795
1,2,3,4,4,2.218305097,10.73544157,1.17096892,2.582973777,10.50200323,1.036128621,1.662780757,2.321259143,0.020534607,1.393654735,0.806710002
1,2,3,4,5,2.231886212,9.65814396,0.92137892,2.984532145,10.73712334,-0.115881132,1.589796151,2.67546936,-0.149555393,1.978355461,-0.494486519
2,3,4,5,5,2.98139685,12.44567093,1.11011892,3.282725138,10.95434666,-1.650521132,1.879659864,2.480042579,1.524784607,2.594232992,-0.728818972
```

### mode
- **Gene&Stage mode**

**Gene&Stage** mode will use the stage information and foldchange information of genes to generate the Bayesian network.
- **Gene_only mode**

**Gene_only** mode will only use the foldchange information of genes to generate the Bayesian network.

That is, for the **Gene_only** mode, P0PCorNS will only use the gene columns as the input, like the following:
```
rel,fmf,pp1,pna
1,1,1,1
1.95934,12.2566,0.970340666,1.80615
1.976248755,11.80867992,1.088135298,1.256936979
2.218305097,10.73544157,1.17096892,2.582973777
```


### optional parameters
`[k]` is one of the parameters needed for running BNW, which sets the number of high-scoring networks to include in model averaging. The bigger k is, the longer the running time will be. Here, we set **k = 20** as the default.

## OUTPUT
P0PCorNS will generate the JSD matrix file (`inputPrefix_jsd_matrix.csv`) and the list file of gene-JSD pairs (that is, `inputPrefix_rearranged_jsd_matrix.csv`), as well as the corresponding files for **Gene_only** mode(`inputPrefix_jsd_matrix-GeneOnly.csv` and `inputPrefix_rearranged_jsd_matrix-GeneOnly.csv`)
- demo_short_jsd_matrix.csv
```
rel,fmf,pp1,pna
0.05265369023718428,0.13325420573769514,0.052034339544552534,0.17669499131189442
0.1385596470513787,0.11302680891399997,0.09093054163610606,0.1947788583778019
0.0,0.14902906284970555,0.09922276826672846,0.13307726121938415
```
- demo_short_rearranged_jsd_matrix.csv
```
rel-1,0.05265369023718428
fmf-1,0.13325420573769514
pp1-1,0.052034339544552534
pna-1,0.17669499131189442
rel-2,0.1385596470513787
fmf-2,0.11302680891399997
pp1-2,0.09093054163610606
pna-2,0.1947788583778019
rel-3,0.0
fmf-3,0.14902906284970555
pp1-3,0.09922276826672846
pna-3,0.13307726121938415
```
- demo_short_jsd_matrix-GeneOnly.csv
```
rel,fmf,pp1,pna
0.17442418897700926,0.16267650468153744,0.10693559671088017,0.15127433096898724
0.12334827111012116,0.07317071878660704,0.052201967361136734,0.19480518170559802
5.705084205643186e-05,0.04079435020700513,0.14037564705260605,0.16052146322824934
```
- demo_short_rearranged_jsd_matrix-GeneOnly.csv
```
rel-1,0.17442418897700926
fmf-1,0.16267650468153744
pp1-1,0.10693559671088017
pna-1,0.15127433096898724
rel-2,0.12334827111012116
fmf-2,0.07317071878660704
pp1-2,0.052201967361136734
pna-2,0.19480518170559802
rel-3,5.705084205643186e-05
fmf-3,0.04079435020700513
pp1-3,0.14037564705260605
pna-3,0.16052146322824934
```





