# Awesome CIFAR Zoo  

<img src="https://user-images.githubusercontent.com/7837172/44953557-0fb54e80-aec9-11e8-9d38-2388bc70c5c5.png" width=15% align="right" /> 

This repository contains the pytorch code for multiple CNN architectures and improve methods based on the following papers, **hope the implementation and results will helpful for your research**!!

- Architecure
  - **(lenet)** [LeNet-5, convolutional neural networks](http://yann.lecun.com/exdb/lenet/)
  - **(alexnet)** [ImageNet Classification with Deep Convolutional Neural Networks](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks)
  - **(vgg)** [Very Deep Convolutional Networks for Large-Scale Image Recognition](https://arxiv.org/abs/1409.1556)
  - **(resnet)** [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385)
  - **(preresnet)** [Identity Mappings in Deep Residual Networks](https://arxiv.org/abs/1603.05027)
  - **(resnext)** [Aggregated Residual Transformations for Deep Neural Networks](https://arxiv.org/abs/1611.05431)
  - **(densenet)** [Densely Connected Convolutional Networks](https://arxiv.org/abs/1608.06993)
  - **(senet)** [Squeeze-and-Excitation Networks](https://arxiv.org/abs/1709.01507)
- Regularization
  - **(shake-shake)** [Shake-Shake regularization](https://arxiv.org/abs/1705.07485)
  - **(cutout)** [Improved Regularization of Convolutional Neural Networks with Cutout](https://arxiv.org/abs/1708.04552)
  - **(mixup)** [mixup: Beyond Empirical Risk Minimization](https://arxiv.org/abs/1710.09412)
- Learning Rate Scheduler
  - **(cos_lr)** [SGDR: Stochastic Gradient Descent with Warm Restarts](https://arxiv.org/abs/1608.03983)
  - **(htd_lr)** [Stochastic Gradient Descent with Hyperbolic-Tangent Decay on Classification](https://arxiv.org/abs/1806.01593)


## Requirements and Usage 

### Requirements

- Python >= 3.5
- PyTorch >= 0.4 
- TensorFlow/Tensorboard (if you want to use the tensorboard for **visualization**)
- Other dependencies (pyyaml, easydict, tensorboardX)

```bash
pip install -r requirements.txt
```

### Usage 

simply run the cmd for the training:

```bash
## 1 GPU for lenet
CUDA_VISIBLE_DEVICES=0 python -u train.py --work-path ./experiments/cifar10/lenet

## resume from ckpt
CUDA_VISIBLE_DEVICES=0 python -u train.py --work-path ./experiments/cifar10/lenet --resume

## 2 GPUs for resnet1202
CUDA_VISIBLE_DEVICES=0,1 python -u train.py --work-path ./experiments/cifar10/preresnet1202

## 4 GPUs for densenet190bc
CUDA_VISIBLE_DEVICES=0,1,2,3 python -u train.py --work-path ./experiments/cifar10/densenet190bc
``` 

We use yaml file ``config.yaml`` to save the parameters, check any files in `./experimets` for more details.  
You can see the training curve via tensorboard, ``tensorboard --logdir path-to-event --port your-port``.  
The training log will be dumped via logging, check ``log.txt`` in your work path.


## Results on CIFAR

### Vanilla architecures

| architecure         |    GPUs    | params | batch size | epoch | C10 test acc (%) | C100 test acc (%) |
| :------------------ | :--------: | :----: | :--------: | :---: | :--------------: | :---------------: |
| Lecun               | 1 x 1080TI |  62K   |    128     |  250  |      67.46       |       34.10       |
| alexnet             | 1 x 1080TI |  2.4M  |    128     |  250  |      75.56       |       38.67       |
| vgg19               | 1 x 1080TI |  20M   |    128     |  250  |      93.00       |       72.07       |
| preresnet20         | 1 x 1080TI | 0.27M  |    128     |  250  |      91.88       |       67.03       |
| preresnet110        | 1 x 1080TI |  1.7M  |    128     |  250  |      94.24       |       72.96       |
| preresnet1202       | 2 x 1080TI | 19.4M  |    128     |  250  |      94.74       |       75.28       |
| densenet100bc       | 2 x 1080TI | 0.76M  |     64     |  300  |      95.08       |       77.55       |
| densenet190bc       | 4 x 1080TI | 25.6M  |     64     |  300  |      96.11       |       82.59       |
| resnext29_16x64d    | 2 x 1080TI | 68.1M  |    128     |  300  |      95.94       |       83.18       |
| se_resnext29_16x64d | 2 x 1080TI | 68.6M  |    128     |  300  |    **96.15**     |     **83.65**     |


### With additional regularization


PS: the default data augmentation methods are ``RandomCrop`` + ``RandomHorizontalFlip`` + ``Normalize``,   
and the ``√`` means which additional method be used. :cake:

| architecure              | epoch | cutout | mixup | C10 test acc (%) |
| :----------------------- | :---: | :----: | :---: | :--------------: |
| preresnet20              |  250  |        |       |      91.88       |
| preresnet20              |  250  |   √    |       |      92.57       |
| preresnet20              |  250  |        |   √   |      92.71       |
| preresnet20              |  250  |   √    |   √   |      92.66       |
| preresnet110             |  250  |        |       |      94.24       |
| preresnet110             |  250  |        |       |      94.24       |
| preresnet110             |  250  |   √    |       |      94.67       |
| preresnet110             |  250  |        |   √   |      94.94       |
| preresnet110             |  250  |   √    |   √   |      95.66       |
| se_resnext29_16x64d      |  300  |        |       |      96.10       |
| se_resnext29_16x64d      |  300  |   √    |       |      96.60       |
| se_resnext29_16x64d      |  300  |        |   √   |      96.86       |
| se_resnext29_16x64d      |  300  |   √    |   √   |    **97.03**     |
| shake_resnet26_2x64d     | 1800  |        |       |      96.94       |
| shake_resnet26_2x64d     | 1800  |   √    |       |    **97.20**     |
| shake_resnet26_2x64d     | 1800  |        |   √   |    **97.42**     |
| **shake_resnet26_2x64d** | 1800  |   √    |   √   |    **97.71**     |

PS: ``shake_resnet26_2x64d`` achieved **97.71%** test accuracy with ``cutout`` and ``mixup``!!    
It's cool, right?  
 
### With different LR scheduler


| architecure  | epoch | step decay | cosine | htd(-6,3) | C10 test acc (%) |
| :----------- | :---: | :--------: | :----: | :-------: | :--------------: |
| preresnet20  |  250  |     √      |        |           |      91.88       |
| preresnet20  |  250  |            |   √    |           |      92.13       |
| preresnet20  |  250  |            |        |     √     |    **92.44**     |
| preresnet110 |  250  |     √      |        |           |      94.24       |
| preresnet110 |  250  |            |   √    |           |      94.48       |
| preresnet110 |  250  |            |        |     √     |    **94.82**     |


## Acknowledgments

Provided codes were adapted from

- [kuangliu/pytorch-cifar](https://github.com/kuangliu/pytorch-cifar)
- [bearpaw/pytorch-classification](https://github.com/bearpaw/pytorch-classification)
- [timgaripov/swa](https://github.com/timgaripov/swa)
- [xgastaldi/shake-shake](https://github.com/xgastaldi/shake-shake)
- [uoguelph-mlrg/Cutout](https://github.com/uoguelph-mlrg/Cutout)
- [BIGBALLON/cifar-10-cnn](https://github.com/BIGBALLON/cifar-10-cnn)


Feel free to contact me if you have any suggestions or questions, issues are welcome,   
create a PR if you find any bugs or you want to contribute. :blush:     