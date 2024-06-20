## 筛选
```python
df.loc[哪些行, 哪些列]
```

## 对【列】的所有单元格处理
df["A"].apply(func)

## 对【df】的所有单元格处理
df.applymap(func)

## groupby+apply 万能聚合统计
（可以理解为，把聚合后的每个df传递给apply函数进行聚合处理，function为自定义的聚合函数）
df.groupby("列1").apply(function) # 详见【00】

## 表匹配（注意保证一对一，避免出现一对多）
pd.merge(df1, df2, left_on, right_on, how)

## 表合并
pd.concat([df1, df2])

## 删除列
df.drop(["A", "B"], axis=1)

## 重置索引
df.reset_index(drop=True)  # 去掉原索引
df.reset_index(drop=False)  # 默认，把原索引作为列保留