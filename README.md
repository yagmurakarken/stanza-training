# stanza-training
During training, we followed the published [stanza-train repo] (https://github.com/stanfordnlp/stanza-train) and [training document] (https://stanfordnlp.github.io/stanza/training.html) to facilitate the process.\
After cloning the Stanza-train repo, we set up the necessary folders and scripts using the following commands as stated in the steps.

```
git clone https://github.com/stanfordnlp/stanza-train.git
cd stanza-train
pip install -r requirements.txt

git clone https://github.com/stanfordnlp/stanza.git
cp config/config.sh stanza/scripts/config.sh
cp config/xpos_vocab_factory.py stanza/stanza/models/pos/xpos_vocab_factory.py
cd stanza
```
At this point, we added the shorthand names of the datasets we want to use during training to the lists in the fxpos_vocab_factory.py.\
The config.sh file provides necessary environment data as data path, word vector path etc. Run `source config.sh` to set the following environment variables. \
The xpos_vocab_factory.py script is used to build XPOS vocabulary file.

## Preparing Word Vector Data
For preparing word vector data, we used these commands:
```
python
>>> import stanza
>>> stanza.download("tr")   # or whichever language you are using
```

These commands will download the `stanza_resources` folder to your home folder. You can change the directory by adding a `dir= ` argument to `stanza.download()` command.\
While training we include the flag `--wordvec_pretrain_file` which is pointing the  `$STANZA_RESOURCES/lang/depparse` file. To train modules that make use of word representations as dependency parsing, it is recommended to use these pretrained embedding vectors.

## Converting UD data

```python -m stanza.utils.datasets.prepare_depparse_treebank ${corpus} ${other_args}```\
where ${corpus} is the full name of the corpus; ${other_args} are other arguments allowed by the training script.\
We used `--wordvec_pretrain_file` as ${other_args}.

## Training with Scripts
We used the provided  scripts for training under the `stanza/utils/training`  folder.

```python -m stanza.utils.training.run_depparse ${corpus} ${other_args}```

### Training Results

|                   | UAS |  LAS | BLEX|
| ------------------| ----| -----| ----|
| UD_Turkish-BOUN   |78.80| 73.10 | 70.76|
| turkish-treebanks  |92.89| 90.50 |86.81|
