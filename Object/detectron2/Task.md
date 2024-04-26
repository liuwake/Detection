# task
## 3：【视频】通用物体检测概述
利用 Anaconda 配好 detectron2 物体检测平台，并利用 PyCharm 完成下面两个步骤：
1. 利用 detectron2 提供的 Faster R-CNN_R50-FPN_1x 模型和 RetinaNet_R50_1x 模型测试https://github.com/rbgirshick/py-faster-rcnn/tree/master/data/demo 里面的 5张图片，对比两者的检测结果
- `python demo.py --config-file ../configs/COCO-Detection/faster_rcnn_R_50_FPN_1x.yaml --input *.jpg --output faster_rcnn_R_50_FPN_1x --opts MODEL.WEIGHTS detectron2://COCO-Detection/faster_rcnn_R_50_FPN_1x/137257794/model_final_b275ba.pkl`
- bug: `python demo.py --config-file ../configs/COCO-Detection/fast_rcnn_R_50_FPN_1x.yaml --input *.jpg --output fast_rcnn_R_50_FPN_1x --opts MODEL.WEIGHTS detectron2://COCO-Detection/fast_rcnn_R_50_FPN_1x/137635226/model_final_e5f7ce.pkl`
- `python demo.py --config-file ../configs/COCO-Detection/retinanet_R_50_FPN_1x.yaml --input *.jpg --output retinanet_R_50_FPN_1x --opts MODEL.WEIGHTS detectron2://COCO-Detection/retinanet_R_50_FPN_1x/190397773/model_final_bfca0b.pkl`

2. 利用 detectron2 提供的 Faster R-CNN_R50-FPN_1x `box AP 37.9`模型和 RetinaNet_R50_1x `box AP 37.4`模型 在MSCOCO 验证集上进行评测 ，得到官方给出的精度.

    ```
    python train_net.py --config-file ./configs/COCO-Detection/faster_rcnn_R_50_FPN_1x.yaml --eval-only MODEL.WEIGHTS detectron2://COCO-Detection/faster_rcnn_R_50_FPN_1x/137257794/model_final_b275ba.pkl
    ```
    ```
    python train_net.py --config-file ./configs/COCO-Detection/fast_rcnn_R_50_FPN_1x.yaml --eval-only MODEL.WEIGHTS detectron2://COCO-Detection/fast_rcnn_R_50_FPN_1x/137635226/model_final_e5f7ce.pkl
    ```
    ```
    python train_net.py --config-file ./configs/COCO-Detection/retinanet_R_50_FPN_1x.yaml --eval-only MODEL.WEIGHTS detectron2://COCO-Detection/retinanet_R_50_FPN_1x/190397773/model_final_bfca0b.pkl
    ```




