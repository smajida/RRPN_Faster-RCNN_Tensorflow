# R2CNN_Attention_Tensorflow

## Abstract
This is a tensorflow re-implementation of [RRPN: Arbitrary-Oriented Scene Text Detection via Rotation Proposals](https://arxiv.org/pdf/1703.01086).      

It should be noted that we did not re-implementate exactly as the paper and just adopted its idea.    

This project is based on [Faster-RCNN](https://github.com/DetectionTeamUCAS/Faster-RCNN_Tensorflow), and completed by [YangXue](https://github.com/yangxue0827) and [YangJirui](https://github.com/yangJirui).

## Citation
Some relevant achievements based on this code.     

    @article{https://arxiv.org/abs/1806.04828
        Author = {Xue Yang, Hao Sun, Xian Sun, Menglong Yan, Zhi Guo, Kun Fu},
        Title = {Position Detection and Direction Prediction for Arbitrary-Oriented Ships via Multiscale Rotation Region Convolutional Neural Network},
        Year = {2018}
    } 
    
    @article{yangxue_r-dfpn:http://www.mdpi.com/2072-4292/10/1/132 or https://arxiv.org/abs/1806.04331
        Author = {Xue Yang, Hao Sun, Kun Fu, Jirui Yang, Xian Sun, Menglong Yan and Zhi Guo},
        Title = {{R-DFPN}: Automatic Ship Detection in Remote Sensing Images from Google Earth of Complex Scenes Based on Multiscale Rotation Dense Feature Pyramid Networks},
        Journal = {Published in remote sensing},
        Year = {2018}
    }

## [DOTA](https://captain-whu.github.io/DOTA/index.html) test results      
![1](DOTA.png)

## Comparison 
**Part of the results are from [DOTA](https://arxiv.org/abs/1711.10398) paper.**
### Task1 - Oriented Leaderboard
| Approaches | mAP | PL | BD | BR | GTF | SV | LV | SH | TC | BC | ST | SBF | RA | HA | SP | HC |
|------------|:---:|:--:|:--:|:--:|:---:|:--:|:--:|:--:|:--:|:--:|:--:|:---:|:--:|:--:|:--:|:--:|
|[SSD](https://link.springer.com/chapter/10.1007%2F978-3-319-46448-0_2)|10.59|39.83|9.09|0.64|13.18|0.26|0.39|1.11|16.24|27.57|9.23|27.16|9.09|3.03|1.05|1.01|
|[YOLOv2](https://arxiv.org/abs/1612.08242)|21.39|39.57|20.29|36.58|23.42|8.85|2.09|4.82|44.34|38.35|34.65|16.02|37.62|47.23|25.5|7.45| 
|[R-FCN](http://papers.nips.cc/paper/6465-r-fcn-object-detection-via-region-based-fully-convolutional-networks)|26.79|37.8|38.21|3.64|37.26|6.74|2.6|5.59|22.85|46.93|66.04|33.37|47.15|10.6|25.19|17.96|
|[FR-H](https://ieeexplore.ieee.org/abstract/document/7485869/)|36.29|47.16|61|9.8|51.74|14.87|12.8|6.88|56.26|59.97|57.32|47.83|48.7|8.23|37.25|23.05|
|[FR-O](https://arxiv.org/abs/1711.10398)|52.93|79.09|69.12|17.17|63.49|34.2|37.16|36.2|89.19|69.6|58.96|49.4|52.52|46.69|44.8|46.3|
|[R2CNN](https://github.com/DetectionTeamUCAS/R2CNN_Faster-RCNN_Tensorflow)|60.67|80.94|65.75|35.34|67.44|59.92|**50.91**|55.81|**90.67**|66.92|72.39|55.06|52.23|55.14|53.35|48.22|
|[RRPN](https://github.com/DetectionTeamUCAS/RRPN_Faster-RCNN_Tensorflow)|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|[Current improvement]()|**68.01**|**89.43**|**81.18**|**42.68**|**70.28**|**63.74**|50.15|**62.76**|90.21|**76.17**|**83.57**|**58.53**|**61.06**|**63.33**|**66.30**|**60.68**|

## Requirements
1、tensorflow >= 1.2     
2、cuda8.0     
3、python2.7 (anaconda2 recommend)    
4、[opencv(cv2)](https://pypi.org/project/opencv-python/) 
5、[tfplot](https://github.com/wookayin/tensorflow-plot)  

## Download Model
1、please download [resnet50_v1](http://download.tensorflow.org/models/resnet_v1_50_2016_08_28.tar.gz)、[resnet101_v1](http://download.tensorflow.org/models/resnet_v1_101_2016_08_28.tar.gz) pre-trained models on Imagenet, put it to data/pretrained_weights.     
2、please download [mobilenet_v2](https://storage.googleapis.com/mobilenet_v2/checkpoints/mobilenet_v2_1.0_224.tgz) pre-trained model on Imagenet, put it to data/pretrained_weights/mobilenet.
3、please download [trained model](https://github.com/DetectionTeamUCAS/Models/tree/master/RRPN_Faster-RCNN_Tensorflow) by this project, put it to output/trained_weights.

## Data Prepare
1、please download [DOTA](https://captain-whu.github.io/DOTA/dataset.html)
2、crop data, reference:
```  
cd $PATH_ROOT/data/io/DOTA
python train_crop.py 
python val_crop.py
```
3、data format
```
├── VOCdevkit
│   ├── VOCdevkit_train
│       ├── Annotation
│       ├── JPEGImages
│    ├── VOCdevkit_test
│       ├── Annotation
│       ├── JPEGImages
```  

## Compile
```  
cd $PATH_ROOT/libs/box_utils/
python setup.py build_ext --inplace
```

```  
cd $PATH_ROOT/libs/box_utils/cython_utils
python setup.py build_ext --inplace
```

## Demo

**Select a configuration file in the folder (libs/configs/) and copy its contents into cfgs.py, then download the corresponding [weights](https://github.com/DetectionTeamUCAS/Models/tree/master/RRPN_Faster-RCNN_Tensorflow).**      

```   
python demo_rh.py --src_folder='/PATH/TO/DOTA/IMAGES_ORIGINAL/' 
                  --image_ext='.png' 
                  --des_folder='/PATH/TO/SAVE/RESULTS/' 
                  --save_res=False
                  --gpu='0'
```

## Eval
```  
python eval.py --img_dir='/PATH/TO/DOTA/IMAGES/' 
               --image_ext='.png' 
               --test_annotation_path='/PATH/TO/TEST/ANNOTATION/'
               --gpu='0'
```

## Inference
```  
python inference.py --data_dir='/PATH/TO/DOTA/IMAGES_CROP/'      
                    --gpu='0'
```

## Train
1、If you want to train your own data, please note:  
```     
(1) Modify parameters (such as CLASS_NUM, DATASET_NAME, VERSION, etc.) in $PATH_ROOT/libs/configs/cfgs.py
(2) Add category information in $PATH_ROOT/libs/label_name_dict/lable_dict.py     
(3) Add data_name to line 75 of $PATH_ROOT/data/io/read_tfrecord.py 
```     

2、make tfrecord
```  
cd $PATH_ROOT/data/io/  
python convert_data_to_tfrecord.py --VOC_dir='/PATH/TO/VOCdevkit/VOCdevkit_train/' 
                                   --xml_dir='Annotation'
                                   --image_dir='JPEGImages'
                                   --save_name='train' 
                                   --img_format='.png' 
                                   --dataset='DOTA'
```     

3、train
```  
cd $PATH_ROOT/tools
python train.py
```

## Tensorboard
```  
cd $PATH_ROOT/output/summary
tensorboard --logdir=.
``` 
