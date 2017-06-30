比赛地址和数据：https://www.kesci.com/apps/home/#!/competition/58dba69775722d38fa2dfcf6/content/0

通过分析，我们发现数据集是按照orderdate_lastord字段排序过的，所以为了保证抽取的样本和原样本相似，我们将训练样本分成：02468 和13579

主要从uid，basicroomid，roomid三个主体进行特征构造。

1.uid：用户上次订单的相关信息反映了用户的偏好，通过对比这次订单和上次订单的差异来构造特征。

2.basicroomid：通过构造basicroom的特征，模型才可以通过比较不同basicroom的相似性和差异性从训练中学习到用户为什么选择该basicroom。

3.roomid：通过构造room的特征，原因同上。

通过分析，我们还大胆的猜测了roomtag_1其实是携程的推荐，标签应该是猜你喜欢。我们的模型其实已经相当于stacking的第一层。
所以我们也根据这种思路进行融合。
我们在02468训练，然后预测13579以及test上的所有数据集。得到一个新特征:prob

因为题目要求预测7天的数据，所以前6天的数据在这些特征上是穿越的。
所以我们将预测分成了两个模型来分开预测：前6天&第7天

在以上特征的基础上使用lgb进行训练，并未继续做任何融合。
