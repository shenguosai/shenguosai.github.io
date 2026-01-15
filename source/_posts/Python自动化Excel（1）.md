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
   df_filled = df.fillna(0) # 用 0 填充
   # 或用前一个值填充
   df_filled = df.fillna(method='ffill')
   ```
13. 删除重复行
   ```python
   df_unique = df.drop_duplicates() # 完全重复
   # 基于某些列去重
   df_unique = df.drop_duplicates(subset=['姓名', '日期'])
   ```
14. 重命名列
   ```python
   df.rename(columns={'旧名':'新名', 'old':'new'},inplace=True)
   ```
15. 选择特定列
   ```python
   selected = df[['列1','列2','列3']]
   ```
16. **条件筛选**
   ```python
   # 单条件
   filtered = df[df['销售额'] > 1000]
   # 多条件
   filtered = df[(df['部门']=='销售部') & (df['月份']=='12月')]
   # 包含某个文本
   filtered = df[df['产品名'].str.contains('Pro')]
   ```
17. 排序数据
   ```python
   sorted_df = df.sort_values(by='销售额',ascending=False) # 降序
   ```
18. 分组统计（Excel 透视表升级版）
   ```python
   grouped = df.groupby('部门')['销售额'].agg(['sum','mean','count'])
   ```
19. 数据透视表
   ```python
   pivot = pd.pivot_table(df,value='销售额',index='部门',coulumns='月份',aggfunc='sum')
   ```
20. 合并多个表（VLOOKUP 加强版）
   ```python
   # 类似 Excel 的 VLOOKUP
   merged = pd.merge(表1,表2,on='关键列',how='left')
   ```
21. 连接多个表
   ```python
   # 上下拼接
   concatenated = pd.concat([df1,df2,df3]ignore_index=True)
   ```
22. 应用函数到整列
   ```python
   # 比如所有金额除以 10000，转为万元
   df['销售额_万元'] = df['销售额'.apply(lambda x: x/10000)]
   ```
23. 拆分列
   ```python
   # 将“省-市”拆分成两列
   df[['省','市']] = df['地址'].str.split('-',expand=True)
   ```
24. 数据类型转换
   ```python
   df['日期列'] = pd.to_datetime(df['日期列']) # 转日期
   df['金额列'] = df['金额列'].astype(float) # 转浮点数
   ```
25. 异常值处理
   ```python
   # 识别并处理异常值（3 个标准差以外）
   mean = df['数据列'].mean()
   std = df['数据列'].std()
   df['是否异常'] = df['数据列'].apply(lambda x: abs(x-mean)>3*std)
   ```
## 格式美化（10个）
26. 设置列宽（需 openpyxl）
   ```python
   from openpyxl import load_workbook

   wb = load_workbook('文件.xlsx')
   ws = wb.active
   ws.column_dimensions['A'].width = 20 # 设置 A 列宽度
   wb.save('新文件.xlsx')
   ```
27. 设置字体和颜色
   ```python
   from openpyxl.styles import Font

   font = Font(name='微软雅黑',size=11,bold=True,color='FF0000')
   ws['A1'].font = font
   ```
28. 设置单元格背景色
   ```python
   from openpyxl.styles import PatternFill

   fill = PatternFill(start_color='FFFF00', end_color='FFFF00', fill_type='solid')
   ws['A1'].fill = fill
   ```
29. 自动调整所有列宽
   ```python
   for column in ws.columns:
      max_length = 0
      column_letter = column[0].column_letter
      for cell in column:
         try:
            if len(str(cell.value)) > max_length:
               max_length = len(str(cell.value))
         except:
            pass
      adjusted_width = (max_length + 2)
      ws.column_dimensions[column_letter].width = adjusted_width
   ```
30. 设置数字格式
   ```python
   from openpyxl.styles import numbers

   ws['B2'].number_format = numbers.FORMAT_NUMBER_COMMA_SEPARATED1 # 千分位
   ws['C2'].number_format = numbers.FORMAT__PERCENTAGE_00 # 百分位
   ```
31. 添加边框
   ```python
   from openpyxl.styles import Border,Side

   thin_border = Border(left=Side(style='thin'),
                        right=Side(style='thin'),
                        top=Side(style='thin'),
                        bottom=Side(style='thin'))
   ws['A1'].border = thin_border
   ```
32. 设置行高
   ```python
   ws.row_dimensions[1].height = 30 # 设置第 1 行高度
   ```
33. 合并单元格
   ```python
   ws.merge_cells('A1:D1') # 合并 A1 到 D1
   ```
34. 条件格式（自动标红小于 0 的值）
   ```python
   from openpyxl.formatting.rule import CellsRule

   red_fill = PatternFill(start_color='FFFF00',end_color='FFFF00',fill_type='solid')
   ws.conditional_formatting.add('B2:B100',CellsRule(operator='lessThan',formula=['0'],fill=red_fill))
   ```
35. 冻结窗格
   ```python
   ws.freeze_panes = 'B2' # 冻结第 1 行和第 1 列
   ```

## 高级自动化（15个）
36. 批量合并多个 Excel 文件
   ```python
   import pandas as pd
   import glob

   all_files = glob.glob('文件夹/*.xlsx')
   df_list = []
   for file in all_files:
      df = pd.read_excel(file)
      df_list.append(df)

   combined_df = pd.concat(df_list, ignore_index=True)
   cimbined_df.to_excel('合并结果.xlsx', index=False)
   ```
37. 按条件拆分 Excel 文件
   ```python
   # 按部门拆分成多个文件
   for dept in df['部门'].unique():
      dept_data = df[df['部门'] == dept]
      dept_data.to_excel(f'{dept}_数据.xlsx',index=False)
   ```
38. 自动发送邮件附件
   ```python
   import smtplib
   from email.mime.multipart import MIMEMultipart
   from email.mime.base import MIMEBase
   from email import encoders

   msg = MIMEMultipart()
   msg['Subject'] = '日报表'
   msg['From'] = '你的邮箱'
   msg['To'] = '领导邮箱'

   # 添加附件
   part = MIMEBase('application', "octet-stream")
   part.set_payload(open("日报.xlse","rb").read())
   encoders.encode_base64(part)
   part.add_header('Content-Disposition','attachment',filename='日报.xlsx')
   msg.attach(part)

   # 发送
   server = smtplib.SMTP('smtp.xxx.com',587)
   server.send_message(msg)
   server.quit()
   ```
39. 定时自动执行
   ```python
   import schedule
   import time

   def daily_task():
      # 你的数据处理代码
      print("任务完成！")
   
   schedule.every().day.at("18:00").do(daily_task) # 每天 18 点执行

   while True:
      schedule.run_pending()
      time.sleep(60) # 每分钟检查一次
   ```
40. 从数据库读取数据到 Excel
   ```python
   import pandas as pd
   import sqlite3 # 或其他数据库驱动

   conn = sqlite3.connect('database.db')
   query = "SELECT * FROM sales WHERE date >= '2024-01-01'"
   df = pd.read_sql_query(query, conn)
   df.to_excel('数据库导出.xlsx', index=False)
   ```
41. 网页数据抓取到 Excel
   ```python
   import requests
   from bs4 import BeautifulSoup
   import pandas as pd

   url = 'https://example.com/data'
   response = requests.get(url)
   soup = BeautifulSoup(response.text, 'html.parser')

   # 解析数据
   data = []
   for item in soup.find_all('tr'):
      row = [td.text for td in item.find_all('td')]
      if row:
         data.append(row)

   pd.DataFrame(data).to_excel('网页数据.xlsx',index=False)
   ```
42. PDF 表格提取到 Excel
   ```python
   import camelot

   # 提取 PDF 中的表格
   tables = camelot.read_pdf('文件.pdf', pages='1')
   tables[0].df.to_excel('PDF表格.xlsx',index=False) # 第一个表格
   ```
43. 自动生成图表并插入 Excel
   ```python
   import matplotlib.pyplot as plt
   from openpyxl.drawing.image import Image

   # 生成图表
   plt.figure(figsize=(10, 6))
   df.groupby('月份')['销售额'].sum().plot(kind='bar')
   plt.savefig('chart.png')

   # 插入到 Excel
   img = Image('chart.png')
   ws.add_image(img, 'A10') # 从 A10 单元格开始插入
   ```
44. 批量明明工作表
   ```python
   from openpyxl import load_workbook

   wb = load_workbook('文件.xlsx')
   for i, sheet in enumerate(wb.sheetnames, 1):
      ws = wb[sheet]
      ws.title = f'数据表_{i}'
   wb.save('重命名.xlsx')
   ```
45. 对比两个 Excel 差异
   ```python
   df1 = pd.read_excel('文件1.xlsx')
   df2 = pd.read_excel('文件2.xlsx')

   # 找出不同的行
   diff = pd.concat([df1,df2]).drop_duplicates(keep=False)
   diff.to_excel('差异结果.xlsx', index=False)
   ```
46. 数据加密保存
   ```python
   from openpyxl import Workbook
   from openpyxl.workbook.protection import WorkbookProtection

   wb = Workbook()
   ws = wb.active
   # 。。。填充数据。。。

   # 设置密码保护
   wb.security = WorkbookProtection(workbookPassword='密码123', lockStructure=True)
   wb.save('加密文件.xlsx')
   ```
47. 自动备份文件
   ```python
   import shutil
   from datetime import datetime

   timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
   shutil.copy2('重要文件.xlsx',f'备份/重要文件_{timestamp}.xlsx')
   ```
48. 监控文件夹新增文件
   ```python
   import time
   import os

   folder = '监控文件夹'
   processed = set(os.listdir(folder))

   while True:
      current = set(os.listdir(folder))
      new_files = current - processed
      if nwe_files:
         for file in new_files:
            if file.endswitch('.xlsx'):
               print(f"新文件：{file}")
               # 处理新文件
      processed = current
      time.sleep(10) # 每 10 秒检查一次
   ```
49. 自动错误重试
   ```python
   import time

   max_retries = 3
   for attempt in range(max_retries):
      try:
         df = pd.read_excel('可能损坏的文件.xlsx')
         break # 成功则跳出循环
      except Exception as e:
         print(f"第{attempt+1}次尝试失败：{e}")
         if attempt == max_retries - 1:
            print("全部尝试失败")
         time.sleep(2) # 等待 2 秒后重试
   ```
50. 生成执行日志
   ```python
   import logging

   logging.basicConfig(filename='自动化日志.log', level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

   try:
      # 你的数据处理代码
      logging.info("开始处理厨具")
      # 。。。处理过程。。。
      logging.info("数据处理完成")
   except Exception as e:
      logging.error(f"处理失败：{e}")
   ```