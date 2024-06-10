<h1 align="center">
BLAM6A-Merge: Leveraging Attention Mechanisms and Feature Fusion Strategies to Improve the Identification of RNA N6-methyladenosine Sites
</h1>

<div align="center">

[![](https://img.shields.io/badge/github-green?style=plastic&logo=github)](https://github.com/DoraemonXia/BLAM6A-Merge)
<!-- [![](https://img.shields.io/badge/dataset-zenodo-orange?style=plastic&logo=zenodo)](https://zenodo.org/records/10021618) -->
</div>

## Overview
This repository contains the source code for paper "BLAM6A-Merge: Leveraging Attention Mechanisms and Feature Fusion Strategies to Improve the Identification of RNA N6-methyladenosine Sites". If you have questions, or you have problem using my tools on test other dataset， don't hesitate to open an issue or ask me via <121106022704@njust.edu.cn>. We are happy to hear from you!

![](https://github.com/DoraemonXia/BLAM6A-Merge/blob/main/imgs/Figure_1.jpg)

<!-- ## News
**Oct 10 2023**: The trained FABind model and processed dataset are released!

**Oct 11 2023**: Initial commits. More codes, pre-trained model, and data are coming soon. -->

## Setup Environment
This is an example for how to set up a working conda environment to run the code. In this example, we have cuda version==12.1, and we install torch==2.0.1. 

To make sure the pyg packages are installed correctely, we directly install them from whl.

```shell
conda create --name BLAM6A python=3.8
conda activate BLAM6A

conda install pytorch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 pytorch-cuda=11.8 -c pytorch -c nvidia
pip install --upgrade pip
pip install pandas numpy matplotlib seaborn scikit-learn gensim
```

## Data
<!--The origin dataset we used can be found from **zenado.-->
We also provide the .fasta in data/ folder.

You can also generate the sequence yourself from the R script in data_prepare/ folder.

## Training process

This File has included the process of extracting features, network training.

```shell
# type_name = ["FullTranscript", "matureRNA"]
# cell_name = ["A549","CD8T","Hek293_abacm","Hek293_sysy","HeLa","MOLM13"]
# for example
python train.py --type_name FullTranscript --cell_name A549
python blastn_process.py --type_name FullTranscript --cell_name A549 --task_name generate_fasta
```

## The use of Blastn
Please firstly install the Blastn of version 2.14.0.

And you can process the result of Blastn. <!-- through **.ipynb. -->

We also provide the results generated by the Blastn tool in the Blastn folder.

<!--blastn -db train -query Kfold_0.fasta -word_size 4 -outfmt 6 -out Kfold_0.out -num_threads 10 -->

```shell
cd data
#for example
cd FullTranscript+A549
makeblastdb -in train.fasta -dbtype nucl -out train -parse_seqids
blastn -db train -query test.fasta -word_size 4 -outfmt 6 -out test.out
cd ../..
python blastn_process.py --type_name FullTranscript --cell_name A549 --task_name blastn_results
```

## Testing process

This file can predict the m6A sites in test.fasta according provided type_name and cell_name.
```shell
# for example
python test.py --type_name FullTranscript --cell_name A549 --if_blastn True
# This will generate a .csv file in result File to save the predicted values.
```