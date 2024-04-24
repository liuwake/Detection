# Detection
## Intro

## Object Detection
### Platform
#### [facebookresearch/detectron2](https://github.com/facebookresearch/detectron2)
##### install
- V0.6 build
```shell
conda create -n d2 python=3.7 opencv=4.1.2 -y & conda activate d2
pip install torch==1.10.1+cu113 torchvision==0.11.2+cu113 torchaudio==0.10.1 -f https://download.pytorch.org/whl/cu113/torch_stable.html
git clone -b v0.6 https://github.com/facebookresearch/detectron2.git && python -m pip install -e detectron2
```
- V0.6 binary

##### demo
`python demo.py --config-file ../configs/COCO-Detection/retinanet_R_50_FPN_1x.yaml --input 000000000781.jpg [--otheroptions] --opts MODEL.WEIGHTS detectron2://COCO-Detection/retinanet_R_50_FPN_1x/190397773/model_final_bfca0b.pkl`

##### test
`python ./tools/train_net.py --config-file ./configs/COCO-Detection/retinanet_R_50_FPN_1x.yaml --eval-only MODEL.WEIGHTS detectron2://COCO-Detection/retinanet_R_50_FPN_1x/190397773/model_final_bfca0b.pkl`

##### bug
- `GLIBCXX_3.4.30`
  ```
  (d2) root@WK-XEON:~/github/detectron2# python ./tools/train_net.py --config-file ./configs/COCO-Detection/retinanet_R_50_FPN_1x.yaml --eval-only MODEL.WEIGHTS detectron2://COCO-Detection/retinanet_R_50_FPN_1x/190397773/model_final_bfca0b.pkl
  Traceback (most recent call last):
    File "./tools/train_net.py", line 24, in <module>
      import detectron2.utils.comm as comm
    File "/root/github/detectron2/detectron2/__init__.py", line 5, in <module>
      setup_environment()
    File "/root/github/detectron2/detectron2/utils/env.py", line 108, in setup_environment
      _configure_libraries()
    File "/root/github/detectron2/detectron2/utils/env.py", line 72, in _configure_libraries
      import cv2
  ImportError: /lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.30' not found (required by /root/miniconda3/envs/d2/lib/python3.7/site-packages/../../libopencv_gapi.so.406)
  ```
    - ~~solved:`conda install -c conda-forge gcc`~~
    - solved: `conda install opencv=4.1.2 -y`

PS
- `conda create -n d2 pytorch==1.10.1 torchvision==0.11.2 torchaudio==0.10.1 cudatoolkit=11.3 -c pytorch -c conda-forge -y && conda activate d2` led to [MKL error](https://github.com/pytorch/pytorch/issues/123097#issuecomment-2055236551).

## Text Detection

## Risk Detection
- 检测刀枪人等安防领域危险对象
  - 需要数据集
  - 算法
    - PlanA检测异物 ,检测异物特异性
    - PlanB检测表面刀枪形状
      - 迁移现有CT安检模型
  - 策略
    - 先出雏形产品,再靠用户丰富数据集提高baseline

## TODO
- [ ] 完善doc
  - [ ] update 目录