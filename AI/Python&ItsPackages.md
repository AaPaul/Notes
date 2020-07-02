# Spyder
#### Code complete
check settings in preference
download enum34
delete enum34
cheak libraries:
```
pip install --upgrade enum34
pip delete
pip install jedi Parso
```
---

Writing codes in a block: ```Ctrl+Enter```


---
---
# PyCharm
### DataFram

#### Find columns/rows
```
df.iloc() 根据索引来寻找行列（先行后列）
df.loc() 行标签和列标签
df.ix() 标签和索引都能使用
```
A specific utilization of finding columns
```
subset_attributes = ['residual sugar', 'total sulfur dioxide', 'sulphates', 
                     'alcohol', 'volatile acidity', 'quality']
rs = round(red_wine[subset_attributes].describe(),2)
```

#### Run with Scientific mode
- Preference->Tools->Python Scientific
- View -> Scientific mode
- Run configurations -> run with python console

#### Fill nan with specific value
```
df.fillna(0) # fill nan with 0
```

#### df.sort_index(axis=0, ascending=False) 
Sorting by an axis (index or column label)

#### Faster way to get a scalar
```
df.loc[dates[0], 'A']
df.at[dates[0], 'A']
# df.at[] is faster.
df.iloc[1, 1]
df.iat[1, 1]
# df.iat[] is faster

```
---
# PLT
### plt
Set label of x and y
```
# One plot
ax = fig.subplots()
ax.set_xlabel("xxx")
ax.set_ylabel("yyy")

# More than 1 plot
ax1, ax2 = fig.subplots(1, 2)   # row and column
ax3 = fig.add_subplots()
```

#### plt.subplot_adjust()
Tune the subplots layout

#### ax.set_title()
Set the title of subplots with a good layout
If I use the ```plt.suptitle()```, the layout is bad where the title has overlapping with the plot.

#### plt.tight_layout(react=[0, 0, 1, 1])
Another function to tune the position of titles of subplots

#### density plot
Using seaborn \
```sns.kdeplot(df['Y house price of unit area'], ax=ax, shade=True)```

#### plt.imshow()
Draw the plots in the figure object which will be showed by plt.show()
**NOTE:**The 3-D image may have different distribution sequence of its RGB channels with this function. ```[H, W, 3] --> imshow(); [N, C, H, W] --> image```
So, we need to transform the 3-D image to gray (1-D) image or we can change the sequence of the image parameters.

#### fig.suptitle()
Set the global title of the figure

#### plt.savefig('postion', bbox_inches='tight', dpi=200)
Set the dpi of saved figure and make sure all contents of the figure are included

#### plt.axis('off') 
Remove the axis of the figure
```
# All of these codes are the functions for controlling the axis
ax.axis('off')
ax.set_xticks([])
ax.set_yticks([])
```
#### plt.subplots()
Set subplots in a specific figure. The example shows below.
```
fig, axs = plt.subplots(3, 3, figsize= (20, 20))
title = fig.suptitle("Continuous distribution",y=0.9, va='center',fontsize=27)

# the flag judge whether the attribute is visitied
count=0
for i in range(3):
    for j in range(3):
        if (count <= 6):
            axs[i, j].set_xlabel(columns[count], fontsize=18)
            axs[i, j].set_ylabel("Frequency", fontsize=18)
            sns.kdeplot(dataset[columns[count]], ax=axs[i, j], shade=True)
            count += 1
        else:
            # Hide axis and boarders of the last 2 subplots.
            axs[i, j].spines['top'].set_visible(False)
            axs[i, j].spines['right'].set_visible(False)
            axs[i, j].spines['bottom'].set_visible(False)
            axs[i, j].spines['left'].set_visible(False)
            axs[i, j].axis('off')
plt.savefig("./Figures/Continours_distribution.png")
plt.show()
```
**Note:** Using base64 to encode the figure which we want to insert in the markdown file is not good enough as it takes up a lot of space. We can upload the image to the github and copy the address into the markdown file to show the image.
![avatar](https://github.com/AaPaul/CSI-5387_Data_Mining/blob/master/Project/Preprocessing/Figures/Continours_distribution.png?raw=true)

---
# Functions

#### get the current path
```
os.getcwd()
os.path.join(path, "path1") connect the path
```

#### The uitilization of pd.apply()
```
df['xxx'].apply(lambda value: 'low' if value <= 5 else 'medium' 
                                        if value <= 7 else 'high')
# apply a function along an axe
```
**take a peek 看一眼**


#### round(a, b)
a保留b位小数


### Pandas
#### pd.count
```pd.count()``` ignores the Nan values when count the number of values.

#### pd.to_numeric()
Transform the data to numeric type

#### DataFrame sort
```
df.sort_values(by="xxx", ascending=False, inplace=True)
df.sort_index()
```

#### pd.Series.plot(style='ro')
It is the same as the plt.plot() as its implement is based on plt. Style parameter is to determine the way to show the plot like using points or line or dot line.
https://blog.csdn.net/helunqu2017/article/details/78629136

## List
#### Delete and append value from a list
```
list.append()
list.remove()
```

#### Difference among Del(), remove(), pop()
Del is a statement instead of the function belonging to the list. 
```
a = ['a','b','c']
Del(a)
a = ['b', 'c'] #a[0]='b'

a.remove('a') # a[0]=[a]

t1 = a.pop(-1) # t1='c'
```

## Numpy 

#### np.linalg.norm(ord=l2)
Calculate the l1-norm, l2-norm or infinite norm (np.inf) $\infty$.

#### np.random.randn(x1, x2, ...)
Create an array of specified shape and fill it with random values as per standard normal distribution.


---
# Weka
#### Error of Reading a csv file.
Error: wrong number of values. read 2, expected 1. read token[EOL], line xxx

---
# Jupyter Notebook
#### Setting of showing full columns and rows
```
pd.set_option('max_columns', 1000)
```

---
### mlxtend is a library including apriori function.

# Python

#### `def fun(*args, **kwargs)`
args are incoming parameters or tuples. kwargs are key-value pairs.
```
def fun(*args, **kwargs):
   print("args=", args)
   print("kwargs=", kwargs)
   print("\n")

fun(1, 2)
# >>>args= (1, 3)
# >>>kwargs= {}

fun(None, d=4)
# >>>args= (None)
# >>>kwargs= {'d':4}
```

#### Module & Script
Then meaning of paragraph shows below is that if the code file including relative path, it can only be imported as the *Script*. If not, there will be an error shows below because of the change of main path.

包含相对路径import 的python脚本不能直接运行，只能作为module被引用。原因正如手册中描述的，所谓相对路径其实就是相对于当前module的路径，但如果直接执行脚本，这个module的name就是“__main__”, 而不是module原来的name， 这样相对路径也就不是原来的相对路径了，导入就会失败，出现错误“ValueError: Attempted relative import in non-package”

Note that both explicit and implicit relative imports are based on the name of the current module. Since the name of the main module is always"__main__", modules intended for use as the main module of a Python application should always use absolute imports.
Reference:
https://blog.csdn.net/chinaren0001/article/details/7338041

#### python传值

函数传递是传递的变量的引用（开辟一个内存空间用来存储该变量的地址），但是传递不可变变量比如元祖，数字等，就不能直接改变变量的值。 总的来说，python的传值不能简单一概而论，要根据传的对象来看。
Reference: https://www.cnblogs.com/loleina/p/5276918.html

#### GitHub分支
Clone的时候一般只能Clone主分支（master），不过有脚本可以来clone所有分支。
Reference： https://blog.csdn.net/wu1169668869/article/details/83345633

#### Hash
哈希是把一个定义域较宽的集合映射到一个值域较小的集合
$$H(S)=\sum\limits_{i=0}^{\vert S \vert -1}{\alpha^{\vert S \vert - (i+1)} \times char(s_i)}$$

$$H(i+1) = (H(i) - char(start)\times\alpha^{L-1}) \times \alpha + char(i+L)$$

#### Python dictionary
**dict.items(), dict.keys(), dict.values()**
```[python]
for key, value in dict.items():
    print(key, value)
```