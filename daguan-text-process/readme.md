
# "达观杯"文本智能处理挑战赛-长文本分类-rank4

非常感谢达观杯给我们提供这次机会以及科赛平台提供了很棒的GPU，再次感谢。


### 赛题网址：

[“达观杯”文本智能处理挑战赛](http://www.dcjingsai.com/common/cmpt/%E2%80%9C%E8%BE%BE%E8%A7%82%E6%9D%AF%E2%80%9D%E6%96%87%E6%9C%AC%E6%99%BA%E8%83%BD%E5%A4%84%E7%90%86%E6%8C%91%E6%88%98%E8%B5%9B_%E7%AB%9E%E8%B5%9B%E4%BF%A1%E6%81%AF.html)

### 任务：

达观数据提供了一批长文本数据和分类信息，结合当下最先进的NLP和人工智能技术，深入分析文本内在结构和语义信息，构建文本分类模型，实现精准分类。

### 数据集

在微信公众号：机器学习社区，后台回复：达观文本处理大赛数据

![image](https://user-images.githubusercontent.com/76510785/233755858-0750c799-3328-40ba-8fe7-5b2ede7cab01.png)

### 技术交流

前沿技术资讯、算法技术交流、求职内推、算法竞赛、职场面试经验交流(校招、社招、实习)等，与 10000+来自港大、北大、清华、中科院、CMU、腾讯、百度、微软等名校名企开发者互动交流~

![image](https://user-images.githubusercontent.com/76510785/233755874-bab91ca7-92b1-48a3-a6f9-631ce75b0f94.png)


### 解决方案：

由于部分代码暂时有用，现在只公开一个单模型：B榜单模型分数可达到0.798.

对于这个文本分类任务，有个小的操作其实都可以达到很高的分数，即使模型不够优秀。通过对于词向量做一个增强，即利用word2vec与glove的差异性，构建一个鲁棒性更高的词语向量表征。大家也可以试试word2vec+glove+faxttext的组合，对于我来说，效果并不是很好，我觉得可能的原因是faxttext与word2vec的相似性很高，弱化了glove的向量表征，同时，对于glove单独的词向量我也没有尝试过，大家也可以尝试一下。

对于模型的话，我开源了一个双层的biGruModel模型，最近也开源了rnnCapsuleModel，希望大家可以取得更好的成绩！



### 运行环境

- tensorflow-gpu>=1.10.0

- keras==2.16.0

- gensim==3.6.0

- scikit-learn==0.20.2 


### 模型运行：

1、将原始数据集input到data文件夹

2、运行 python read_data.py,从而将原始数据的csv格式转化为feather格式（因为feather格式读取数据较快）

3、由于应用到glove算法生成词向量和字向量，且没有python接口，我们使用斯坦福大学开源的C语言版本的glove库。

   生成词向量
   
  （1）python glove_word.py (生成glove所需要的格式的词向量)
  
  （2） make & sh glove_word.sh (生成词向量)
  
  （3）将生成的词向量(glove_vectors_word.txt)放入embedding 文件夹下
  ```shell
  python glove_word.py
  make & sh glove_word.sh
  ```
  
   生成字向量
     
  （1）python glove_char.py (生成glove所需要的格式的字向量)
  
  （2） make & sh glove_char.sh (生成字向量)
  
  （3）将生成的词向量(glove_vectors_char.txt)放入embedding 文件夹下
  
  ``` shell
  python glove_char.py
  make & sh glove_char.sh
  ```
   
4、运行模型：

biGruModel:
```shell
CUDA_VISIBLE_DEVICES=0 python main_glove_word2vec.py  --gpu="0" --column_name="word_seg" --word_seq_len=1800 --embedding_vector=200 --num_words=500000 --model_name="bi_gru_model" --batch_size=128 --KFold=10 --classification=19
```

rnnCapsuleModel:
```shell
CUDA_VISIBLE_DEVICES=0 python main.py  --gpu="0" --column_name="word_seg" --word_seq_len=1800 --embedding_vector=200 --num_words=500000 --model_name="Gru_Capsule_Model" --batch_size=128 --KFold=10 --classification=19
```


备注：如果gpu 较小，batch_size 可以设置较小一点
  


所有的命令都封装在 sh run.sh （很简单一个命令）！
```shell
sh run.sh
```

大概的先介绍到这里，有时间在介绍啦！

