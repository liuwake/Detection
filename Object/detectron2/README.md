# [facebookresearch/detectron2](https://github.com/facebookresearch/detectron2)

## install
- V0.6 build
```shell
conda create -n d2 python=3.7 opencv=4.1.2 -y && conda activate d2
pip install torch==1.10.1+cu113 torchvision==0.11.2+cu113 torchaudio==0.10.1 -f https://download.pytorch.org/whl/cu113/torch_stable.html
git clone -b v0.6 https://github.com/facebookresearch/detectron2.git && python -m pip install -e detectron2
```
- V0.6 binary

- coco
  ```shell
  wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip
  wget http://images.cocodataset.org/zips/val2017.zip
  wget http://images.cocodataset.org/zips/test2017.zip
  wget http://images.cocodataset.org/zips/train2017.zip
  ```
## Run
### demo
`python demo.py --config-file ../configs/COCO-Detection/retinanet_R_50_FPN_1x.yaml --input 000000000781.jpg [--otheroptions] --opts MODEL.WEIGHTS detectron2://COCO-Detection/retinanet_R_50_FPN_1x/190397773/model_final_bfca0b.pkl`

### test
`python ./tools/train_net.py --config-file ./configs/COCO-Detection/retinanet_R_50_FPN_1x.yaml --eval-only MODEL.WEIGHTS detectron2://COCO-Detection/retinanet_R_50_FPN_1x/190397773/model_final_bfca0b.pkl`

### debug
- `python ./tools/train_net.py --config-file ./configs/COCO-Detection/faster_rcnn_R_50_FPN_1x.yaml --num-gpus 1 SOLVER.IMS_PER_BATCH 1 INPUT.MIN_SIZE_TRAIN (400,) DATASETS.TRAIN ('coco_2017_val',) DATALOADER.NUM_WORKERS 0`
  - BUG: `  "Predicted boxes or scores contain Inf/NaN. Training has diverged."FloatingPointError: Predicted boxes or scores contain Inf/NaN. Training has diverged.`
![alt text](<截屏2024-04-26 14.17.58.png>)
## Config
### pychram
- `./tools/train_net.py`
    - script: `--config-file ./configs/COCO-Detection/retinanet_R_50_FPN_1x.yaml --eval-only MODEL.WEIGHTS detectron2://COCO-Detection/retinanet_R_50_FPN_1x/190397773/model_final_bfca0b.pkl`

## bug
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
      - ref: 
        - [AttributeError: module ‘distutils‘ has no attribute ‘version‘ 解决方案](https://zhuanlan.zhihu.com/p/556704117)
        - [【Tensorboard报错解决】AttributeError:module ‘distutils‘ has no attribute ‘version‘](https://blog.csdn.net/weixin_44115162/article/details/128612465)

- `SummaryWriter`, and `distutils`
  ```
  Traceback (most recent call last): File "/root/github/detectron2/tools/train_net.py", line 169, in <module> args=(args,), File "/root/github/detectron2/detectron2/engine/launch.py", line 82, in launch main_func(*args) File "/root/github/detectron2/tools/train_net.py", line 151, in main trainer = Trainer(cfg) File "/root/github/detectron2/detectron2/engine/defaults.py", line 396, in init self.register_hooks(self.build_hooks()) File "/root/github/detectron2/detectron2/engine/defaults.py", line 463, in build_hooks ret.append(hooks.PeriodicWriter(self.build_writers(), period=20)) File "/root/github/detectron2/detectron2/engine/defaults.py", line 475, in build_writers return default_writers(self.cfg.OUTPUT_DIR, self.max_iter) File "/root/github/detectron2/detectron2/engine/defaults.py", line 248, in default_writers TensorboardXWriter(output_dir), File "/root/github/detectron2/detectron2/utils/events.py", line 145, in init from torch.utils.tensorboard import SummaryWriter File "/root/miniconda3/envs/d2/lib/python3.7/site-packages/torch/utils/tensorboard/init.py", line 4, in <module> LooseVersion = distutils.version.LooseVersion AttributeError: module 'distutils' has no attribute 'version'
  ```
  - solved: ` pip install setuptools==58.0.4`

PS
- `conda create -n d2 pytorch==1.10.1 torchvision==0.11.2 torchaudio==0.10.1 cudatoolkit=11.3 -c pytorch -c conda-forge -y && conda activate d2` led to [MKL error](https://github.com/pytorch/pytorch/issues/123097#issuecomment-2055236551).