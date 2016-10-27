# LSTM-punctuation


#### DATA FORMAT

You just need to remove the punctuations in your input text and create a file in which each word in the list below is a SHIFT and each removed punctuation is a GEN(Punc-Punc). The sentences are separated with a blank line. You will need a training set, a development set and a held out test set for evaluation.

```
[][The-DT, economy-NN, 's-POS, temperature-NN, will-MD, be-VB, taken-VBN, from-IN, several-DT, vantage-NN, points-NNS, this-DT, week-NN, with-IN, readings-NNS, on-IN, trade-NN, output-NN, housing-NN, and-CC, inflation-NN, END-END]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
GEN(,-,)
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
GEN(,-,)
[][]
SHIFT
[][]
GEN(,-,)
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
GEN(.-.)
[][]
SHIFT
[][]

[][The-DT, most-RBS, troublesome-JJ, report-NN, may-MD, be-VB, the-DT, August-NNP, merchandise-NN, trade-NN, deficit-NN, due-JJ, out-IN, tomorrow-NN, END-END]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
SHIFT
[][]
GEN(.-.)
[][]
SHIFT
[][]
```

If you do not have POS tags you can put word-NA instead for all words. Example:
```
[][The-NA, most-NA, troublesome-NA, report-NA, may-NA, be-NA, the-NA, August-NA, merchandise-NA, trade-NA, deficit-NA, due-NA, out-NA, tomorrow-NA, END-END]
SHIFT
[][]
SHIFT
[][]
...
```

### Build the system

The first time you clone the repository, you need to sync the cnn/ submodule.
```
git submodule init
git submodule update

mkdir build
cd build
cmake .. -DEIGEN3_INCLUDE_DIR=/path/to/eigen
make -j2
```

### Training

You need to MANUALLY STOP (ctrl c) training if the parameters file hasn't been changed for more than 5 hours.
The system will create a symbolic link: latest_model to your latest parameters file.

#### Training with embeddings

You need to MANUALLY STOP (ctrl c) training if the parameters file hasn't been changed for more than 5 hours.
The system will create a symbolic link: latest_model to your latest parameters file.

    ./lstm-parse -T train.parser -d tdev.parser --hidden_dim 100 --lstm_input_dim 100 -w filewithembeddings --pretrained_dim 100 --rel_dim 20 --action_dim 20 --input_dim 100 -t -S > log.txt &


#### Training without embeddings

    ./lstm-parse -T train.parser -d tdev.parser --hidden_dim 100 --lstm_input_dim 100 --rel_dim 20 --action_dim 20 --input_dim 100 -t -S > log.txt &


#### Training with POS tags 

    ./lstm-parse -T conll2003/train.parser -d conll2003/dev.parser --hidden_dim 100 --lstm_input_dim 100 --rel_dim 20 --action_dim 20 --input_dim 100 -t -P -S > log.txt &
    

### Decoding with embeddings


    ./lstm-parse -T train.parser -d test.parser --hidden_dim 100 --lstm_input_dim 100 -w sskip.100.vectors --pretrained_dim 100 --rel_dim 20 --action_dim 20 --input_dim 100 -m latest_model -S > output.txt
    python attach_prediction.py -p output.txt -t conll2003/test -o evaloutput.txt


#### Decoding without embeddings


    ./lstm-parse -T train.parser -d test.parser --hidden_dim 100 --lstm_input_dim 100 -w sskip.100.vectors --pretrained_dim 100 --rel_dim 20 --action_dim 20 --input_dim 100 -m latest_model -S > output.txt

#### Decoding with POS tags


    ./lstm-parse -T train.parser -d test.parser --hidden_dim 100 --lstm_input_dim 100 -w sskip.100.vectors --pretrained_dim 100 --rel_dim 20 --action_dim 20 --input_dim 100 -m latest_model -S -P > output.txt


You can of course use postags and embeddings. Just put the params accordingly.

### Citation

If you make use of this software, please cite the following:

    @inproceedings{2016emnlpbl,
      author={Miguel Ballesteros and Leo Wanner},
      title={A Neural Network Architecture for Multilingual Punctuation Generation},
      booktitle={Proc. EMNLP},
      year=2016,
    }
