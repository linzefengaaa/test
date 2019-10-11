ecg_ResNext_pytorch-master
==========================
来源：["合肥高新杯"心电人机智能大赛](https://tianchi.aliyun.com/competition/entrance/231754/introduction)

**A榜ResNext4*64d20190930在线F1-Score=0.8081**

**B榜ResNext4*64d20191010在线F1-Score=0.8322**

**系统环境**：ubuntu 16.04 python3.5 pytorch1.2.0
***
**GPU参数**:*cuda版本:10.0,cudnn版本：7.6.0*
***
**网络定义：***在code文件夹下的models文件夹定义了使用的ResNext网络，从头训练权重，定义的ResNext网络总共有4个，可以选择调用，这里经过多次训练得到ResNext4*64d效果最为理想*
***
**大致思路：采用采样将8个导联的数据转化为二维1*8*4096的像素格式，将ResNext卷积的2d改用1d卷积，使用block构建深度神经网络，在A榜预测时将给定的数据集按9：1划分为训练集和验证集，保存出最优的验证集下的权重，最后将给出的测试数据用刚刚的权重喂进模型，生成预测结果**
***
**B榜提升**：由于官方给出了A榜的测试集的标准答案，所以在B榜训练时，将TestA的数据和label加到A榜的训练集参与训练，最后结果准确度提升了0.028
# 数据预处理
数据解压放在data目录下，使用8个导联的数据，将数据集进行训练集和验证集划分，进入code文件夹在Terminal下运行：
```shell
python data_process.py
```
得到预处理后的数据
# label预处理
数据解压放在data目录下，进入code文件夹在Terminal下运行：
```shell
python data_process.py
```
# 模型训练
## 如果没有加载上次训练的权重.pth文件，就会创建一个当前时间下的网络权重文件夹，保存在code文件夹
```shell
python main.py train #从零开始训练
```
如果要加载上次训练的权重：
```shell
python main.py train --ckpt ./ckpt/path #加载训练好的权重进行训练
```
# 模型验证
## 加载权重进行验证集测试：
```shell
python main.py val --ckpt ./ckpt/path #加载训练好的权重进行训练
```
# 模型测试
## 模型测试，在submit文件夹下生成提交结果
```shell
python main.py test --ckpt ./ckpt/path #加载预训练权重进行测试
```
## 提交结果生成（复现提交B榜的最优结果）
```shell
python main.py test --ckpt ./ckpt/ResNeXt29_4x64d_201910092212/best_w.pth #加载最优权重生成预测结果
```
**一些细节**

 1. 训练数据只进行了简单的数据增强，最终无normalize
 2.由于机器显存只有22G，采样后的4096样本batch_size只能开到228张，如果调大效果会更好


参考论文：
https://www.nature.com/articles/s41591-018-0268-3
参考模型构建：
https://blog.csdn.net/winycg/article/details/89077738
### 有问题请邮件至：2060428268@qq.com
