groupby 是Pandas中进行分组运算的函数，它能将数据集按照你需要的列进行分组。

apply 则是一个自由度非常高的函数，它可以将一个自定义函数应用到DataFrame的行或列中。

一起使用的时候，可以将 apply 函数用于Groupby对象上，这样就能对每一个分组应用一个函数。最常见的用法是对分组后的结果进行一些复杂的函数运算，比如：多个列之间的运算等。
## 示例代码
```python
import pandas as pd

df = pd.DataFrame({
    'Country': ['China', 'China', 'India', 'India', 'America', 'America', 'China'],
    'City': ['Beijing', 'Shanghai', 'Mumbai', 'Delhi', 'New York', 'Los Angles', 'Guang Zhou'],
    'Population': [2000, 2300, 1300, 1700, 1400, 1600, 3500]
})

def calculate_total_population(group):  # 每个国家的总人口和总城市数量
    return pd.Series({
        'Total Population': group['Population'].sum(),
        'city_amount': group['City'].count()
    })

def the_biggest_city(group):  # 每个国家最多人口的城市
    group = group.sort_values(by="Population", ascending=False)
    return pd.Series({
        'the_biggest_city': group['City'].tolist()[0],
        'Population': group['Population'].tolist()[0],
    })


result1 = df.groupby('Country').apply(calculate_total_population)
result2 = df.groupby('Country').apply(the_biggest_city)
print(df)
print(result1)
print(result2)
```
## 示例结果
![image](https://github.com/mjqgh/mjqgh.github.io/assets/173347114/66e3d3c0-8b9c-487d-a1e1-d15b86785476)
![image](https://github.com/mjqgh/mjqgh.github.io/assets/173347114/65aec3ec-1d7b-4965-9bca-7eb86457fe23)
![image](https://github.com/mjqgh/mjqgh.github.io/assets/173347114/ea9dbda8-7b0c-4fbb-997d-97a4c71b4b61)
