# FrAug
FrAug is a data augmentation framework for time-series forecasting. It has two augmentation techniques: Frequency Masking and Frequency Mixing

## Getting Started
### Environment Requirements

First, please make sure you have installed Conda. Then, our environment can be installed by:
```
conda create -n FrAug python=3.7
conda activate FrAug
pip install -r requirements.txt
```

### Data Preparation
You can obtain all the eight benchmarks from [Google Drive](https://drive.google.com/drive/folders/1ZOYpTUa82_jCcxIdTmyr0LXQfvaM9vIy) provided in Autoformer. All the datasets are well pre-processed and can be used easily.

```
mkdir dataset
```
**Please put them in the `./dataset` directory**

## To reproduce the results in our paper

### Original performance of models
We provide scripts to facilitate reproducing main experiment results in our paper.

You can get the Original performances of DLinear by running scripts in `scripts/Original/DLinear`. For example, to get the original performance(without augmentations) of DLinear in ETTh1, you can run

```
sh scripts/Original/DLinear/etth1.sh
```

For FEDformer, Autoformer and Informer, you can use scripts: `scripts/Original/former/Formers_Long.sh`.  For LightTS, the script is `scripts/Original/LightTS/lightTS.sh`.

### Performance of models with FrAug
This experiment shows the performance of model with FrAug.

For DLinear, you can run the scripts in `scripts/LongForecast/DLinear`. For FEDformer, Autoformer and Informer, the script is `scripts/FrAug/former/Formers_Long.sh`. For LightTS, the script is `scripts/FrAug/LightTS/lightTS.sh`.

There are few hyperparameters in this part:
| Parameter      |                              Interpretation                          |
| ------------- | -------------------------------------------------------| 
| aug            | Set it to f_mask for Frequency Masking, f_mix for Frequency Mixing                   |
| aug_rate      | The corresponding mask/mix rate   | 
| in_batch_augmentation | Augment data in training batch  (save memory) |

### Cold start forecasting
This experiment shows the performance of model in cold-start forecasting. By setting data_size=0.01, we can use only 1% of the training set. 

Scripts can be found in `scripts/ColdStart`.

There are few hyperparameters in this part:
| Parameter      |                              Interpretation                          |
| ------------- | -------------------------------------------------------| 
| data_size            | The size of original traning set, 0.5 means use 50% of training data for training           |
| aug_data_size      | The size of augmented data, for example aug_data_size=49 means augment 50x the training set   | 
| in_dataset_augmentation | Augment data in training dataset (create augmented data before training)                  |

### Test time training
This experiment shows the performance of model with test time training. We divide the test set to several parts. Since in real life, testing data comes sequentially by time. Therefore, after testing on one part, we can use the data to retrain the model for testing in the future part. However, newly add training data from test set can have little impact on the model. We can use FrAug to augment the new data to increase its importance.

Scripts can be found in `scripts/TestTime`.

There are few hyperparameters in this part:
| Parameter      |                              Interpretation                          |
| ------------- | -------------------------------------------------------| 
| testset_div            | Divide the data to "testset_div" parts. The larger the number, the more times we find available test data to retrain the model           |
