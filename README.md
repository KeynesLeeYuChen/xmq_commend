
# 推荐规则

#### 规则一：每个用户的好友(或粉丝)中与该用户具有相同兴趣的人占多数  
#### 规则二：与用户越相似的好友或粉丝，其标签越可能是用户的标签  
#### 规则三：用户与某好友互动频率越高，用户与该好友的兴趣相似度越大（包括评论、动态、聊天）  

### 一. 用户标签规则
因为APP注册过程并没有强制性的添加兴趣爱好，所以用户一些相对标签无法获得，只能获得用户地址，学校，专业，毕业时间等。  
这个时候需要给用户一些默认的标签，按照用户所在学校的人群大类兴趣，从学校的用户的已有数据，进行推导，把大类标签作为用户的默认标签。

1.学校标签  
通过学校的共同性进行推荐，清华的学生更多地推荐清华的学生  

2.专业标签  
通过专业的共同性进行推荐，但是因为有很多交叉学科（比如UI设计就是设计和心理学、计算机的交叉学科，这样可以给用户打上不同的用户标签，推荐多个设计、心理学、计算机的个集或者交集的用户）、相同大类用户（比如文科类用户推荐文科用户、理科类用户推荐理科用户、工科推荐工科或者理科用户） 

3.学院标签
通过学院的共同性进行推荐，经济学院的学生更容易和经济学院的学生形成交流

3.年限推荐  
大学四年时间内，所接触到的各个年级学生，更容易成为密集交流者  
  
4.兴趣推荐  
1)用户设定兴趣的推荐  
  通过用户自己设定的兴趣进行推荐，但是因为用户的“设定行为”具有不确定性，所以对于用户的准确的兴趣有一定的标准差。  
2)用户行为兴趣的推荐  
  ①用户好友的兴趣标签：取出现最多的几个标签作为用户隐性标签。  
  ②用户参与活动的兴趣标签：因为活动没有设定标签，可以通过参与用户中出现最多的几个标签，作为活动的标签，这样也可返回为用户兴趣标签。

### 二.用户价值
用户价值暂时定为用户使用频率（单日内打开次数）、用户使用时长（单日内所有时长）、用户影响指数（动态被参与次数，如转发、评论、点赞等）  

#### 1.使用频率与用户使用时长  
用以衡定用户活跃度，如果长时间没有使用，那么用户对于平台的价值会下降。  

#### 2.用户影响指数  
##### 规则一：影响力越高的用户回复的动态的影响力越高，从而使该动态主人的影响力变高。  
  
##### 规则二：影响力越高的用户转发的动态的影响力越高，从而使该动态原创作者的影响力变高。  
  
##### 规则三：影响力越高的用户倾向于在其动态中@影响力高或者的用户。  
  
PageRank算法：  
1.赋予所有用户相同的权重。  
2.将每个用户的影响力权重按照其关注的人数等量分配  
3.对每个用户来说，其影响力等于其粉丝分配给他的权重之和  
4.第2步和第3步迭代，直到权重不再发生大的变化为止  

运用pagerank算法，可以计算出不同的用户，不同的影响力权重。
但是这个权重有很大的不稳定性，如果有人因为一些原因，关注的人和好友突涨，那么整体的影响力会上升特别快。
用户的影响力除了他的动态关系之外，还与他的个人属性有很大的关系，比如动态的质量。动态的质量可以采用其被转发的数目、被回复的数目、被点赞的数目来得到。

### 三.用户相似度
运用余弦相似度来衡定用户的相似性，加入权重因素，让衡定结果更准确。  

##### 1.余弦相似度
余弦夹角公式： 
$$ \cos\theta = \frac{\alpha*\beta}{|\alpha|·|\beta|} $$  

公式变形后：
$$ \cos\theta = \frac{\alpha*\beta}{\sqrt{\alpha^2}·\sqrt{\beta^2}} $$

运用：   
  
 用户  |  学校  |  专业  | 兴趣大类 | 兴趣小类  
user1 | 锦城大学| 经济学 |   体育   |  足球  
user2 | 锦城大学| 保险学 |   体育   |  足球  

转换为用户矩阵：  
 用户  |  锦城大学  |  经济学  |  保险学  |  体育  |  足球  
user1 |      1    |    1    |    0    |   1   |    1  
user2 |      1    |    0    |    1    |   1   |    1  

则两个用户的相似度为：
$$\because \cos\theta = \frac{1\times1+1\times0+0\times1+1\times1+1\times1}{\sqrt{1^2+1^2+0^2+1^2+1^2}\times\sqrt{1^2+0^2+1^2+1^2+1^2}} $$  

$$\therefore \cos\theta = \frac{3}{4} = 0.75$$  

权重算法：对于大类加上权重，比如锦城大学、比如体育  
 用户  |  锦城大学  |  经济学  |  保险学  |  体育  |  足球  
user1 |      2    |    1    |    0    |   2   |    1  
user2 |      2    |    0    |    1    |   2   |    1  

$$\because \cos\theta = \frac{2\times2+1\times0+0\times1+2\times2+1\times1}{\sqrt{2^2+1^2+0^2+2^2+1^2}\times\sqrt{2^2+0^2+1^2+2^2+1^2}} $$  

$$\therefore \cos\theta = \frac{9}{10} = 0.9$$  
  
加上权重过后，相同的会表现出更好的相同性，但是不同的，就会更不同。  
##### 2.余弦相似度变形  
对于公式再理解，可以看出其实就是相同的特征在所有的特征中的占比。  

$$\because \cos\theta = \frac{相同的特征}{所有特征} $$  
$$\therefore \cos\theta = \frac{user1_s\bigcap user2_s}{user1_s\bigcup user2_s}$$

### 四.用户联系系数  
##### 1.用户互动系数  
用户互动系数为不同用户之间的交流系数，包括用户的所有互动（交流、评论、点赞、转发、参与）  

$$ 
设 交流为X_1、评论为X_2、转发为X_3、点赞为X_4、参与X_5,\therefore \lambda_1>\lambda_2>\lambda_3>\lambda_4>\lambda_5
$$  
$$
用户互动系数 = \log\sum^n(\lambda_1 X_1 + \lambda_2 X_2+\lambda_3 X_3+\lambda_4 X_4+\lambda_5 X_5)
$$  

##### 2.用户互动相似系数
用户A有两个朋友用户B与用户C，用户B和用户A的互动系数等于30，用户C与用户A的互动系数等于31，则用户B到用户A的互动相似系数为30/31=0.96  

$$  
设 A\to B = 30,C\to B =31
$$  
$$
用户互动相似系数 = \frac{min(A\to B,C\to B)}{max(A\to B,C\to B)}
$$   

这样推荐出来的用户，更容易三人为团，形成一个社交圈


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')
%matplotlib inline
```


```python
user_data = pd.read_csv('../data/user_data.csv')
```


```python
user_data = user_data.drop('Unnamed: 0',axis=1)
```

#### 创建模拟用户
用以寻找相近的用户


```python
userA = pd.Series([
    101,
    '四川',
    '成都市',
    '西华大学',
    '经济学院',
    '文学',
    '经济学',
    '影视',
    '美丽人生',
    '2011',
    '技能',
    '编程',
    '名人',
    '搞笑漫画日和',
    '宋冬野',
    '搞笑漫画日和',
    '妖精的尾巴',
    '七龙珠',
    '搞笑漫画日和'
])
```

### $$ 推荐系数 = \log(\lambda_1 * 互动系数 + \lambda_2 * 用户价值 + \lambda_3 * 用户相似度) $$

构建虚拟用户互动系数和用户价值


```python
user_data['similarity'] = ''
```


```python
score_list = []
```


```python
userA_set = set(userA)
userB_set = set(userA)
for i in range(0,len(user_data)):
    userB_set = set(user_data.loc[i])
    score = len(userA_set&userB_set)/(np.sqrt(len(userA_set))*np.sqrt(len(userB_set)))
    score_list.append(score)
```


```python
user_data['similarity'] = score_list
```


```python
user_data['value'] = ''
user_data['value'] = user_data['value'].apply(lambda x : np.random.choice(range(1,10)))
```


```python
user_data['connect'] = ''
user_data['connect'] = user_data['connect'].apply(lambda x : np.random.choice(range(1,10)))
```


```python
for i in range(0,len(user_data)):
    user_data['score'][i] = np.log10(0.2*user_data['connect'][i]+0.2*user_data['value'][i]+12*user_data['similarity'][i])
```


```python
user_data.sort_values('score',ascending=False)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>region_parent</th>
      <th>region_detail</th>
      <th>school_parent</th>
      <th>school_detail</th>
      <th>major_parent</th>
      <th>major_detail</th>
      <th>interest_parent</th>
      <th>interest_detail</th>
      <th>interest_parent1</th>
      <th>...</th>
      <th>interest_detail2</th>
      <th>year</th>
      <th>interest_detail3</th>
      <th>interest_detail4</th>
      <th>interest_detail5</th>
      <th>interest_detail6</th>
      <th>similarity</th>
      <th>value</th>
      <th>connect</th>
      <th>score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>154</th>
      <td>10154</td>
      <td>四川</td>
      <td>成都市</td>
      <td>西华大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>经济学</td>
      <td>影视</td>
      <td>美丽人生</td>
      <td>技能</td>
      <td>...</td>
      <td>辛普森的一家</td>
      <td>2008</td>
      <td>搞笑漫画日和</td>
      <td>妖精的尾巴</td>
      <td>七龙珠</td>
      <td>搞笑漫画日和</td>
      <td>0.596559</td>
      <td>4</td>
      <td>1</td>
      <td>0.911621</td>
    </tr>
    <tr>
      <th>213</th>
      <td>10213</td>
      <td>四川</td>
      <td>内江市</td>
      <td>西南财经大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>经济学</td>
      <td>技能</td>
      <td>编程</td>
      <td>影视</td>
      <td>...</td>
      <td>灌篮高手</td>
      <td>2004</td>
      <td>死神</td>
      <td>七龙珠</td>
      <td>命运石之门</td>
      <td>海贼王</td>
      <td>0.370479</td>
      <td>8</td>
      <td>9</td>
      <td>0.894635</td>
    </tr>
    <tr>
      <th>36</th>
      <td>10036</td>
      <td>四川</td>
      <td>泸州市</td>
      <td>西南石油大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>电子商务</td>
      <td>影视</td>
      <td>阿甘正传</td>
      <td>美食</td>
      <td>...</td>
      <td>编程</td>
      <td>2011</td>
      <td>绘画</td>
      <td>编程</td>
      <td>绘画</td>
      <td>文案</td>
      <td>0.325396</td>
      <td>9</td>
      <td>7</td>
      <td>0.851549</td>
    </tr>
    <tr>
      <th>674</th>
      <td>10674</td>
      <td>四川</td>
      <td>内江市</td>
      <td>西华大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>电子商务</td>
      <td>名人</td>
      <td>冯小刚</td>
      <td>名人</td>
      <td>...</td>
      <td>迈克尔·杰克逊</td>
      <td>2010</td>
      <td>林允</td>
      <td>冯小刚</td>
      <td>杨幂</td>
      <td>泰勒·斯威夫特</td>
      <td>0.285831</td>
      <td>9</td>
      <td>9</td>
      <td>0.846954</td>
    </tr>
    <tr>
      <th>386</th>
      <td>10386</td>
      <td>四川</td>
      <td>攀枝花市</td>
      <td>四川师范大学</td>
      <td>建筑学院</td>
      <td>工学</td>
      <td>建筑学</td>
      <td>名人</td>
      <td>迈克尔·杰克逊</td>
      <td>名人</td>
      <td>...</td>
      <td>辛普森的一家</td>
      <td>2003</td>
      <td>妖精的尾巴</td>
      <td>七龙珠</td>
      <td>七龙珠</td>
      <td>妖精的尾巴</td>
      <td>0.278207</td>
      <td>9</td>
      <td>9</td>
      <td>0.841265</td>
    </tr>
    <tr>
      <th>713</th>
      <td>10713</td>
      <td>四川</td>
      <td>雅安市</td>
      <td>成都信息工程大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>国际经济与贸易</td>
      <td>综艺</td>
      <td>演员的诞生</td>
      <td>名人</td>
      <td>...</td>
      <td>摄影</td>
      <td>2008</td>
      <td>文案</td>
      <td>文案</td>
      <td>料理</td>
      <td>编程</td>
      <td>0.370479</td>
      <td>3</td>
      <td>9</td>
      <td>0.835421</td>
    </tr>
    <tr>
      <th>785</th>
      <td>10785</td>
      <td>四川</td>
      <td>德阳市</td>
      <td>西南石油大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>金融学</td>
      <td>名人</td>
      <td>GALA</td>
      <td>影视</td>
      <td>...</td>
      <td>电子音乐</td>
      <td>2000</td>
      <td>电子音乐</td>
      <td>绘画</td>
      <td>编程</td>
      <td>书法</td>
      <td>0.370479</td>
      <td>4</td>
      <td>8</td>
      <td>0.835421</td>
    </tr>
    <tr>
      <th>655</th>
      <td>10655</td>
      <td>四川</td>
      <td>成都市</td>
      <td>成都信息工程大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>金融学</td>
      <td>影视</td>
      <td>泰坦尼克号</td>
      <td>美食</td>
      <td>...</td>
      <td>策划</td>
      <td>2008</td>
      <td>写作</td>
      <td>写作</td>
      <td>设计</td>
      <td>文案</td>
      <td>0.317554</td>
      <td>8</td>
      <td>7</td>
      <td>0.833188</td>
    </tr>
    <tr>
      <th>843</th>
      <td>10843</td>
      <td>四川</td>
      <td>资阳市</td>
      <td>西华大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>人力资源</td>
      <td>影视</td>
      <td>霸王别姬</td>
      <td>美食</td>
      <td>...</td>
      <td>电子音乐</td>
      <td>2015</td>
      <td>编程</td>
      <td>设计</td>
      <td>设计</td>
      <td>绘画</td>
      <td>0.271163</td>
      <td>9</td>
      <td>8</td>
      <td>0.823080</td>
    </tr>
    <tr>
      <th>829</th>
      <td>10829</td>
      <td>四川</td>
      <td>成都市</td>
      <td>西华大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>经济学</td>
      <td>综艺</td>
      <td>演员的诞生</td>
      <td>动漫</td>
      <td>...</td>
      <td>文案</td>
      <td>2001</td>
      <td>摄影</td>
      <td>花艺</td>
      <td>绘画</td>
      <td>书法</td>
      <td>0.370479</td>
      <td>2</td>
      <td>9</td>
      <td>0.822544</td>
    </tr>
    <tr>
      <th>647</th>
      <td>10647</td>
      <td>四川</td>
      <td>自贡市</td>
      <td>西南交通大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>金融学</td>
      <td>名人</td>
      <td>迈克尔·杰克逊</td>
      <td>美食</td>
      <td>...</td>
      <td>中国惊奇先生</td>
      <td>2004</td>
      <td>死神</td>
      <td>妖精的尾巴</td>
      <td>七龙珠</td>
      <td>死神</td>
      <td>0.317554</td>
      <td>5</td>
      <td>9</td>
      <td>0.820244</td>
    </tr>
    <tr>
      <th>614</th>
      <td>10614</td>
      <td>四川</td>
      <td>泸州市</td>
      <td>西南石油大学</td>
      <td>土木与环境工程学院</td>
      <td>工学</td>
      <td>土木工程</td>
      <td>动漫</td>
      <td>妖精的尾巴</td>
      <td>影视</td>
      <td>...</td>
      <td>妖精的尾巴</td>
      <td>2007</td>
      <td>灌篮高手</td>
      <td>七龙珠</td>
      <td>搞笑漫画日和</td>
      <td>妖精的尾巴</td>
      <td>0.278207</td>
      <td>8</td>
      <td>8</td>
      <td>0.815477</td>
    </tr>
    <tr>
      <th>744</th>
      <td>10744</td>
      <td>四川</td>
      <td>攀枝花市</td>
      <td>西南财经大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>人力资源</td>
      <td>动漫</td>
      <td>七龙珠</td>
      <td>技能</td>
      <td>...</td>
      <td>春娇与志明</td>
      <td>2016</td>
      <td>环太平洋</td>
      <td>漫威</td>
      <td>美丽人生</td>
      <td>海豚湾</td>
      <td>0.258544</td>
      <td>9</td>
      <td>8</td>
      <td>0.813082</td>
    </tr>
    <tr>
      <th>985</th>
      <td>10985</td>
      <td>四川</td>
      <td>成都市</td>
      <td>成都理工大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>经济学</td>
      <td>名人</td>
      <td>迪丽热巴</td>
      <td>影视</td>
      <td>...</td>
      <td>绘画</td>
      <td>2010</td>
      <td>编程</td>
      <td>摄影</td>
      <td>策划</td>
      <td>文案</td>
      <td>0.423405</td>
      <td>3</td>
      <td>4</td>
      <td>0.811633</td>
    </tr>
    <tr>
      <th>274</th>
      <td>10274</td>
      <td>四川</td>
      <td>广元市</td>
      <td>西南石油大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>经济学</td>
      <td>名人</td>
      <td>冯小刚</td>
      <td>综艺</td>
      <td>...</td>
      <td>海贼王</td>
      <td>2006</td>
      <td>妖精的尾巴</td>
      <td>火影忍者</td>
      <td>火影忍者</td>
      <td>命运石之门</td>
      <td>0.271163</td>
      <td>7</td>
      <td>9</td>
      <td>0.809826</td>
    </tr>
    <tr>
      <th>460</th>
      <td>10460</td>
      <td>四川</td>
      <td>南充市</td>
      <td>四川大学锦城学院</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>经济学</td>
      <td>技能</td>
      <td>编程</td>
      <td>名人</td>
      <td>...</td>
      <td>冯小刚</td>
      <td>2012</td>
      <td>GALA</td>
      <td>迈克尔·杰克逊</td>
      <td>冯小刚</td>
      <td>冯小刚</td>
      <td>0.352941</td>
      <td>8</td>
      <td>3</td>
      <td>0.808568</td>
    </tr>
    <tr>
      <th>974</th>
      <td>10974</td>
      <td>四川</td>
      <td>广元市</td>
      <td>电子科技大学</td>
      <td>文学与传媒学院</td>
      <td>文学</td>
      <td>行政管理</td>
      <td>影视</td>
      <td>终结者</td>
      <td>名人</td>
      <td>...</td>
      <td>七龙珠</td>
      <td>2006</td>
      <td>火影忍者</td>
      <td>妖精的尾巴</td>
      <td>七龙珠</td>
      <td>喜羊羊和灰太狼</td>
      <td>0.317554</td>
      <td>7</td>
      <td>6</td>
      <td>0.806902</td>
    </tr>
    <tr>
      <th>346</th>
      <td>10346</td>
      <td>四川</td>
      <td>成都市</td>
      <td>西南石油大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>投资学</td>
      <td>影视</td>
      <td>让子弹飞</td>
      <td>动漫</td>
      <td>...</td>
      <td>中国好声音</td>
      <td>2005</td>
      <td>中国好声音</td>
      <td>真正男子汉</td>
      <td>中国式相亲</td>
      <td>笑傲江湖</td>
      <td>0.317554</td>
      <td>9</td>
      <td>4</td>
      <td>0.806902</td>
    </tr>
    <tr>
      <th>665</th>
      <td>10665</td>
      <td>四川</td>
      <td>成都市</td>
      <td>西南石油大学</td>
      <td>文学与传媒学院</td>
      <td>文学</td>
      <td>汉语言文学</td>
      <td>综艺</td>
      <td>老梁故事汇</td>
      <td>综艺</td>
      <td>...</td>
      <td>阿甘正传</td>
      <td>2004</td>
      <td>美丽人生</td>
      <td>海豚湾</td>
      <td>泰坦尼克号</td>
      <td>复仇者联盟</td>
      <td>0.264628</td>
      <td>8</td>
      <td>8</td>
      <td>0.804517</td>
    </tr>
    <tr>
      <th>576</th>
      <td>10576</td>
      <td>四川</td>
      <td>德阳市</td>
      <td>电子科技大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>电子商务</td>
      <td>动漫</td>
      <td>七龙珠</td>
      <td>影视</td>
      <td>...</td>
      <td>豆腐脑</td>
      <td>2006</td>
      <td>豆腐脑</td>
      <td>卤肉饭</td>
      <td>口水鸡</td>
      <td>糖醋排骨</td>
      <td>0.264628</td>
      <td>9</td>
      <td>7</td>
      <td>0.804517</td>
    </tr>
    <tr>
      <th>895</th>
      <td>10895</td>
      <td>四川</td>
      <td>雅安市</td>
      <td>西华大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>经济学</td>
      <td>综艺</td>
      <td>真正男子汉</td>
      <td>综艺</td>
      <td>...</td>
      <td>一代宗师</td>
      <td>2001</td>
      <td>阿甘正传</td>
      <td>复仇者联盟</td>
      <td>迪斯尼</td>
      <td>复仇者联盟</td>
      <td>0.278207</td>
      <td>9</td>
      <td>6</td>
      <td>0.801986</td>
    </tr>
    <tr>
      <th>445</th>
      <td>10445</td>
      <td>四川</td>
      <td>成都市</td>
      <td>西南石油大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>市场营销</td>
      <td>技能</td>
      <td>绘画</td>
      <td>名人</td>
      <td>...</td>
      <td>环太平洋</td>
      <td>2001</td>
      <td>缝纫机乐队</td>
      <td>漫威</td>
      <td>美丽人生</td>
      <td>帕丁顿熊</td>
      <td>0.310253</td>
      <td>7</td>
      <td>6</td>
      <td>0.800925</td>
    </tr>
    <tr>
      <th>789</th>
      <td>10789</td>
      <td>四川</td>
      <td>泸州市</td>
      <td>西华大学</td>
      <td>电气学院</td>
      <td>工学</td>
      <td>电气工程</td>
      <td>美食</td>
      <td>糖醋排骨</td>
      <td>技能</td>
      <td>...</td>
      <td>海贼王</td>
      <td>2014</td>
      <td>喜羊羊和灰太狼</td>
      <td>七龙珠</td>
      <td>妖精的尾巴</td>
      <td>中国惊奇先生</td>
      <td>0.258544</td>
      <td>8</td>
      <td>8</td>
      <td>0.799515</td>
    </tr>
    <tr>
      <th>224</th>
      <td>10224</td>
      <td>四川</td>
      <td>宜宾市</td>
      <td>成都理工大学</td>
      <td>艺术学院</td>
      <td>艺术学</td>
      <td>播音主持</td>
      <td>名人</td>
      <td>赵丽颖</td>
      <td>动漫</td>
      <td>...</td>
      <td>写作</td>
      <td>2014</td>
      <td>料理</td>
      <td>文案</td>
      <td>书法</td>
      <td>编程</td>
      <td>0.258544</td>
      <td>7</td>
      <td>9</td>
      <td>0.799515</td>
    </tr>
    <tr>
      <th>207</th>
      <td>10207</td>
      <td>四川</td>
      <td>成都市</td>
      <td>西南财经大学</td>
      <td>计算机学院</td>
      <td>工学</td>
      <td>软件工程</td>
      <td>动漫</td>
      <td>七龙珠</td>
      <td>影视</td>
      <td>...</td>
      <td>花艺</td>
      <td>2002</td>
      <td>策划</td>
      <td>写作</td>
      <td>文案</td>
      <td>电子音乐</td>
      <td>0.258544</td>
      <td>9</td>
      <td>7</td>
      <td>0.799515</td>
    </tr>
    <tr>
      <th>545</th>
      <td>10545</td>
      <td>四川</td>
      <td>成都市</td>
      <td>四川大学锦城学院</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>国际经济与贸易</td>
      <td>动漫</td>
      <td>灌篮高手</td>
      <td>美食</td>
      <td>...</td>
      <td>电子音乐</td>
      <td>2004</td>
      <td>设计</td>
      <td>料理</td>
      <td>绘画</td>
      <td>书法</td>
      <td>0.258544</td>
      <td>8</td>
      <td>8</td>
      <td>0.799515</td>
    </tr>
    <tr>
      <th>700</th>
      <td>10700</td>
      <td>四川</td>
      <td>宜宾市</td>
      <td>四川师范大学</td>
      <td>经济学院</td>
      <td>经济学</td>
      <td>经济学</td>
      <td>影视</td>
      <td>寻梦幻游记</td>
      <td>综艺</td>
      <td>...</td>
      <td>林允</td>
      <td>2005</td>
      <td>周杰伦</td>
      <td>吴亦凡</td>
      <td>吴亦凡</td>
      <td>赵丽颖</td>
      <td>0.271163</td>
      <td>8</td>
      <td>7</td>
      <td>0.796155</td>
    </tr>
    <tr>
      <th>992</th>
      <td>10992</td>
      <td>四川</td>
      <td>自贡市</td>
      <td>成都信息工程大学</td>
      <td>文学与传媒学院</td>
      <td>文学</td>
      <td>行政管理</td>
      <td>名人</td>
      <td>宋冬野</td>
      <td>影视</td>
      <td>...</td>
      <td>绘画</td>
      <td>2017</td>
      <td>编程</td>
      <td>电子音乐</td>
      <td>料理</td>
      <td>料理</td>
      <td>0.370479</td>
      <td>6</td>
      <td>3</td>
      <td>0.795585</td>
    </tr>
    <tr>
      <th>958</th>
      <td>10958</td>
      <td>四川</td>
      <td>达州市</td>
      <td>西南石油大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>市场营销</td>
      <td>动漫</td>
      <td>七龙珠</td>
      <td>名人</td>
      <td>...</td>
      <td>绘画</td>
      <td>2012</td>
      <td>电子音乐</td>
      <td>电子音乐</td>
      <td>料理</td>
      <td>写作</td>
      <td>0.211702</td>
      <td>9</td>
      <td>9</td>
      <td>0.788199</td>
    </tr>
    <tr>
      <th>117</th>
      <td>10117</td>
      <td>四川</td>
      <td>德阳市</td>
      <td>西南石油大学</td>
      <td>机械学院</td>
      <td>工学</td>
      <td>测控技术与仪器</td>
      <td>综艺</td>
      <td>创造101</td>
      <td>影视</td>
      <td>...</td>
      <td>设计</td>
      <td>2002</td>
      <td>编程</td>
      <td>设计</td>
      <td>电子音乐</td>
      <td>花艺</td>
      <td>0.211702</td>
      <td>9</td>
      <td>9</td>
      <td>0.788199</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>939</th>
      <td>10939</td>
      <td>四川</td>
      <td>内江市</td>
      <td>四川大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>工商管理</td>
      <td>综艺</td>
      <td>老梁故事汇</td>
      <td>美食</td>
      <td>...</td>
      <td>中国惊奇先生</td>
      <td>2006</td>
      <td>中国惊奇先生</td>
      <td>无头骑士异闻录</td>
      <td>喜羊羊和灰太狼</td>
      <td>命运石之门</td>
      <td>0.052926</td>
      <td>4</td>
      <td>3</td>
      <td>0.308587</td>
    </tr>
    <tr>
      <th>212</th>
      <td>10212</td>
      <td>四川</td>
      <td>攀枝花市</td>
      <td>西南石油大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>工商管理</td>
      <td>动漫</td>
      <td>死神</td>
      <td>综艺</td>
      <td>...</td>
      <td>蒙面唱将猜猜猜</td>
      <td>2004</td>
      <td>真正男子汉</td>
      <td>笑傲江湖</td>
      <td>极限挑张</td>
      <td>我们十七岁</td>
      <td>0.052926</td>
      <td>4</td>
      <td>3</td>
      <td>0.308587</td>
    </tr>
    <tr>
      <th>542</th>
      <td>10542</td>
      <td>重庆</td>
      <td>重庆</td>
      <td>四川大学锦城学院</td>
      <td>机械学院</td>
      <td>工学</td>
      <td>机械电子工程</td>
      <td>动漫</td>
      <td>死神</td>
      <td>美食</td>
      <td>...</td>
      <td>赵丽颖</td>
      <td>2002</td>
      <td>鹿晗</td>
      <td>迪丽热巴</td>
      <td>冯小刚</td>
      <td>张瀚</td>
      <td>0.052926</td>
      <td>1</td>
      <td>6</td>
      <td>0.308587</td>
    </tr>
    <tr>
      <th>20</th>
      <td>10020</td>
      <td>四川</td>
      <td>广元市</td>
      <td>电子科技大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>市场营销</td>
      <td>美食</td>
      <td>火锅</td>
      <td>动漫</td>
      <td>...</td>
      <td>档案</td>
      <td>2003</td>
      <td>天天向上</td>
      <td>快乐大本营</td>
      <td>我们十七岁</td>
      <td>真正男子汉</td>
      <td>0.051709</td>
      <td>3</td>
      <td>4</td>
      <td>0.305460</td>
    </tr>
    <tr>
      <th>551</th>
      <td>10551</td>
      <td>重庆</td>
      <td>重庆</td>
      <td>成都信息工程大学</td>
      <td>计算机学院</td>
      <td>工学</td>
      <td>软件工程</td>
      <td>综艺</td>
      <td>真正男子汉</td>
      <td>美食</td>
      <td>...</td>
      <td>烤肉</td>
      <td>2014</td>
      <td>火锅</td>
      <td>糖醋排骨</td>
      <td>烤肉</td>
      <td>麻婆豆腐</td>
      <td>0.000000</td>
      <td>3</td>
      <td>7</td>
      <td>0.301030</td>
    </tr>
    <tr>
      <th>517</th>
      <td>10517</td>
      <td>重庆</td>
      <td>重庆</td>
      <td>西南石油大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>市场营销</td>
      <td>综艺</td>
      <td>老梁故事汇</td>
      <td>影视</td>
      <td>...</td>
      <td>绘画</td>
      <td>2004</td>
      <td>料理</td>
      <td>摄影</td>
      <td>电子音乐</td>
      <td>料理</td>
      <td>0.111283</td>
      <td>1</td>
      <td>2</td>
      <td>0.286770</td>
    </tr>
    <tr>
      <th>693</th>
      <td>10693</td>
      <td>四川</td>
      <td>成都市</td>
      <td>成都信息工程大学</td>
      <td>土木与环境工程学院</td>
      <td>工学</td>
      <td>建筑工程技术</td>
      <td>综艺</td>
      <td>热血街舞团</td>
      <td>动漫</td>
      <td>...</td>
      <td>档案</td>
      <td>2011</td>
      <td>演员的诞生</td>
      <td>演员的诞生</td>
      <td>创造101</td>
      <td>天天向上</td>
      <td>0.108465</td>
      <td>2</td>
      <td>1</td>
      <td>0.279115</td>
    </tr>
    <tr>
      <th>601</th>
      <td>10601</td>
      <td>重庆</td>
      <td>重庆</td>
      <td>西南石油大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>工商管理</td>
      <td>美食</td>
      <td>卤肉饭</td>
      <td>名人</td>
      <td>...</td>
      <td>糖醋排骨</td>
      <td>2006</td>
      <td>牛肉面</td>
      <td>火锅</td>
      <td>麻婆豆腐</td>
      <td>口水鸡</td>
      <td>0.055641</td>
      <td>5</td>
      <td>1</td>
      <td>0.271307</td>
    </tr>
    <tr>
      <th>240</th>
      <td>10240</td>
      <td>重庆</td>
      <td>重庆</td>
      <td>西南石油大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>市场营销</td>
      <td>动漫</td>
      <td>海贼王</td>
      <td>动漫</td>
      <td>...</td>
      <td>寻梦幻游记</td>
      <td>2009</td>
      <td>迪斯尼</td>
      <td>环太平洋</td>
      <td>让子弹飞</td>
      <td>漫威</td>
      <td>0.054233</td>
      <td>1</td>
      <td>5</td>
      <td>0.267357</td>
    </tr>
    <tr>
      <th>910</th>
      <td>10910</td>
      <td>四川</td>
      <td>自贡市</td>
      <td>西南交通大学</td>
      <td>理学院</td>
      <td>理学</td>
      <td>物理学</td>
      <td>动漫</td>
      <td>中国惊奇先生</td>
      <td>综艺</td>
      <td>...</td>
      <td>豆腐脑</td>
      <td>2003</td>
      <td>串串香</td>
      <td>口水鸡</td>
      <td>串串香</td>
      <td>酸菜鱼</td>
      <td>0.052926</td>
      <td>1</td>
      <td>5</td>
      <td>0.263661</td>
    </tr>
    <tr>
      <th>217</th>
      <td>10217</td>
      <td>重庆</td>
      <td>重庆</td>
      <td>四川大学</td>
      <td>土木与环境工程学院</td>
      <td>工学</td>
      <td>给排水科学与工程</td>
      <td>动漫</td>
      <td>海贼王</td>
      <td>技能</td>
      <td>...</td>
      <td>我们十七岁</td>
      <td>2000</td>
      <td>极限挑张</td>
      <td>笑傲江湖</td>
      <td>热血街舞团</td>
      <td>天天向上</td>
      <td>0.052926</td>
      <td>1</td>
      <td>5</td>
      <td>0.263661</td>
    </tr>
    <tr>
      <th>345</th>
      <td>10345</td>
      <td>四川</td>
      <td>自贡市</td>
      <td>西南财经大学</td>
      <td>土木与环境工程学院</td>
      <td>工学</td>
      <td>建筑工程技术</td>
      <td>美食</td>
      <td>牛肉面</td>
      <td>名人</td>
      <td>...</td>
      <td>火锅</td>
      <td>2005</td>
      <td>寿司</td>
      <td>鱼香肉丝</td>
      <td>串串香</td>
      <td>牛肉面</td>
      <td>0.108465</td>
      <td>1</td>
      <td>1</td>
      <td>0.230853</td>
    </tr>
    <tr>
      <th>802</th>
      <td>10802</td>
      <td>四川</td>
      <td>乐山市</td>
      <td>西南交通大学</td>
      <td>计算机学院</td>
      <td>工学</td>
      <td>软件工程</td>
      <td>影视</td>
      <td>阿甘正传</td>
      <td>美食</td>
      <td>...</td>
      <td>鱼香肉丝</td>
      <td>2002</td>
      <td>烤肉</td>
      <td>麻婆豆腐</td>
      <td>口水鸡</td>
      <td>豆腐脑</td>
      <td>0.108465</td>
      <td>1</td>
      <td>1</td>
      <td>0.230853</td>
    </tr>
    <tr>
      <th>157</th>
      <td>10157</td>
      <td>四川</td>
      <td>宜宾市</td>
      <td>成都理工大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>市场营销</td>
      <td>技能</td>
      <td>摄影</td>
      <td>综艺</td>
      <td>...</td>
      <td>中国好声音</td>
      <td>2004</td>
      <td>天天向上</td>
      <td>快乐大本营</td>
      <td>蒙面唱将猜猜猜</td>
      <td>我们十七岁</td>
      <td>0.108465</td>
      <td>1</td>
      <td>1</td>
      <td>0.230853</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10009</td>
      <td>四川</td>
      <td>眉山市</td>
      <td>西南财经大学</td>
      <td>土木与环境工程学院</td>
      <td>工学</td>
      <td>建筑工程技术</td>
      <td>影视</td>
      <td>复仇者联盟</td>
      <td>美食</td>
      <td>...</td>
      <td>酸菜鱼</td>
      <td>2010</td>
      <td>三汁焖锅</td>
      <td>口水鸡</td>
      <td>豆腐脑</td>
      <td>麻婆豆腐</td>
      <td>0.108465</td>
      <td>1</td>
      <td>1</td>
      <td>0.230853</td>
    </tr>
    <tr>
      <th>158</th>
      <td>10158</td>
      <td>四川</td>
      <td>德阳市</td>
      <td>四川师范大学</td>
      <td>土木与环境工程学院</td>
      <td>工学</td>
      <td>建筑工程技术</td>
      <td>美食</td>
      <td>串串香</td>
      <td>美食</td>
      <td>...</td>
      <td>卤肉饭</td>
      <td>2015</td>
      <td>酸菜鱼</td>
      <td>豆腐脑</td>
      <td>麻婆豆腐</td>
      <td>串串香</td>
      <td>0.057166</td>
      <td>3</td>
      <td>2</td>
      <td>0.226856</td>
    </tr>
    <tr>
      <th>425</th>
      <td>10425</td>
      <td>四川</td>
      <td>眉山市</td>
      <td>电子科技大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>工商管理</td>
      <td>综艺</td>
      <td>老梁故事汇</td>
      <td>名人</td>
      <td>...</td>
      <td>串串香</td>
      <td>2006</td>
      <td>烤肉</td>
      <td>口水鸡</td>
      <td>烤肉</td>
      <td>火锅</td>
      <td>0.105851</td>
      <td>1</td>
      <td>1</td>
      <td>0.222772</td>
    </tr>
    <tr>
      <th>225</th>
      <td>10225</td>
      <td>四川</td>
      <td>德阳市</td>
      <td>四川师范大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>市场营销</td>
      <td>动漫</td>
      <td>海贼王</td>
      <td>影视</td>
      <td>...</td>
      <td>中国好声音</td>
      <td>2014</td>
      <td>创造101</td>
      <td>档案</td>
      <td>天天向上</td>
      <td>蒙面唱将猜猜猜</td>
      <td>0.105851</td>
      <td>1</td>
      <td>1</td>
      <td>0.222772</td>
    </tr>
    <tr>
      <th>45</th>
      <td>10045</td>
      <td>重庆</td>
      <td>重庆</td>
      <td>电子科技大学</td>
      <td>财务会计学院</td>
      <td>管理学</td>
      <td>财务管理</td>
      <td>综艺</td>
      <td>创造101</td>
      <td>美食</td>
      <td>...</td>
      <td>海豚湾</td>
      <td>2005</td>
      <td>霸王别姬</td>
      <td>海豚湾</td>
      <td>复仇者联盟</td>
      <td>霸王别姬</td>
      <td>0.055641</td>
      <td>1</td>
      <td>4</td>
      <td>0.222117</td>
    </tr>
    <tr>
      <th>202</th>
      <td>10202</td>
      <td>重庆</td>
      <td>重庆</td>
      <td>西南石油大学</td>
      <td>土木与环境工程学院</td>
      <td>工学</td>
      <td>给排水科学与工程</td>
      <td>美食</td>
      <td>糖醋排骨</td>
      <td>美食</td>
      <td>...</td>
      <td>复仇者联盟</td>
      <td>2017</td>
      <td>泰坦尼克号</td>
      <td>迪斯尼</td>
      <td>一代宗师</td>
      <td>海豚湾</td>
      <td>0.055641</td>
      <td>4</td>
      <td>1</td>
      <td>0.222117</td>
    </tr>
    <tr>
      <th>141</th>
      <td>10141</td>
      <td>四川</td>
      <td>乐山市</td>
      <td>西南交通大学</td>
      <td>建筑学院</td>
      <td>工学</td>
      <td>建筑学</td>
      <td>综艺</td>
      <td>中国式相亲</td>
      <td>动漫</td>
      <td>...</td>
      <td>烤肉</td>
      <td>2011</td>
      <td>卤肉饭</td>
      <td>卤肉饭</td>
      <td>酸菜鱼</td>
      <td>酸菜鱼</td>
      <td>0.054233</td>
      <td>2</td>
      <td>3</td>
      <td>0.217692</td>
    </tr>
    <tr>
      <th>768</th>
      <td>10768</td>
      <td>四川</td>
      <td>乐山市</td>
      <td>四川大学</td>
      <td>建筑学院</td>
      <td>工学</td>
      <td>建筑学</td>
      <td>综艺</td>
      <td>天天向上</td>
      <td>动漫</td>
      <td>...</td>
      <td>缝纫机乐队</td>
      <td>2017</td>
      <td>迪斯尼</td>
      <td>阿甘正传</td>
      <td>环太平洋</td>
      <td>海豚湾</td>
      <td>0.103418</td>
      <td>1</td>
      <td>1</td>
      <td>0.215111</td>
    </tr>
    <tr>
      <th>741</th>
      <td>10741</td>
      <td>四川</td>
      <td>广安市</td>
      <td>成都信息工程大学</td>
      <td>土木与环境工程学院</td>
      <td>工学</td>
      <td>土木工程</td>
      <td>综艺</td>
      <td>极限挑张</td>
      <td>美食</td>
      <td>...</td>
      <td>麻婆豆腐</td>
      <td>2015</td>
      <td>糖醋排骨</td>
      <td>酸菜鱼</td>
      <td>酸菜鱼</td>
      <td>牛肉面</td>
      <td>0.055641</td>
      <td>3</td>
      <td>1</td>
      <td>0.166637</td>
    </tr>
    <tr>
      <th>113</th>
      <td>10113</td>
      <td>四川</td>
      <td>泸州市</td>
      <td>四川师范大学</td>
      <td>机械学院</td>
      <td>工学</td>
      <td>机械设计与制造</td>
      <td>综艺</td>
      <td>蒙面唱将猜猜猜</td>
      <td>动漫</td>
      <td>...</td>
      <td>三汁焖锅</td>
      <td>2012</td>
      <td>豆腐脑</td>
      <td>寿司</td>
      <td>寿司</td>
      <td>鱼香肉丝</td>
      <td>0.052926</td>
      <td>2</td>
      <td>2</td>
      <td>0.156884</td>
    </tr>
    <tr>
      <th>796</th>
      <td>10796</td>
      <td>重庆</td>
      <td>重庆</td>
      <td>西南石油大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>人力资源</td>
      <td>动漫</td>
      <td>中国惊奇先生</td>
      <td>综艺</td>
      <td>...</td>
      <td>奔跑吧</td>
      <td>2007</td>
      <td>真正男子汉</td>
      <td>档案</td>
      <td>极限挑张</td>
      <td>非诚勿扰</td>
      <td>0.000000</td>
      <td>3</td>
      <td>4</td>
      <td>0.146128</td>
    </tr>
    <tr>
      <th>807</th>
      <td>10807</td>
      <td>四川</td>
      <td>自贡市</td>
      <td>西南石油大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>工商管理</td>
      <td>动漫</td>
      <td>中国惊奇先生</td>
      <td>动漫</td>
      <td>...</td>
      <td>中国好声音</td>
      <td>2001</td>
      <td>创造101</td>
      <td>非诚勿扰</td>
      <td>非诚勿扰</td>
      <td>创造101</td>
      <td>0.057166</td>
      <td>2</td>
      <td>1</td>
      <td>0.109239</td>
    </tr>
    <tr>
      <th>245</th>
      <td>10245</td>
      <td>四川</td>
      <td>德阳市</td>
      <td>西南石油大学</td>
      <td>财务会计学院</td>
      <td>管理学</td>
      <td>会计</td>
      <td>动漫</td>
      <td>海贼王</td>
      <td>动漫</td>
      <td>...</td>
      <td>串串香</td>
      <td>2002</td>
      <td>豆腐脑</td>
      <td>串串香</td>
      <td>鱼香肉丝</td>
      <td>牛肉面</td>
      <td>0.054233</td>
      <td>1</td>
      <td>2</td>
      <td>0.097185</td>
    </tr>
    <tr>
      <th>865</th>
      <td>10865</td>
      <td>四川</td>
      <td>巴中市</td>
      <td>西南财经大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>市场营销</td>
      <td>美食</td>
      <td>串串香</td>
      <td>综艺</td>
      <td>...</td>
      <td>钵钵鸡</td>
      <td>2013</td>
      <td>牛肉面</td>
      <td>钵钵鸡</td>
      <td>钵钵鸡</td>
      <td>麻婆豆腐</td>
      <td>0.055641</td>
      <td>1</td>
      <td>1</td>
      <td>0.028448</td>
    </tr>
    <tr>
      <th>429</th>
      <td>10429</td>
      <td>重庆</td>
      <td>重庆</td>
      <td>西南交通大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>人力资源</td>
      <td>综艺</td>
      <td>笑傲江湖</td>
      <td>美食</td>
      <td>...</td>
      <td>泰坦尼克号</td>
      <td>2016</td>
      <td>环太平洋</td>
      <td>泰坦尼克号</td>
      <td>一代宗师</td>
      <td>海豚湾</td>
      <td>0.054233</td>
      <td>1</td>
      <td>1</td>
      <td>0.021516</td>
    </tr>
    <tr>
      <th>956</th>
      <td>10956</td>
      <td>重庆</td>
      <td>重庆</td>
      <td>西南交通大学</td>
      <td>管理学院</td>
      <td>管理学</td>
      <td>工商管理</td>
      <td>美食</td>
      <td>酸菜鱼</td>
      <td>综艺</td>
      <td>...</td>
      <td>糖醋排骨</td>
      <td>2000</td>
      <td>寿司</td>
      <td>牛肉面</td>
      <td>鱼香肉丝</td>
      <td>口水鸡</td>
      <td>0.000000</td>
      <td>1</td>
      <td>2</td>
      <td>-0.221849</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 22 columns</p>
</div>




```python
set(user_data.loc[5,:])&set(user_data.loc[788,:])
```


```python

```
