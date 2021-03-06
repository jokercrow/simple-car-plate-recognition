# simple-car-plate-recognition
简单车牌识别-Mask_RCNN定位车牌+手写方法分割字符+CNN单个字符识别

# 数据准备

准备用于车牌定位的数据集，要收集250张车辆图片，200张用于训练，50张用于测试，然后在这些图片上标注出车牌区域。这里有图片https://gitee.com/easypr/EasyPR/tree/master/resources/image/general_test 。标注工具使用VGG Image Annotator (VIA)，就是一个网页程序，可以导入图片，使用多边形标注，标注好了以后，导出json。我已经标注好的数据集可以从这里下载https://github.com/airxiechao/simple-car-plate-recognition/blob/master/dataset/carplate.zip ，用7zip解压。

准备用于字符识别的数据集，包含分隔好的单个车牌汉子、字母和数字。这里有https://gitee.com/easypr/EasyPR/blob/master/resources/train/ann.7z 。

# 训练Mask-RCNN定位车牌

这篇文章https://engineering.matterport.com/splash-of-color-instance-segmentation-with-mask-r-cnn-and-tensorflow-7c761e238b46 讲了如何用Mask-RCNN识别图片中的气球，仿照其方法。在https://github.com/matterport/Mask_RCNN/releases 下载预先用COCO数据集训练好的模型mask_rcnn_coco.h5，放到Mask-RCNN文件夹，按照文章的方法编写carplate.py用于载入车辆图片数据和训练，用inspect_data.ipynb浏览标注数据。执行python carplate.py  train --dataset=../dataset/carplate --weights=coco 进行训练，训练完后，在logs文件夹中找到最后一轮的h5模型文件，比如mask_rcnn_carplate_0030.h5，复制出来。用inspect_model.ipynd查看模型训练的效果。这部分代码在https://github.com/airxiechao/simple-car-plate-recognition/tree/master/Mask_RCNN 下载。

# 训练CNN单个字符识别

仿照keras的mnist_cnn例子https://github.com/keras-team/keras/blob/master/examples/mnist_cnn.py 训练，将训练好的模型导出为char_cnn.h5。这部分代码在https://github.com/airxiechao/simple-car-plate-recognition/blob/master/char_cnn/char_cnn.ipynb 下载。

# 分割车牌字符

把车牌区域转换成灰度图像，利用字符间间隔主要是黑色的特征，先横向分割出单个字符，再进行纵向分割，去除单个字符上下的噪音。代码是https://github.com/airxiechao/simple-car-plate-recognition/blob/master/inference.ipynb 中的split_x和split_y。

# 执行推理

代码在https://github.com/airxiechao/simple-car-plate-recognition/blob/master/inference.ipynb 。
