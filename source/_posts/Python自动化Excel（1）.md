---
title: Python自动化Excel（1）
author: 小人市井
top: true
date: 2026-01-14 18:13:19
summary:
img:
coverImg:
tags: Python
categories:
- [技术兴趣,自动化]
password:
---
# 安装第三方库
```bash
pip install pandas openpyxl xlrd xlwt matplotlib -i https://pypi.tuna.tsinghua.edu.cn/simple
```

# 验证安装
```bash
# test.py 文件里写一下两行
import pandas as pd
print("Python+Excel，准备起飞！")
# 运行：python test.py
```

# 50 个代码块
## 文件操作（10个）
1. 读取 Excel 文件
   ```python
   import pandas as pd
   df = pd.read_excel('文件.xlsx', sheet_name='Sheet1')
   ```
2. 读取 CSV 文件
   ```python
   df = pd.read_csv('文件.csv', encoding='utf-8')
   ```
3. 保存为 Excel
   ```python
   df.to_excel('新文件.xlsx', index=False) # 不要索引列
   ```
4. 批量读取文件夹所有 Excel
   ```python
   import os
   all_Files = [f for f in os.listdir('文件夹') if f.endswith('.xlsx')]
   ```
5. 查看数据信息
   ```python
   print(df.shape) # 行列数
   print(df.head()) # 前 5 行
   print(df.describe()) # 统计摘要
   ```
6. 处理大文件（分块读取）
   ```python
   chunk_size = 10000
   for chunk in pd.read_excel('超大文件.xlsx', chunksize=chunk_size):
    process(chunk) # 分批处理
   ```
7. 读取多个工作表
   ```python
   xls = pd.ExcelFile('文件.xlsx')
   sheet_nemes = xls.sheet_names # 获取所有表名
   ```
8. 保存多个工作表
   ```python
   with pd.ExcelWrite('输出.xlsx') as writer:
    df1.to_excel(writer, sheet_name='表1')
    df2.to_excel(writer, sheet_name='表2')
   ```
9.  自动识别文件编码
   ```python
   import chardet
   with open('文件.csv', 'rb') as f:
    result = chardet.detect(f.read())
    encoding = result['encoding']
   ```
10. 检查文件是否存在
   ```python
   import os
   if os.path.exist('文件.xlsx'):
    print("文件存在")
   ```
## 数据处理（15个）
11. 删除空白行
   ```python
   df_clean = df.dropna() # 删除任何空值的行
   ```
12. 填充空值
   ```python
   
   ```
13. 删除重复行
14. 重命名列
15. 选择特定列
16. **条件筛选**
17. 排序数据
18. 分组统计（Excel 透视表升级版）
19. 数据透视表
20. 合并多个表（VLOOKUP 加强版）
21. 连接多个表
22. 应用函数到整列
23. 拆分列
24. 数据类型转换
25. 异常值处理

## 格式美化（10个）
26. 设置列宽（需 openpyxl）
27. 设置字体和颜色
28. 设置单元格背景色
29. 自动调整所有列宽
30. 设置数字格式
31. 添加边框
32. 设置行高
33. 合并单元格
34. 条件格式（自动标红小于 0 的值）
35. 冻结窗格

## 高级自动化（15个）
36. 批量合并多个 Excel 文件
37. 按条件拆分 Excel 文件
38. 自动发送邮件附件
39. 定时自动执行
40. 从数据库读取数据到 Excel
41. 网页数据抓取到 Excel
42. PDF 表格提取到 Excel
43. 自动生成图表并插入 Excel
44. 批量明明工作表
45. 对比两个 Excel 差异
46. 数据加密保存
47. 自动备份文件
48. 监控文件夹新增文件
49. 自动错误重试
50. 生成执行日志