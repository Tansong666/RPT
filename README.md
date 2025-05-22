# RPT 根系表型计算软件

## 项目简介
RPT（Root Phenotyping Toolkit）是由华中农业大学高通量作物表型团队自主研发的根系表型计算专用软件，专注于解决作物根系表型分析的高通量、自动化需求。软件基于PaddlePaddle深度学习框架与PaddleSeg语义分割工具链，集成U-Net、U-Net 3+、U2-Net等经典与改进型分割模型，支持从根系图像采集到形态参数量化的全流程分析，可精准提取根长、根表面积、分叉数、直径分布等核心表型指标，为作物遗传育种与逆境生理研究提供高效数据支撑。

---

## 核心功能
- **根系图像智能分割**：针对复杂背景下的根系图像（如土培/水培根系、扫描/显微图像），通过U-Net系列模型实现根系与背景的高精度语义分割，支持自定义模型调优；
- **多维度表型量化**：自动计算根总长、平均直径、表面积、体积、分叉数、拓扑指数等20+项关键表型参数（示例输出见`output/root_metrics.csv`）；
- **全流程可视化**：提供图像分割结果叠加显示（`ui/segmentation_viewer.py`）、参数分布直方图/箱线图（`component/plot_widget.py`）等可视化模块；
- **多场景适配**：支持不同分辨率（512×512至4096×4096）、不同成像方式（平板扫描、显微成像、无人机航拍）的根系图像输入；
- **一键式部署**：内置PyInstaller打包配置（`RPT.spec`），支持Windows系统生成独立可执行文件（`dist/RPT.exe`），无需Python环境即可运行。

---

## 安装与运行
### 环境要求
- 操作系统：Windows 10/11（64位）
- 硬件建议：NVIDIA GPU（显存≥4GB，支持CUDA 11.2+）以加速模型推理；CPU版需Intel i5及以上处理器
- 依赖库（已集成于打包文件，源码安装需手动配置）：
  - PaddlePaddle 2.4.1（GPU版优先）
  - OpenCV 4.5.5（图像IO与预处理）
  - PaddleSeg 2.8.0（语义分割核心）
  - Pandas/Matplotlib（参数计算与可视化）

### 快速安装（推荐）
直接使用预打包可执行文件：
1. 从Release页面下载`RPT_v1.0_win64.zip`并解压；
2. 双击`dist/RPT.exe`启动软件（首次运行需等待环境初始化，约1-2分钟）。

### 源码编译（开发者）
```bash
# 克隆项目（替换为实际仓库地址）
git clone https://github.com/HZAU-Phenomics/RPT.git
cd RPT

# 安装依赖（建议使用Anaconda环境）
conda create -n rpt python=3.8
conda activate rpt
pip install -r requirements.txt

# 启动图形界面
python main.py

# 打包为可执行文件（需已安装pyinstaller）
pyinstaller RPT.spec
```

---

## 使用流程示例
### 场景：水培水稻根系表型分析
1. **数据导入**：点击界面"导入图像"，选择水培根系扫描图像（支持PNG/JPG/TIFF格式）；
2. **模型选择**：在"模型配置"中选择`U-Net 3+`（针对细根分叉场景优化），设置输入尺寸512×512；
3. **参数计算**：点击"开始分析"，软件自动完成分割→骨架提取→参数计算，结果保存至`output/20240920_roots`；
4. **结果查看**：在"结果面板"查看分割叠加图（`seg_overlay.png`）与参数报表（`metrics.xlsx`），支持导出为PDF报告。

---

## 目录结构
```
RPT/
├── .gitignore          # Git版本控制忽略规则（排除临时文件、打包产物）
├── dist/               # PyInstaller生成的可执行文件（RPT.exe）
├── icon.ico            # 软件图标（128×128像素）
├── main.py             # 主程序入口（启动图形界面）
├── paddleseg/          # 核心算法模块（基于PaddleSeg二次开发）
│   ├── models/         # 分割模型（unet.py、unet_3plus.py、u2net.py）
│   ├── transforms/     # 根系图像专用增强（旋转/裁剪/亮度调整）
│   └── utils/          # 表型计算工具（骨架提取、参数统计）
├── threads/            # 多线程任务模块（异步执行训练/分析任务）
├── ui/                 # 图形界面文件（Qt Designer生成的.ui转.py文件）
├── RPT.spec            # PyInstaller打包配置（指定依赖与图标）
└── requirements.txt    # 源码安装依赖列表
```

---

## 团队与支持
- 开发团队：华中农业大学高通量作物表型团队（作物遗传改良国家重点实验室）
- 技术支持：请通过邮箱`phenomics@hzau.edu.cn`联系（请注明"RPT软件问题"）
- 版本更新：关注GitHub仓库`HZAU-Phenomics/RPT`获取最新功能与Bug修复。

---

## 许可证
本项目采用MIT开源许可证，允许学术研究与商业应用。具体条款见`LICENSE`文件（需随软件分发）。

        