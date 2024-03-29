# Pandas级联    
**Pandas提供了各种工具(功能)，可以轻松地将Series，DataFrame和Panel对象组合在一起**
```
pd.concat(objs,axis=0,join='outer',join_axes=None,ignore_index=False)
```
|参数|作用|
|:-:|:-:|
|objs|这是Series，DataFrame或Panel对象的序列或映射|
|axis|{0，1，...}，默认为0，这是连接的轴|
|join|{'inner', 'outer'}，默认inner。如何处理其他轴上的索引。联合的外部和交叉的内部|
|join_axes|这是Index对象的列表。用于其他(n-1)轴的特定索引，而不是执行内部/外部集逻辑|
|join_index|布尔值，默认为False。如果指定为True，则不要使用连接轴上的索引值。结果轴将被标记为：0，...，n-1|


## 连接对象
**concat()函数完成了沿轴执行级联操作的所有重要工作**
```
import pandas as pd
one = pd.DataFrame({
         'Name': ['Alex', 'Amy', 'Allen', 'Alice', 'Ayoung'],
         'subject_id':['sub1','sub2','sub4','sub6','sub5'],
         'Marks_scored':[98,90,87,69,78]},
         index=[1,2,3,4,5])
two = pd.DataFrame({
         'Name': ['Billy', 'Brian', 'Bran', 'Bryce', 'Betty'],
         'subject_id':['sub2','sub4','sub3','sub6','sub5'],
         'Marks_scored':[89,80,79,97,88]},
         index=[1,2,3,4,5])
rs = pd.concat([one,two])
print(rs)
```
```
   Marks_scored    Name subject_id
1            98    Alex       sub1
2            90     Amy       sub2
3            87   Allen       sub4
4            69   Alice       sub6
5            78  Ayoung       sub5
1            89   Billy       sub2
2            80   Brian       sub4
3            79    Bran       sub3
4            97   Bryce       sub6
5            88   Betty       sub5
```

**假设想把特定的键与每个碎片的DataFrame关联起来。可以通过使用键参数来实现这一点**
```
import pandas as pd
one = pd.DataFrame({
         'Name': ['Alex', 'Amy', 'Allen', 'Alice', 'Ayoung'],
         'subject_id':['sub1','sub2','sub4','sub6','sub5'],
         'Marks_scored':[98,90,87,69,78]},
         index=[1,2,3,4,5])
two = pd.DataFrame({
         'Name': ['Billy', 'Brian', 'Bran', 'Bryce', 'Betty'],
         'subject_id':['sub2','sub4','sub3','sub6','sub5'],
         'Marks_scored':[89,80,79,97,88]},
         index=[1,2,3,4,5])
rs = pd.concat([one,two],keys=['x','y'])
print(rs)
```
```
     Marks_scored    Name subject_id
x 1            98    Alex       sub1
  2            90     Amy       sub2
  3            87   Allen       sub4
  4            69   Alice       sub6
  5            78  Ayoung       sub5
y 1            89   Billy       sub2
  2            80   Brian       sub4
  3            79    Bran       sub3
  4            97   Bryce       sub6
  5            88   Betty       sub5
```

**结果的索引是重复的; 每个索引重复。如果想要生成的对象必须遵循自己的索引，请将ignore_index设置为True**
```
import pandas as pd
one = pd.DataFrame({
         'Name': ['Alex', 'Amy', 'Allen', 'Alice', 'Ayoung'],
         'subject_id':['sub1','sub2','sub4','sub6','sub5'],
         'Marks_scored':[98,90,87,69,78]},
         index=[1,2,3,4,5])
two = pd.DataFrame({
         'Name': ['Billy', 'Brian', 'Bran', 'Bryce', 'Betty'],
         'subject_id':['sub2','sub4','sub3','sub6','sub5'],
         'Marks_scored':[89,80,79,97,88]},
         index=[1,2,3,4,5])
rs = pd.concat([one,two],keys=['x','y'],ignore_index=True)

print(rs)
```
```
   Marks_scored    Name subject_id
0            98    Alex       sub1
1            90     Amy       sub2
2            87   Allen       sub4
3            69   Alice       sub6
4            78  Ayoung       sub5
5            89   Billy       sub2
6            80   Brian       sub4
7            79    Bran       sub3
8            97   Bryce       sub6
9            88   Betty       sub5
```

**如果需要沿axis=1添加两个对象，则会添加新列**
```
import pandas as pd
one = pd.DataFrame({
         'Name': ['Alex', 'Amy', 'Allen', 'Alice', 'Ayoung'],
         'subject_id':['sub1','sub2','sub4','sub6','sub5'],
         'Marks_scored':[98,90,87,69,78]},
         index=[1,2,3,4,5])
two = pd.DataFrame({
         'Name': ['Billy', 'Brian', 'Bran', 'Bryce', 'Betty'],
         'subject_id':['sub2','sub4','sub3','sub6','sub5'],
         'Marks_scored':[89,80,79,97,88]},
         index=[1,2,3,4,5])
rs = pd.concat([one,two],axis=1)
print(rs)
```
```
   Marks_scored    Name subject_id  Marks_scored   Name subject_id
1            98    Alex       sub1            89  Billy       sub2
2            90     Amy       sub2            80  Brian       sub4
3            87   Allen       sub4            79   Bran       sub3
4            69   Alice       sub6            97  Bryce       sub6
5            78  Ayoung       sub5            88  Betty       sub5
```


## 使用附加连接（append）  
**连接的一个有用的快捷方式是在Series和DataFrame实例的append方法。这些方法实际上早于concat()方法。 它们沿axis=0连接，即索引**
```
import pandas as pd
one = pd.DataFrame({
         'Name': ['Alex', 'Amy', 'Allen', 'Alice', 'Ayoung'],
         'subject_id':['sub1','sub2','sub4','sub6','sub5'],
         'Marks_scored':[98,90,87,69,78]},
         index=[1,2,3,4,5])
two = pd.DataFrame({
         'Name': ['Billy', 'Brian', 'Bran', 'Bryce', 'Betty'],
         'subject_id':['sub2','sub4','sub3','sub6','sub5'],
         'Marks_scored':[89,80,79,97,88]},
         index=[1,2,3,4,5])
rs = one.append(two)
print(rs)
```
```
   Marks_scored    Name subject_id
1            98    Alex       sub1
2            90     Amy       sub2
3            87   Allen       sub4
4            69   Alice       sub6
5            78  Ayoung       sub5
1            89   Billy       sub2
2            80   Brian       sub4
3            79    Bran       sub3
4            97   Bryce       sub6
5            88   Betty       sub5
```

**append()函数也可以带多个对象**
```
import pandas as pd
one = pd.DataFrame({
         'Name': ['Alex', 'Amy', 'Allen', 'Alice', 'Ayoung'],
         'subject_id':['sub1','sub2','sub4','sub6','sub5'],
         'Marks_scored':[98,90,87,69,78]},
         index=[1,2,3,4,5])

two = pd.DataFrame({
         'Name': ['Billy', 'Brian', 'Bran', 'Bryce', 'Betty'],
         'subject_id':['sub2','sub4','sub3','sub6','sub5'],
         'Marks_scored':[89,80,79,97,88]},
         index=[1,2,3,4,5])
rs = one.append([two,one,two])
print(rs)
```
```
   Marks_scored    Name subject_id
1            98    Alex       sub1
2            90     Amy       sub2
3            87   Allen       sub4
4            69   Alice       sub6
5            78  Ayoung       sub5
1            89   Billy       sub2
2            80   Brian       sub4
3            79    Bran       sub3
4            97   Bryce       sub6
5            88   Betty       sub5
1            98    Alex       sub1
2            90     Amy       sub2
3            87   Allen       sub4
4            69   Alice       sub6
5            78  Ayoung       sub5
1            89   Billy       sub2
2            80   Brian       sub4
3            79    Bran       sub3
4            97   Bryce       sub6
5            88   Betty       sub5
```


## 时间序列   

**Pandas为时间序列数据的工作时间提供了一个强大的工具，尤其是在金融领域。在处理时间序列数据时，我们经常遇到以下情况**
- 生成时间序列
- 将时间序列转换为不同的频率


### 获取当前时间   
**datetime.now()用于获取当前的日期和时间**
```
import pandas as pd
print pd.datetime.now()
```
```
2017-11-03 02:17:45.997992
```

### 创建一个时间戳
**时间戳数据是时间序列数据的最基本类型，它将数值与时间点相关联。 对于Pandas对象来说，意味着使用时间点**
```
import pandas as pd
time = pd.Timestamp('2018-11-01')
print(time)
```
```
2018-11-01 00:00:00
```
**也可以转换整数或浮动时期。这些的默认单位是纳秒(因为这些是如何存储时间戳的)。 然而，时代往往存储在另一个可以指定的单元中**
```
import pandas as pd
time = pd.Timestamp(1588686880,unit='s')
print(time)
```
```
2020-05-05 13:54:40
```

### 创建一个时间范围   
```
import pandas as pd
time = pd.date_range("12:00", "23:59", freq="30min").time
print(time)
```
```
[datetime.time(12, 0) datetime.time(12, 30) datetime.time(13, 0)
 datetime.time(13, 30) datetime.time(14, 0) datetime.time(14, 30)
 datetime.time(15, 0) datetime.time(15, 30) datetime.time(16, 0)
 datetime.time(16, 30) datetime.time(17, 0) datetime.time(17, 30)
 datetime.time(18, 0) datetime.time(18, 30) datetime.time(19, 0)
 datetime.time(19, 30) datetime.time(20, 0) datetime.time(20, 30)
 datetime.time(21, 0) datetime.time(21, 30) datetime.time(22, 0)
 datetime.time(22, 30) datetime.time(23, 0) datetime.time(23, 30)]
```

### 改变时间的频率
```
import pandas as pd

time = pd.date_range("12:00", "23:59", freq="H").time
print(time)
```
```
[datetime.time(12, 0) datetime.time(13, 0) datetime.time(14, 0)
 datetime.time(15, 0) datetime.time(16, 0) datetime.time(17, 0)
 datetime.time(18, 0) datetime.time(19, 0) datetime.time(20, 0)
 datetime.time(21, 0) datetime.time(22, 0) datetime.time(23, 0)]
```

### 转换为时间戳

**要转换类似日期的对象(例如字符串，时代或混合)的序列或类似列表的对象，可以使用to_datetime函数。当传递时将返回一个Series(具有相同的索引)，
而类似列表被转换为DatetimeIndex**
```
import pandas as pd
time = pd.to_datetime(pd.Series(['Jul 31, 2009','2019-10-10', None]))
print(time)
```
```
0   2009-07-31
1   2019-10-10
2          NaT
dtype: datetime64[ns]
```

```
import pandas as pd
import pandas as pd
time = pd.to_datetime(['2009/11/23', '2019.12.31', None])
print(time)
```
```
DatetimeIndex(['2009-11-23', '2019-12-31', 'NaT'], dtype='datetime64[ns]', freq=None)
```



























