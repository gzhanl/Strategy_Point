# Strategy_Point
构建策略时的技巧和注意事项

## 导入数据时操作 ##
```python
# 任何原始数据读入都进行一下排序、去重，以防万一    * 重要 *
df.sort_values(by=['交易日期'], inplace=True)
df.drop_duplicates(subset=['交易日期'], inplace=True)
df.reset_index(inplace=True, drop=True)    #  因为前面的drop所以要重置索引
```

## 计算后复权价 ##
```python
df['涨跌幅'] = df['收盘价'] / df['前收盘价'] - 1
df['复权因子'] = (1 + df['涨跌幅']).cumprod()
df['收盘价_复权'] = df['复权因子'] * (df.iloc[0]['收盘价'] / df.iloc[0]['复权因子'])
df['开盘价_复权'] = df['开盘价'] / df['收盘价'] * df['收盘价_复权']
df['最高价_复权'] = df['最高价'] / df['收盘价'] * df['收盘价_复权']
df['最低价_复权'] = df['最低价'] / df['收盘价'] * df['收盘价_复权']
df.drop(['复权因子'], axis=1, inplace=True)
```
