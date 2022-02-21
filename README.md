## **RL for BGC components**
A reinforcement learning approach to support improving components in fungal candidate BGCs, based on Pfam protein domains and optional use of BGC functional annotations.


#### **Requirements**

Unix/Linux and Python 3.6+ are recommended. 
Library dependencies can be found in `/src/requirements.txt`, and should be installed in the project `virtualenv` before starting.
PySpark requires Java 8 or later (recommended 8 or 11) to be installed, and `JAVA_HOME` set.
On Windows environments PySpark may require [additional steps](https://cwiki.apache.org/confluence/display/HADOOP2/WindowsProblems), such as obtaining Hadoop native libraries for Windows and setting `HADOOP_HOME`.

#### **How to start**

Make a copy of `/src/config.init.DEFAULT`, and rename it to `/src/config.init`. Update the `[default] home` to the current project root path.

#### **Train - configure & run**

At the `[prediction]` section  in the `config.init` file, specify the minimum parameters accordingly:
- indicate the corpus location in `source.path`
- specify the learner parameters as desired in `episodes`, `alpha`, `gamma`, `epsilon`, `penalty.threshold`, and `keepskip.threshold` 


To train the reinforcement learner, from the project `virtualenv` simply run:

```bash
(.env) user@foo:~RL-bgc-components/src$ python -m pipeprediction.RL
```

Training data can be obtained at [this repository](https://github.com/bioinfoUQAM/fungalbgcdata).
Sample training data files are provided in `/corpus/train`.
Model and feature files are outputted in `/corpus/metrics/models`.
Trained model files (based on best performing parameters and balanced dataset) are also provided in `/corpus/metrics/models`.


#### **Test - configure & run**

At the `[prediction]` section  in the `config.init` file: 
- set `True` to parameters `neighbor.weight`, `dry.islands`, and `average.action`  to use the functional annotation strategies available

At the `[eval]` section  in the `config.init` file, use the following parameters to indicate the requested inputs: 
- `result.path`: file with list of candidate BGC predictions 
- `goldID.path`: file with list of gold BGC clusters (for comparison, if available)
- `similarity.path`: file with output from a BLAST all-vs-all for target genome (if available)
- `gene.length`: file with list of amino acid length for each gene (or designated genome regions)
- `gene.map`: path for all files of domains per gene (or designated genome regions)

Sample files are provided in `/Databases` and `/corpus/metrics`.

Candidate BGC predictions can be obtained using a BGC prediction tool, such as [TOUCAN](https://github.com/bioinfoUQAM/TOUCAN), or generated in the same format as `/corpus/metrics/sample-candidateBGCs.IDs.test`.
Amino acid sequence lengths for candidate BGC genes can be extracted from the FASTA sequence file(s), and listed in the same format as `/Databases/sample-geneLength`.
Pfam protein domains per genes can be obtained using [PfamScan](https://www.ebi.ac.uk/Tools/pfa/pfamscan/), and listed in the same format as `/Databases/sample-geneMap/*.domains` (similar to FASTA). 

To use the reinforcement learner and evaluate candidate BGCs, from the project `virtualenv` simply run:

```bash
(.env) user@foo:~RL-bgc-components/src$ python -m eval.Evaluator
```

Result files are outputted in `/corpus/metrics/`.
