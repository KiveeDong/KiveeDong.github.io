---
layout: post
title:  "论文阅读：Detecting and Recognizing Human Object Interactions"
date:   2018-09-07
---
InteractNet（CVPR2018）
-----------
[文章链接](https://arxiv.org/abs/1704.07333)
<font size=4>
V-COCO  ：40.0%  
<div align=center>
![InteractNet](https://img-blog.csdn.net/20180906223822648?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3MDE0NzUw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
<div align=left><font size=4>
首先用RPN得到proposal，然后用RoIAlign提取每个proposal的特征。接下来：
 - object detection分支，进行box regression和classification；
 - human-centric分支，判断box内人的动作，每个动作给一个score，并且尝试定位target物体的位置，我想应该是每个动作都有一个target位置，给出一个可能在的位置的高斯分布，感觉和LiFeifei她们那篇referring relationship的论文有点像，这样做有一个很强的假设：box内的特征表明了动作的受体的位置；
 - 利用object的特征，也计算得到关于动作的score，然后将其和第二步得到的分数相加并过sigmoid。
<div align=center>
![这里写图片描述](https://img-blog.csdn.net/20180906224012334?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3MDE0NzUw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
<div align=left><font size=4>
上式代表对于某对human-object是关系a的分数，前两项分别是human和object的类别分数，第三项是human-centric branch或者interaction branch产生的动作分数，第四项是由h和a推断的target的位置和o的位置的近似程度，值越大越好，我感觉是判断是否有关系的一种手段，另一种手段是直接用h和o的特征进行计算。 Cascaded Inference:由于这种从单个h或者o得到a的计算方式，计算复杂度只有O(n)，许多别的方法是两两concat计算，计算复杂度是O(n^2)。即便选择进行interaction branch，虽然时间复杂度也是O(n^2)，但由于是简单的加法，所以计算时间依然非常地少！
