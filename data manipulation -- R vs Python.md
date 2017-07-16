

R语言里面，点(.)是可以在变量名里的。Python则不可以，它是用来作为获取对象的属性的。



# 筛选行

```R
iris %>%
    filter(Sepal.Length > 5, Sepal.Width<3)
```

```python
iris.query("Sepal_Length > 5 and Sepal_Width<3")
```

R里面不需要加引号。

# 筛选列

```R
iris %>%
    select(Sepal.Length, Species)
# or
iris[,c("Sepal.Length", "Species")]
```

python

```python
iris[['Sepal_Length', 'Species']]
iris.loc[:, ['Sepal_Length', 'Species']]
```

# 删除列

###### Python

```python
del iris['Species']
iris.drop(['Species'])
```

###### R

```R
DF[, 'city'] <- NULL
select(DF, -city)
```

# 重命名

python

```python
iris.rename(columns = {'Species':'species'})
```

R

```R
df %>%
	reanme(species = Species)
```



# 排序

python

```python
iris.sort_values(by=['Sepal_Length', 'Sepal_Width'], ascending=[False, True])
```

R

```R
iris %>%
	arrange(desc(Sepal.Length), Sepal.Width)
```

# 去重

python

```python
iris.drop_duplicates()
# or
iris.Species.unqiue()
# or 
iris.groupby('Speceis').size().index
```

R

```R
iris %>%
	distinct(Species)
```

# 变换

python

```python
iris.assign(sepal_ratio = lambda df:df.Sepal_Length / df.Sepal_Width)
```

R

```R
iris %>%
	mutate(Sepal.Ratio = Sepal.Length / Sepal.Width)
```

如果仅仅想保留新的变量，R里面有函数transmute, python 没有对应的。但是也不难。

# 分组

python

```python
iris.groupby(['Species']).mean() # max,min,std,...apply, etc.
```

R

```R
iris %>%
    group_by(Species) %>%
    summarise(Sepal.Avg.Length=mean(Sepal.Length))
```

这里面R不如python方便。不过对于分组做其他事情，R还是比较方便的。

# 汇总

python

```python
iris.groupby('Species').agg({'Sepal_Length':'max', 'Sepal_Width':'std'})
```

R

```R
iris %>%
	group_by(Species) %>%
	summarise(Sepal.Length = max(Sepal.Length), Sepal.Width=sd(Sepal.Width))
```

# 聚合

python

```python
import pandas as pd
pd.merge(...)/df.join(...)
```

R

```R
left_join, right_join, inner_join, full_join, semi_join, aniti_join, intersect, union, setdiff
```

# 管道

python

```python
import seaborn as sns
(sns.load_dataset('iris')
    .query("sepal_length > 5")
    .assign(sepal_ratio = lambda df: df.sepal_length / df.sepal_width)
    .pipe((sns.violinplot,'data'), x='species', y='sepal_ratio')
)
```

R

```R
iris %>%
    filter(Sepal.Length > 5) %>%
    mutate(Sepal.Ratio = Sepal.Length / Sepal.Width) %>%
    ggplot(aes(x=Species, y=Sepal.Ratio)) + geom_violin()
```



# 参考资料

[1] (https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf)

[2] (http://standarderror.github.io/notes/Data-munging-cheat-sheet/)

[3] (https://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html)

[4] (http://pythonhosted.org/pandas-ply/)