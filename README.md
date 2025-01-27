# Anomaly-Detection-PCB
This is an implementation of the paper [PaDiM: a Patch Distribution Modeling Framework for Anomaly Detection and Localization](https://arxiv.org/pdf/2011.08785).   

This code is heavily borrowed from the unofficial implementation by [xiahaifeng1995](https://github.com/xiahaifeng1995/PaDiM-Anomaly-Detection-Localization-master) and the efficient net version implementation by [yougjae](https://github.com/youngjae-avikus/PaDiM-EfficientNet/tree/master)

## Requirements
* python >= 3.7
* pytorch >= 1.5
* torchvision
* pillow==9.5.0
* tqdm
* sklearn
* matplotlib
* scikit-image
* [EfficientNet-PyTorch](https://github.com/lukemelas/EfficientNet-PyTorch)

## Datasets
For initial testing , the  MVTec AD datasets : Download from [MVTec website](https://www.mvtec.com/company/research/datasets/mvtec-ad/) is used. 

Custom PCB data created using Unity. The tree structure required for running training:
```bash
pcb
├── ground_truth
│   ├── misplaced_objects
│   │   ├── step0.camera_solo_1_mask.png
│   │   ├── step0.camera_solo_2_mask.png
│   │   ├── step0.camera_solo_3_mask.png
│   │   ├── step0.camera_solo_4_mask.png
│   │   ├── step0.camera_solo_5_mask.png
│   │   ├── step0.camera_solo_6_mask.png
│   │   ├── step0.camera_solo_7_mask.png
│   │   └── step0.camera_solo_mask.png
│   └── missing_objects
│       ├── step0.camera_solo_1_mask.png
│       ├── step0.camera_solo_2_mask.png
│       ├── step0.camera_solo_3_mask.png
│       └── step0.camera_solo_mask.png
├── test
│   ├── good
│   │   ├── step0.camera_solo.png
│   │   ├── step0.camera_solo_1.png
│   │   ├── step1.camera_solo.png
│   │   └── step1.camera_solo_1.png
│   ├── misplaced_objects
│   │   ├── step0.camera_solo.png
│   │   ├── step0.camera_solo_1.png
│   │   ├── step0.camera_solo_2.png
│   │   .
    │   .
│   └── missing_objects
│       ├── step0.camera_solo.png
│       ├── step0.camera_solo_1.png
│       .
        .
└── train
    └── good
        ├── step0.camera_solo.png
        ├── step0.camera_solo_1.png
        ├── step0.camera_solo_3.png
        ├── step0.camera_solo_4.png
        .
        .

toolbox
├── ground_truth
│   ├── misplaced_objects
│   │   ├── step0.camera_solo_1_mask.png
│   │   ├── step0.camera_solo_2_mask.png
│   │   ├── step0.camera_solo_3_mask.png
│   │   ├── step0.camera_solo_4_mask.png
│   │   ├── step0.camera_solo_5_mask.png
│   │   ├── step0.camera_solo_6_mask.png
│   │   ├── step0.camera_solo_7_mask.png
│   │   └── step0.camera_solo_mask.png
│   └── missing_objects
│       ├── step0.camera_solo_1_mask.png
│       ├── step0.camera_solo_2_mask.png
│       ├── step0.camera_solo_3_mask.png
│       └── step0.camera_solo_mask.png
├── test
│   ├── good
│   │   ├── step0.camera_solo.png
│   │   ├── step0.camera_solo_1.png
│   │   ├── step1.camera_solo.png
│   │   └── step1.camera_solo_1.png
│   ├── misplaced_objects
│   │   ├── step0.camera_solo.png
│   │   ├── step0.camera_solo_1.png
│   │   ├── step0.camera_solo_2.png
│   │   .
    │   .
│   └── missing_objects
│       ├── step0.camera_solo.png
│       ├── step0.camera_solo_1.png
│       .
        .
└── train
    └── good
        ├── step0.camera_solo.png
        ├── step0.camera_solo_1.png
        ├── step0.camera_solo_3.png
        ├── step0.camera_solo_4.png
        .
        .


```

It must be noted you will need to change the list of class names in `datasets/mvtec.py` in line 13 as:
```python
CLASS_NAMES = ['pcb', 'toolbox']
```
## Training 

In order to train the Efficient net model
```python
 python efficient_main.py -d [dataset location] -s [results storage directory] --training 
 ```

 ## Inference

 Create a folder inside the directory called model_pkl_efficientnet-b4. The b4 here refers to the model architecture used, thus needs to be replaced of the following format
 ```
model_pkl_<model name>
 ```

Place the trained model inside the folder. The model trianed for the PCB can be found [here](https://drive.google.com/file/d/1h28jrUBAWC0qK6xuzVMcmFyPKlIyZUKM/view?usp=sharing). 

One can also use gdown to download the pkl file.
```
$ pip install gdown
$ gdown --fuzzy "link to the google drive file"
```

Then run the inference code :
```python
python inference_test.py -t data_path_of_single_test_image -s save_path_where_the_pkl_folder_exists_and_where_results_are_stored
```

## Tests

#### Inference time
The inference times recorded for 35 test cases of PCB anomaly images

|    Machine    |      CPU      |     GPU      |     Time(in seconds)      |
| ------------- | ------------- | ------------ | -----------------------   |
| GPU_machine_1  |  i9  |  RTX 12 GB | 0.2047 |
| Personal  | i5  | None | 2.43 |

## Code flow structure

Training 

![training](https://github.com/venkatramnank/Anomaly-Detection-PCB-Unity/blob/main/imgs/training.png)

Inference

![inference](https://github.com/venkatramnank/Anomaly-Detection-PCB-Unity/blob/main/imgs/inference.png)

## In depth code documentation

In depth code documentation is available here : [In Depth Code Doc](https://github.com/venkatramnank/Anomaly-Detection-PCB-Unity/blob/main/Anomaly%20Detection%20For%20PCB%20(Code%20documetation).md)

## Reference
[1] Thomas Defard, Aleksandr Setkov, Angelique Loesch, Romaric Audigier. *PaDiM: a Patch Distribution Modeling Framework for Anomaly Detection and Localization*. https://arxiv.org/pdf/2011.08785

[2] https://github.com/byungjae89/SPADE-pytorch

[3] https://github.com/byungjae89/MahalanobisAD-pytorch
