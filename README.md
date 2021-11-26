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

## 计算涨跌停价格 (结果是三位小数，后面要四舍五入处理) ##
```python
df['涨停价'] = df['前收盘价'] * 1.1
df['跌停价'] = df['前收盘价'] * 0.9
```

## 四舍五入注意事项 ##
```python
# print(round(3.5), round(4.5))  # 银行家舍入法：四舍六进，五，奇进偶不进(小数点前一位)
df['涨停价'] = df['涨停价'].apply(lambda x: float(Decimal(x*100).quantize(Decimal('1'), rounding=ROUND_HALF_UP) / 100))
df['跌停价'] = df['跌停价'].apply(lambda x: float(Decimal(x*100).quantize(Decimal('1'), rounding=ROUND_HALF_UP) / 100))
```
