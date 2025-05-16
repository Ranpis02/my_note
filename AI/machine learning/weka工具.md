# 1 getting started with weka

## 1.1 Exploring the Explorer

从 data 文件件中导入数据，然后进行可视化观察

<img src="https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20241024105416065.png" alt="image-20241024105416065" style="zoom:50%;" />

## 1.2 Data Format

Weka 的数据集是通过 `.arff` 文件进行存储的，我们以 `weather.nominal.arff` 文件为例，其格式如下：

```css
% weather.nominal.arff
@relation weather

@attribute outlook {sunny, overcast, rainy}
@attribute temperature numeric
@attribute humidity numeric
@attribute windy {TRUE, FALSE}
@attribute play {yes, no}

@data
sunny,85,85,FALSE,no
sunny,80,90,TRUE,no
overcast,83,86,FALSE,yes
rainy,70,96,FALSE,yes
rainy,68,80,FALSE,yes
rainy,65,70,TRUE,no
overcast,64,65,TRUE,yes
sunny,72,95,FALSE,no
sunny,69,70,FALSE,yes
rainy,75,80,FALSE,yes
sunny,75,70,TRUE,yes
overcast,72,90,TRUE,yes
overcast,81,75,FALSE,yes
rainy,71,91,TRUE,no
```

其中:

- `%`: 注释行，用于指示数据源、上下文以及含义
- `@relation`：用于声明数据集的内部名称
- `@attribute`：用于声明属性，对于数值属性(numeric attribute)，我们声明的方式为 `@attribute <attr_name> numeric`；而对于名义属性，我们的声明的方式为 `@attribute <attr_name> {<attr_value...>}` 
- `@data`：后面接数据部分，数据使用 `,` 分割，每一列代表特征，每一行代表一个样本，特征的索引需要和声明的属性索引一一对应



## 1.3 building a classifier

![image-20241024154302222](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20241024154302222.png)



## 1.4 use a filter

![image-20241024162806026](https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20241024162806026.png)



## 1.5 visualizing your data



# 2 evaluation

## 2.1 be a classifier

