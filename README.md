# MIQA Pixel-based classifier

## DON'T Use Visual Studio Code DevContainers!
Normally I love VSC DevContainers but installing the requirements into a devcontainer seems to lock things up. It's a pain.

You can try, maybe your computer is faster, better, smarter than mine.

## Setup
- Pull down the repo
- Create a python virtual environment: `python3 -m venv venv`
- Activate the virtual environment: `source venv/bin/activate`
- Install required packages: `pip install -r ./miqa/learning/requirements.txt`

## Get the data
For example, copy [PredictHD_small](https://drive.google.com/drive/u/1/folders/1SYY5LdKvU6fHgty1ynYXsVzqhGamsZmM) from the shared Google Drive into `nn_work_dir`, as well as files `T1_fold0.csv`, `T1_fold1.csv` and `T1_fold2.csv` from [Learning](https://drive.google.com/drive/u/1/folders/1uT24WMjZLt7IJWPXR-K7YYwiFUSomr_L).
Now edit the paths inside `T1_fold*.csv` files to match the path on your file system, e.g.:
```shell
sed -i 's+P:/PREDICTHD_BIDS_DEFACE/+/home/exampleUser/nn_work_dir/PredictHD_small/+g' T1_fold0.csv
sed -i 's+P:/PREDICTHD_BIDS_DEFACE/+/home/exampleUser/nn_work_dir/PredictHD_small/+g' T1_fold1.csv
sed -i 's+P:/PREDICTHD_BIDS_DEFACE/+/home/exampleUser/nn_work_dir/PredictHD_small/+g' T1_fold2.csv
```

## Run training
To run training on the 3-fold cross-validation using fold 0 as validation set and folds 1 and 2 as training set, use:
```shell
python ./miqa/learning/nn_classifier.py -f ./T1_fold -c 3 -v 0
```
This will produce a file called `miqa01-val0.pth`. To run full 3-fold cross validation, execute:
```shell
python ./miqa/learning/nn_classifier.py -f ./T1_fold -c 3 --all
```
This will produce `miqa01-val0.pth`, `miqa01-val1.pth` and `miqa01-val2.pth`.

## Get pre-trained model files
This git repository comes with pre-trained model files in the models subdirectory allowing for using the neural net without waiting for training. These files are large, so they are maintained with Git LFS. Therefore, upon cloning this repository, you will receive pointer files to the content and will not be able to use them yet.

In order to convert these pointer files to the actual content, you must install git-lfs on your system.

```shell
sudo apt install git-lfs
```

Then, anywhere in the miqa directory:

```shell
git lfs install
git lfs fetch
git lfs checkout
```

This will convert your pointer files. You should now be able to use the files in the models subdirectory.


## Run inference
To run inference on a single file, execute:
```shell
python ./nn_training.py -m ./models/miqaT1-val0.pth -1 ./coronacases_001.nii.gz
```
The above command uses the neural network weights produced by training on folds 1 and 2.

To run inference on one of the folds, e.g. fold 1, use:
```shell
python ./miqa/learning/nn_classifier.py -f ./T1_fold -c 3 -v 1 --evaluate
```
