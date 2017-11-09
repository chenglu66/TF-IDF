# TF-IDF
基于自己写的文章的关键词提取
在基于内容的推荐系统中常使用基于关键字向量空间模型，但是怎么衡量在一个文本中那个字是关键字，和文章的主题相关比较大，怎么做呢？
是不是出现的次数越多越和主题相关，但是有个问题如果文章有长比较长，有的比较短，那么这种仅仅是靠计算出现的次数的方式，是不是对短点的文档相对来说比较的不公平呢？那怎么处理呢？
是不是归一化比较好点，这样在计算相似度上面，不会因为各个维度之间评述的差异，而对结果产生影响，因此最佳的归一化途径有很多，直接除以最大值即可，或者用与最小值的差除以最大间距。这两个方法哪个好点，感觉其实应该都是差不多的。
然后还有一个问题，加入一个稀有词汇出现文章中，那么我应该保证他的权重要比常出现的那个词汇要高一些，因为在别的文章中不曾出现，而这本文章中出现，这是这篇文章的特色，可以理解为该文章的特色。总结下就是下面三条：
1.稀有词汇的相关不能小于常见次数的权重
2.一篇文章常出现的词汇的权重要比只出现一次的权重要大。
3.长文档不一定要好于段文档（归一化）
TF-IDF（tk,dj)=TF(tk,dj)*log(N/nk)
其中N表示所有文档的合计，nk就是含有tk词汇的至少出现一次文章的个数。
TF（tk，dj）=fkj/max（z）fzj
一边文章中出现的词汇的最大次数的即为max(z)fzj,
后的权重应该是归一化之后的权重，这样在计算相似度上面不会带来不利影响，
wkj=TF-IDF(tk,dj)/（sum文档数量(s=1toT)（TF-IDF(ts,dj））^1/2
在后面就是利用余弦相似度来计算相似度。


#程序开始
import sys
import jieba
import string
import re
from collections import Counter
reload(sys)
sys.setdefaultencoding('utf-8')  # 设置默认编码为utf-8
# 计算词频的例子
def com_tf():
    f = open('data/7s.txt', 'r')
    data = f.read()
    data_temp = data.decode('utf-8')  # 转换为unicode编码形式
    data = ''.join(re.findall(u'[\u4e00-\u9fff]+', data_temp))  # 必须为unicode类型，取出所有中文字符
    # sts = data.translate(None, string.punctuation)            # 删除英文的标点符号，中文标点不支持。

    data2 = jieba.cut(data)  # 分词
    data3 = " ".join(data2)  # 结果转换为字符串（列表转换为字符串）

    open('data/7temp.txt', 'w').write(data3)  # 分词结果写入7temp.txt

    wlist = data3.split()      # 将分词结果按空格切割为列表（字符串的切割）
    num_dict = Counter(wlist)  # 统计词频

    # 统计结果写入result.txt(字典的遍历)
    for (k, v) in num_dict.items():
        open('data/result.txt', 'a+').write(str(k) + ' ' + str(v) + '\n')   # 将k，v转换为str类型


if __name__ == '__main__':
    com_tf()
