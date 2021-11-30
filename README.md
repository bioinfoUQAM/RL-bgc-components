## **RL for BGC components**
A reinforcement learning approach to support improving components in fungal candidate BGCs, based on Pfam protein domains and optional use of BGC functional annotations.


#### **Requirements**

Python 3.6+ is recommended. 
Library dependencies can be found in `/src/requirements.txt`, and should be installed in the project `virtualenv` before starting.
PySpark requires Java 8 or later (recommended 8 or 11) to be installed, and `JAVA_HOME` set.

#### **How to start**

Make a copy of `/src/config.init.DEFAULT`, and rename it to `/src/config.init`. Update the `[default] home` to the current project root path.

#### **Train - configure & run**

At the `[prediction]` section  in the `config.init` file, specify the minimum parameters accordingly:
- indicate the corpus location in `source.path`
- specify the learner parameters as desired in `episodes`, `alpha`, `gamma`, `epsilon`, `penalty.threshold`, and `keepskip.threshold` 


To train the reinforcement learner, from the project `virtualenv` simply run:

```bash
(.env) user@foo:~bgc-components/src$ python -m pipeprediction.RL
```

Trained models and features are outputted at `/metrics/models`.

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

To use the reinforcement learner and evaluate candidate BGCs, from the project `virtualenv` simply run:

```bash
(.env) user@foo:~bgc-components/src$ python -m eval.Evaluator
```
