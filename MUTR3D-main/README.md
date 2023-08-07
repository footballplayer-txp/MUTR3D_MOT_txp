# MUTR3D: A Multi-camera Tracking Framework via 3D-to-2D Queries.  [Paper](https://arxiv.org/abs/2205.00613) - [Project Page](https://tsinghua-mars-lab.github.io/mutr3d/)


This repo implements the paper MUTR3D: A Multi-camera Tracking Framework via 3D-to-2D Queries. We built our implementation upon MMdetection3D.

The major part of the code is in the directory `plugin/track`. To use this code with MMDetection3D, we need older versions of MMDetection3D families(see Environment section), and you need to replace `mmdet3d/api` with the `mmdet3d/api` provided here. 


## How to run



## Environment

First, install: 
1. mmcv==1.3.14
2. mmdetection==2.12.0
3. [nuscenses-devkit](https://github.com/nutonomy/nuscenes-devkit)
4. Note: for tracking we need to install:
`motmetrics==1.1.3`, not newer version, like `motmetrics==1.2.0`!!

Second, clone mmdetection3d==0.13.0, but replace its `mmdet3d/api/` from mmdetection3d by `mmdet3d/api/` in this repo.

e.g. 
```
git clone https://github.com/open-mmlab/mmdetection3d.git
cd mmdetection3d
git checkout v0.13.0
# cp -r ../mmdet3d/api mmdet3d/
# cp ../mmdet3d/models/builder.py mmdet3d/models/
# cp ../mmdet3d/models/detectors/mvx_two_stage.py mmdet3d/models/detectors/mvx_two_stage.py

# replace the mmdetection3d/mmdet3d with the mmdet3d_full
cp -r ../mmdet3d_full ./mmdet3d

cp -r ../plugin ./ 
cp -r ../tools ./ 
# then install mmdetection3d following its instruction. 
# and mmdetection3d becomes your new working directories. 
```



### Dataset preprocessing
After preparing the nuScenes Dataset following mmdetection3d,  you need to generate a meta file or say `.pkl` file. 

```
python3 tools/data_converter/nusc_track.py
```


### Run training

I provide a template config file in `plugin/track/configs/resnet101_fpn_3frame.py`. You can directly use this config or read this file, especially its comments, and modify whatever you want. I recommend using DETR3D pre-trained models or other nuScenes 3D Detection pre-trained models. 

basic training scripts on a machine with 8 GPUS: 
```
CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 bash tools/dist_train_tracker.sh plugin/track/configs/resnet101_fpn_3frame.py 8 --work-dir=work_dirs/experiment_name
```

basic test scripts
```
# You can perform inferece, then save the result file
python3 tools/test.py plugin/track/configs/resnet101_fpn_3frame.py <model-path> --format-only --eval-options jsonfile_prefix=<dir-name-for-saving-json-results>

# or you can perform inference and directly perform the evaluation
python3 tools/test.py plugin/track/configs/resnet101_fpn_3frame.py <model-path>  --eval --bbox
```

### Visualization
For visualization, I suggest user to generate the results json file first. I provide some sample code at `tools/nusc_visualizer.py` for visualizing the predictions, see function `_test_pred()` in `tools/nusc_visualize.py` for examples. 

## Results

| Backbones  | AMOTA-val | AMOTP-val | IDS-val | Download |   
|---|---|---| --- | --- |
| ResNet-101 w/ FPN  | 29.5  | 1.498 | 4388 | [model](https://drive.google.com/file/d/1MXbHWalo-zyt9TU31x-re4wOuX5G4wOH/view?usp=sharing) \| [val results](https://drive.google.com/file/d/1qf8D3cTDCdlspOEJpgRXnSGW9AotaRxP/view?usp=sharing)  |
| ResNet-50 w/ FPN  |  25.2 |  1.573| 3899 | [model](https://drive.google.com/file/d/1_BPDvDPKN7j476w2g5IMAagCW5szfF2y/view?usp=sharing) \| [val results](https://drive.google.com/file/d/1bIgsRgBwTjcGzlNaHAoXMCLWsqEr3cnH/view?usp=sharing)  |



## Acknowledgment

For the implementation, we rely heavily on [MMCV](https://github.com/open-mmlab/mmcv), [MMDetection](https://github.com/open-mmlab/mmdetection), [MMDetection3D](https://github.com/open-mmlab/mmdetection3d),[MOTR](https://github.com/megvii-model/MOTR), and [DETR3D](https://github.com/WangYueFt/detr3d)



## Relevant projects 
1. [DETR3D: 3D Object Detection from Multi-view Images via 3D-to-2D Queries](https://tsinghua-mars-lab.github.io/detr3d/)
2. [FUTR3D: A Unified Sensor Fusion Framework for 3D Detection](https://tsinghua-mars-lab.github.io/futr3d/)
3. For more projects on Autonomous Driving, check out our camera-centered autonomous driving projects page [webpage](https://tsinghua-mars-lab.github.io/vcad/) 


## Reference


```
@article{zhang2022mutr3d,
  title={MUTR3D: A Multi-camera Tracking Framework via 3D-to-2D Queries},
  author={Zhang, Tianyuan and Chen, Xuanyao and Wang, Yue and Wang, Yilun and Zhao, Hang},
  journal={arXiv preprint arXiv:2205.00613},
  year={2022}
}
```

Contact: [Tianyuan Zhang](http://tianyuanzhang.com/) at: `tianyuaz@andrew.cmu.edu` or `tianyuanzhang1998@gmail.com`
