# TreeDRSparsing
The codes for the paper "Discourse Representation Parsing for Sentences and Documents" ACL 2019.

## The requirements

    python 3.7
    pytorch 1.10.0+cu111
  
## Prepare
Clone the codes on the branch DRTS

    git clone -b py37 https://github.com/cestwc/TreeDRSparsing.git
    
Download the dataset from https://drive.google.com/open?id=1nayRyFiT_-vHaf6-oyVwHaEK79CYzMpU, move them to the folder data/; Download the pretrained vector from https://drive.google.com/open?id=1ICyISR-0PhuQYxIsqE5P7_r-OCsETIEU, move it to the folder embeddings/ (ensure each line is a word representation).

```python
import sys
sys.path.append('TreeDRSparsing')

import nltk
nltk.download('averaged_perceptron_tagger')
nltk.download('wordnet')
```

    ls data
    data/trn.tree.align_drs
    data/dev.tree.align_drs
    data/tst.tree.align_drs
    data/dict
    
See branch `bs_sattn_drssup` to get the oracles as:
  
    data/trn.tree.align_drs.oracle.doc.*
    data/dev.tree.align_drs.oracle.doc.*
    data/tst.tree.align_drs.oracle.doc.*
    
## Training
Using the command below for training:

    python TreeDRSparsing/main.py train --gpu --model-path-base models --pretrain-path embeddings/sskip.100.vectors --action-dict-path data/dict --train-input data/trn.tree.align_drs.oracle.doc.in --train-action data/trn.tree.align_drs.oracle.doc.out --check-per-update 100 --eval-per-update 5000 --optimizer adam
    
The models are save in the foler models, for each checkpoint, a model will be saved e.g. model1.

## Evaluation
Using the command below for validating each checkpoint, see branch `bs_sattn_drssup` to see the F1 score on development dataset.

    python TreeDRSparsing/main.py test --gpu --model-path-base models --pretrain-path embeddings/sskip.100.vectors --action-dict-path data/dict --test-input data/dev.tree.align_drs.oracle.doc.in --test-output dev_outputs_tmp --const --beam-size 1

    
Authors choose the model with the highest F1 on develpment dataset as the final model, and trained model can be download from https://drive.google.com/open?id=1rzr4nd67tGHNo6T099e_FDxZkBVRFwbB.

## To get branch `py37` from `bs_sattn_drssup`:

    apt-get install -y 2to3
    2to3 --output-dir=TreeDRSparsing -W -n TreeDRSparsing
