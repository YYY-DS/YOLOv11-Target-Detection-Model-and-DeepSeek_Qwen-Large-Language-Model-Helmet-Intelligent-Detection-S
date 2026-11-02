# YOLOv11 目标检测模型与 DeepSeek/Qwen 大语言模型安全帽智能检测系统  |  安全帽检测 YOLOv11 DeepSeek 智慧工地 目标检测 深度学习 计算机视觉9234期
## 系统概述

本系统基于 **YOLOv11 目标检测模型**与 **DeepSeek/Qwen 大语言模型**，融合 **Pytorch、SpringBoot、Flask、Vue3、MySQL** 等技术，构建了一套完整的**安全帽佩戴检测与智能分析平台**。系统支持图片、视频、摄像头实时检测，并可自动生成 AI 分析建议与 PDF 检测报告，适配工地安全监管、工厂车间及高危作业场景。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8d8814c5ab55414183b8318959682ea6.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7670f89a8795424f8ef5263f937d71b1.png)

## 系统核心信息

| 项目 | 详细说明 |
| :--- | :--- |
| **检测类别** | 共 **2** 类：`佩戴安全帽`、`未佩戴安全帽` |
| **核心技术** | YOLOv11 + DeepSeek/Qwen + Pytorch + SpringBoot + Flask + Vue3 |
| **检测模式** | 图片检测、视频检测、摄像头实时检测 |
| **系统架构** | 前后端分离（Vue3 + SpringBoot + Flask + MySQL） |
| **应用价值** | 1. **安全生产**：支撑工地、工厂等场景的安全帽佩戴实时监测与违规告警。<br>2. **智能监管**：辅助安监部门远程巡检，提升检查覆盖面与效率。<br>3. **AI 分析**：结合大语言模型生成违规行为分析与整改建议。<br>4. **报告导出**：自动生成 PDF 检测报告，便于存档与汇报。 |

## 技术栈

| 层次 | 技术选型 |
| :--- | :--- |
| **深度学习** | YOLOv11、Pytorch |
| **算法服务** | Flask |
| **业务后端** | SpringBoot、MyBatis-Plus |
| **前端** | Vue3、Echarts、TypeScript、Element-Plus、Axios |
| **数据库** | MySQL |
| **AI 分析** | DeepSeek、Qwen |


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3f8484a7651b4042a4da18f45829ef4a.png)

## 系统功能

**1. 多模态检测**
- **图片检测**：单张图片上传识别
- **视频检测**：上传视频，逐帧分析并输出标注视频
- **摄像头实时检测**：连接摄像头，实时输出检测结果

**2. 检测结果可视化**
- 检测结果可视化展示
- 目标位置、类别、置信度等信息实时显示

**3. AI 智能建议**
- 结合 DeepSeek、Qwen 等大模型，对检测结果给出安全合规建议与整改措施

**4. 报告导出**
- 支持将检测报告导出为 PDF 文件

## 交付内容

1. **整理好的 YOLO 格式数据集**
2. **详细的系统部署教程**
3. **实时视频、摄像头、图片检测**
4. **代码简洁，有注释**
5. **提供训练好的 YOLO 权重**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/da0c04dfcc484e60aff0f0afce956104.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2022176400c645889458abc61b1f17ce.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3a86880c14a94baa9529db25f006a723.png)

## 快速开始（代码示例）

**1. 数据集配置文件 (`data.yaml`)**
```yaml
train: ./dataset/images/train
val: ./dataset/images/val
nc: 2
names: ['佩戴安全帽', '未佩戴安全帽']
```

**2. YOLOv11 训练代码**
```python
from ultralytics import YOLO

model = YOLO('yolo11n.pt')
model.train(
    data='data.yaml',
    epochs=100,
    imgsz=640,
    batch=16,
    device=0
)
```

**3. Flask 推理与 AI 分析服务**
```python
from flask import Flask, request, jsonify
from ultralytics import YOLO
import cv2
import numpy as np

app = Flask(__name__)
model = YOLO('runs/detect/train/weights/best.pt')

@app.route('/detect', methods=['POST'])
def detect():
    file = request.files['image']
    img = cv2.imdecode(np.frombuffer(file.read(), np.uint8), cv2.IMREAD_COLOR)
    results = model(img)
    detections = []
    for box in results[0].boxes:
        detections.append({
            'class': model.names[int(box.cls[0])],
            'confidence': float(box.conf[0]),
            'bbox': box.xyxy[0].tolist()
        })
    # 调用 DeepSeek/Qwen 生成安全建议
    ai_suggestion = analyze_with_llm(detections)
    return jsonify({'detections': detections, 'suggestion': ai_suggestion})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

**4. Vue3 前端检测页面**
```vue
<template>
  <div class="detector-container">
    <el-upload action="/api/detection/image" :on-success="handleSuccess">
      <el-button type="primary">上传图片</el-button>
    </el-upload>
    <div v-if="resultImage" class="result-area">
      <img :src="resultImage" alt="Detection Result" />
      <el-table :data="detections" border>
        <el-table-column prop="class" label="佩戴状态" />
        <el-table-column prop="confidence" label="置信度" />
      </el-table>
      <el-button @click="exportPDF">导出 PDF 报告</el-button>
    </div>
  </div>
</template>
```

## 标签
`安全帽检测` `YOLOv11` `DeepSeek` `智慧工地` `目标检测` `深度学习` `计算机视觉`
