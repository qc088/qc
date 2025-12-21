```python
import pandas as pd                    
import pingouin as pg                                    

# 定义各因子对应的题项
factor_items = {                    
    "服务专业性": ["服务专业性1", "服务专业性2", "服务专业性3"],
    "视觉魅力性": ["视觉魅力性1", "视觉魅力性2", "视觉魅力性3"],
    "互动有效性": ["互动有效性1", "互动有效性2", "互动有效性3"],
    "拟人化": ["拟人化1", "拟人化2", "拟人化3"],
    "信任感": ["信任感1", "信任感2", "信任感3"],  
    "购买行为": ["购买行为1", "购买行为2", "购买行为3"]    
}

# 读取 CSV 数据（请替换为你的实际文件路径，确保编码为 UTF-8 或按需调整 encoding 参数）

file_path = r"D:\桌面\19977\Desktop\data.csv"            
try:
    data = pd.read_csv(file_path, encoding='utf-8')  # 假设编码为 UTF-8，有问题可换 'gbk' 等  
except UnicodeDecodeError:  
    data = pd.read_csv(file_path, encoding='gbk')    # 备选编码，根据实际情况调整

< # 存储各因子的 Cronbach’s Alpha 结果

alpha_results = {}        

for factor, items in factor_items.items():    
    # 检查数据中是否存在对应的题项列，不存在则跳过并提示
    missing_items = [item for item in items if item not in data.columns]      
    if missing_items:
        print(f"因子 {factor} 跳过！数据中缺少题项：{missing_items}")
        continue  
    
    # 计算 Cronbach’s Alpha，并强制转换结果为 DataFrame（兼容不同版本返回类型）
    alpha_result = pg.cronbach_alpha(data=data[items])      
    if not isinstance(alpha_result, pd.DataFrame):  
        alpha_result = pd.DataFrame(alpha_result)  # 转换为 DataFrame 方便处理        
    
    # 提取 Alpha 值（取第一行第一列，默认结果只有一个数值）
    alpha_value = alpha_result.iloc[0, 0] if not alpha_result.empty else None    
    alpha_results[factor] = alpha_value    

# 打印各因子的 Alpha 结果

print("各因子 Cronbach’s Alpha 计算结果：")    
for factor, alpha in alpha_results.items():    
    print(f"因子 {factor}：{alpha:.4f}")

