# Caffe_MultiLabel_Classification
This is a instance of multilabel classification in caffe. Thanks to the tools of multi-label data conversion tool in https://github.com/HolidayXue/CodeSnap by HolidayXue. 
# Step
##1、Recompile
Download convert_multilabel.cpp in https://github.com/HolidayXue/CodeSnap, replace convert_imageset.cpp, then recompile caffe. You will get a new exe :convert_multilabel.exe, which can make multi-label data.
##2、Manufacture data
Download my ZnCar data in this page. Try to manufacture your own lmdb by the example command line:
```
convert_imageset.exe  --resize_height=227  --resize_width=227 ZnCar/ ZnCar/Label.txt ZnCarTrainImage ZnCarTrainLabel 2
```
We also need mean file:
```
compute_image_mean.exe ZnCarTrainImage ZnCarTrainMean.binaryproto
```
Generally speaking, you should get lmdb:ZnCarTrainImage/ZnCarTestImage/ZnCarTrainLabel/ZnCarTestlabel mean_file:ZnCarTrainMean.binaryproto/ZnCarTestMean.binaryproto finally.
##3、Finetune AlexNet
We can use pretrained caffemodel:bvlc_alexnet.caffemodel to assign the weights to some of the layers that have no change(conv/pool/fc6/7). Finetune command line:
```
Build\x64\Release\caffe.exe train --solver=D:\caffe-master\models\bvlc_alexnet\solver.prototxt --weights=D:\caffe-master\models\bvlc_alexnet\bvlc_alexnet.caffemodel
```
You can refer to some of my parameters in solver.prototxt and multi_label_AlexNet.prototxt.
##4.Modify classification.cpp
classification.exe in Caffe only support single label classification, we should modify classification.cpp. You can use my classification_multilabel.cpp and recompile just like step.1

##5.Use our model to classify a picture
(deploy.prototxt is necessary.)We can use our model to classify a picture for 2 labels:
```
classification.exe deploy.prototxt network.caffemodel mean.binaryproto label1.txt label2.txt img.jpg
```
The source Image:

![](http://img.blog.csdn.net/20170120172753355?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTXJfQ3Vycnk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

The result:

![](http://img.blog.csdn.net/20170120172805042?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTXJfQ3Vycnk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# DataSet
You can try ZnCar.zip. It includes 3 vehicle types and 2 vehicle surfaces.  60 train imgs and 12 test imgs.

### Note that this project only support 2 classes classification. 
The network structure:
![](http://img.blog.csdn.net/20170120103953309?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTXJfQ3Vycnk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
