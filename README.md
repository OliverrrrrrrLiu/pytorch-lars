# pytorch-lars
Layer-wise Adaptive Rate Scaling in PyTorch

This repo contains a PyTorch implementation of layer-wise adaptive rate scaling (LARS) from the paper "[Large Batch Training of Convolutional Networks](https://arxiv.org/abs/1708.03888)" by You, Gitman, and Ginsburg.

To run, do

`python train.py --optimizer LARS --cuda lars_results`

It uses [skeletor-ml](https://github.com/noahgolmant/skeletor) for experiment logging.

## Preliminary results

I just tested this using a ResNet18 on CIFAR-10. eo test on very large batch sizes, I used a standard [gradient accumulation trick](https://medium.com/huggingface/training-larger-batches-practical-tips-on-1-gpu-multi-gpu-distributed-setups-ec88c3e51255).

![Alt text](images/lars_test_curves.jpg?raw=true "Title")

| Batch Size | Test Accuracy |
| ---------- | ------------- |
|    64      |    89.97      |
|    256     |    86.59      |
|    1024    |    83.05      |
|    4096    | 76.92         |
| 16384      | 68.33         |

As a comparison, using SGD with momentum, I am able to achieve about 93.5% test accuracy in 200 epochs using a geometric decay schedule (using [this](https://github.com/kuangliu/pytorch-cifar) implementation). I have not done extensive hyperparameter tuning, though -- I used the default parameters suggested by the paper. I had a base learning rate of 0.1, 200 epochs, eta .001, momentum 0.9, weight decay of 5e-4, and the polynomial learning rate decay schedule.
