# MIQA Pixel-based Classifier

## DON'T Use Visual Studio Code DevContainers!
Normally I love VSC DevContainers but installing the requirements into a devcontainer seems to lock things up. It's a pain.

You can try, maybe your computer is faster, better, smarter than mine (likely).

## Setup
- Pull down the repo
- Create a python virtual environment: `python3 -m venv venv`
- Activate the virtual environment: `source venv/bin/activate`
- Install required packages: `pip install -r ./miqa/learning/requirements.txt`

## Pre-trained Model Files
This repo includes pre-trained models in the models subdirectory so that you can begin using the neural net without waiting for training to complete.

## Run training

### Get the data
For example, copy [PredictHD_small](https://drive.google.com/drive/u/1/folders/1SYY5LdKvU6fHgty1ynYXsVzqhGamsZmM) from the shared Google Drive, as well as files `T1_fold0.csv`, `T1_fold1.csv` and `T1_fold2.csv` from [Learning](https://drive.google.com/drive/u/1/folders/1uT24WMjZLt7IJWPXR-K7YYwiFUSomr_L).

Now edit the paths inside `T1_fold*.csv` files to match the path on your file system, e.g.:
```shell
sed -i 's+P:/PREDICTHD_BIDS_DEFACE/+/home/exampleUser/nn_work_dir/PredictHD_small/+g' T1_fold0.csv
```

### Limited 3-Fold Cross Validation
To run training on the 3-fold cross-validation using fold 0 as validation set and folds 1 and 2 as training set, use:

```shell
python ./miqa/learning/nn_training.py -f ./T1_fold -c 3 -v 0
```

### Full 3-Fold Cross Validation
This will produce a file called `miqa01-val0.pth`. 

To run full 3-fold cross validation, execute:

```shell
python ./miqa/learning/nn_training.py -f ./T1_fold -c 3 --all
```

This will produce `miqa01-val0.pth`, `miqa01-val1.pth` and `miqa01-val2.pth`.

## Run inference

### Inference on a Single File
To run inference on a single file, execute:
```shell
python ./nn_training.py -m ./models/miqaT1-val0.pth -1 ./images/coronacases_001.nii.gz
```
The above command uses the neural network weights produced by training on folds 1 and 2.


## Inference on One of the Folds
To run inference on one of the folds, e.g. fold 1, use:
```shell
python ./miqa/learning/nn_training.py -f ./T1_fold -c 3 -v 1 --evaluate
```
