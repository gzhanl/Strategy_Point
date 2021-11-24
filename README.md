# Strategy_Point
构建策略时的技巧和注意事项

## 导入数据时操作 ##
```python
# 任何原始数据读入都进行一下排序、去重，以防万一    * 重要 *
df.sort_values(by=['交易日期'], inplace=True)
df.drop_duplicates(subset=['交易日期'], inplace=True)
df.reset_index(inplace=True, drop=True)
```
