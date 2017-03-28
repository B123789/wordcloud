#加载所需的库
# -*- coding: utf-8 -*-   
#正则表达式
import re 
#import requests
#import time 
#科学计算
import numpy as np 
#编码转换
import codecs 
#基于numpy的高级数据分析
import pandas 
#解析HTML
from lxml import etree 
#import seaborn as sns
#import os
#词云
from wordcloud import WordCloud 
#图标绘制
import matplotlib.pyplot as plt 
#绘图模式
%matplotlib inline 


#遍历文件中的数据
file = codecs.open("E:\\jupyter\\wordcloud\\123.html",'r')
html = file.read()
file.close()
print(len(html))


#通过相应规则整理数据
page = etree.HTML(html)
chats = page.xpath(u"//pre") #取pre节点

items_list =[]
for chat in chats:
    items_list.append(chat.text) #取文本

print(len(items_list))
print (items_list)[1]


#对list进行处理，取出文字类型的数据并汇总到一个content中
content = str(items_list)
content = content.decode("unicode_escape")

finishcontent=""

punctuation = ']!，,;:?"\'u[？… '
def removePunctuation(text):
    text = re.sub(r'[{}]+'.format(punctuation),'',text)
    return text.strip().lower()

finishcontent = removePunctuation(content)
    
                                                   
print (len(content))
print (len(finishcontent))


#分词 
import jieba
segment = []
seg_list = jieba.cut(finishcontent)
for segs in seg_list:
    if len(segs) > 1:
        segment.append(segs)
        
print(len(segment))


#去停用词
final = ""
final = pandas.DataFrame({'segment':segment})
final.head()

#stopwordsfile = 'E:\\jupyter\\wordcloud\\stopkey.txt'
#stopwords = open(stopwordsfile).read().decode('gbk')

#stopwords = {}.fromkeys(['的','其','或','亦','方','于','即','皆',
#                         '因','仍','故','尚','呢','了','着'])

#stopwords = {}.fromkeys([ line.rstrip() for line in open(stopwordsfile)])

#stopwords = pandas.Series(['其','或','亦','方','于','即','皆',
#                         '因','仍','故','尚','呢','了','的','着',
#                         '" "'])

#for yo in segment:
#    if yo not in stopwords:
#        final += yo

stopwords = pandas.read_csv("E:\\jupyter\\wordcloud\\stopkey.txt",encoding='gbk',
                           index_col = False,quoting = 3,sep = "\t")

final = final[~final.segment.isin(stopwords)]

print (len(stopwords))
print (len(final))
print (stopwords)


#统计词频
words_stat=final.groupby(by=['segment'])['segment'].agg({"number":np.size})
words_stat=words_stat.reset_index().sort_values(by="number",ascending=False)

words_stat.head(20)


import matplotlib
zhfont1 = matplotlib.font_manager.FontProperties(fname='msyh.ttf')
# 设置显示中文
matplotlib.rcParams['font.sans-serif'] = ['msyh'] #指定默认字体
matplotlib.rcParams['axes.unicode_minus'] = False #解决保存图像是负号'-'显示为方块的问题
 
from matplotlib.font_manager import FontProperties
font = FontProperties(fname=r"msyh.ttc", size=14)
 
words_stat[:20].plot(y='number', kind='bar')#x='segment', 中文未能正常显示
 
words_stat[:20].plot(x='segment', y='number', kind='bar')#中文未能正常显示



#照片做词云
from scipy.misc import imread
import matplotlib.pyplot as plt
from wordcloud import WordCloud,ImageColorGenerator
bimg=imread('e:\\jupyter\\wordcloud\\1.jpg')
wordcloud=WordCloud(background_color="black",mask=bimg,font_path='msyh.ttf')
wordcloud=wordcloud.fit_words(words_stat.head(50).itertuples(index=False))
bimgColors=ImageColorGenerator(bimg)
plt.figure(figsize=(100,15))
plt.axis("off")
plt.imshow(wordcloud.recolor(color_func=bimgColors))
plt.show()
