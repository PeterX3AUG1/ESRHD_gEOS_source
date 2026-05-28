# 相对论流体力学一般Synge型状态方程高阶熵稳定格式 (JSC 2024)
[![Paper](https://img.shields.io/badge/Paper-JSC%202024-blue.svg)](https://doi.org/10.1007/s10915-023-02440-x)
[![DOI](https://img.shields.io/badge/DOI-10.1007/s10915--023--02440--x-green.svg)](https://doi.org/10.1007/s10915-023-02440-x)

## 项目概述
本仓库是论文 **"High-Order Accurate Entropy Stable Schemes for Relativistic Hydrodynamics with General Synge-Type Equation of State"** (Journal of Scientific Computing, 2024, 98:43) 的官方完整数值实验代码实现。

针对现有相对论流体力学熵稳定格式仅适用于理想状态方程（与相对论动理学不相容）的核心问题，本工作建立了适用于一般Synge型状态方程的严格凸熵对，构造了统一形式的显式两点熵守恒通量，并发展了任意高阶精度的熵稳定格式。通过13个标准数值算例验证了算法的精度、鲁棒性和熵稳定性。

## 核心学术贡献
- ✅ 对于一般Synge型状态方程的相对论流体力学方程建立了严格凸的熵对，并完成了严格的数学证明
- ✅ 构造了统一形式的显式两点熵守恒通量，支持一般Synge型状态方程
- ✅ 发展了2-6阶精度的熵守恒(EC)格式和3-5阶精度的熵稳定(ES)格式
- ✅ 推导了基于缩放特征向量的一般耗散矩阵，保证格式能精确分辨静止接触间断
- ✅ 所有数值结果100%可复现，包含完整的源代码和预计算数据

## 仓库结构
所有算例均与论文章节一一对应，每个算例独立打包，下载即可运行：

| 序号 | 压缩包名称 | 对应论文章节 | 核心验证内容 | 关键技术点 |
|------|------------|--------------|--------------|------------|
| 1 | Example01.zip | 5.1 精度测试 | 空间收敛阶验证 | 6阶EC、5阶ES、四精度浮点数计算 |
| 2 | Example02.zip | 5.2 等熵问题 | 离散熵演化验证 | 熵守恒/熵稳定性质、SSP-RK3/RRK3时间离散 |
| 3 | Example03.zip | 5.3 密度扰动问题 | 小尺度扰动捕捉能力 | 激波与正弦波相互作用、RC-EOS |
| 4 | Example04.zip | 5.4 爆炸波相互作用 | 强激波鲁棒性 | 双爆炸波、TM-EOS、4000网格大规模计算 |
| 5 | Example05.zip | 5.5 Riemann问题I | 基本波系捕捉 | 左稀疏波+接触间断+右激波、RC-EOS |
| 6 | Example06.zip | 5.6 Riemann问题II | 极端强激波鲁棒性 | 1000倍压强比、TM-EOS |
| 7 | Example07.zip | 5.7 Riemann问题III | 非物理解振荡抑制 | 双激波问题、与非熵稳定格式对比 |
| 8 | Example08.zip | 5.8 Riemann问题IV | 相向流动问题 | IP-EOS、稀疏波相互作用 |
| 9 | Example09.zip | 5.9 2D精度测试 | 二维收敛阶验证 | 多物态方程支持、二维扩展能力 |
| 10 | Example10.zip | 5.10 2D Riemann问题I | 复杂接触间断捕捉 | 四象限初始条件、螺旋结构、IP-EOS |
| 11 | Example11.zip | 5.11 2D Riemann问题II | 稀疏波相互作用 | 四稀疏波、激波形成、TM-EOS |
| 12 | Example12.zip | 5.12 2D Riemann问题III | 蘑菇云结构模拟 | 双接触间断+双激波、RC-EOS |
| 13 | Example13.zip | 5.13 激波-气泡相互作用 | 大尺度工程问题模拟 | 650×180网格、反射边界、RC-EOS |

## 快速开始
### 依赖环境
所有算例使用标准C++11编写，无任何第三方库依赖：
- C++编译器：GCC 7.0+ / Clang 10.0+ / Intel ICC 19.0+
- 构建工具：CMake 3.10+
- 可视化：MATLAB R2020a+（可选，用于复现论文图表）

### 编译运行步骤（所有算例通用）
1. 下载任意算例压缩包，例如 `Example10.zip`
2. 解压并进入目录：
   ```bash
   unzip Example10.zip
   cd Example10
3. 选择对应的算例，进入下一级目录，以 Example 10 为例：
   ```bash
   cd RHD2D_RP_1_EOS_2_eigmat
4. 编译代码：
   ```bash
   cmake .
   make
5. 运行代码：
   ```bash
   ./main
   ```
## 输出说明
- 程序运行完成后，会在当前目录下生成数值结果文件：`[算例名称].dat`
- 输出格式：16 位科学计数法，每行依次为`密度 速度 压强`
- 示例输出片段：
  ```plaintext
  2.9999999999998930e+00 -4.9999999999999833e-01 5.0000000000000344e-01 4.9999999999998419e+00
  2.9999999999998903e+00 -4.9999999999999922e-01 5.0000000000000355e-01 4.9999999999998277e+00
  2.9999999999998885e+00 -4.9999999999999939e-01 5.0000000000000344e-01 4.9999999999998224e+00
  ```
## 代码结构
```plaintext
ExampleXX/
├── src/                         # 核心源代码目录
│   ├── Euler1DInit.cpp          # 程序入口，定义网格大小和算法阶数，包含边界条件处理、初始化、赋初值和输出
│   ├── Euler1D.h/cpp            # 主计算类，包含时间演化、熵守恒/熵稳定通量实现
│   ├── defineGlobalVariable.h   # 宏定义、类成员变量和成员函数的声明
│   ├── LimiterRestrction.h/cpp  # WENO/ENO重构实现
│   ├── setFlux.h/cpp            # 计算通量函数
│   ├── ConVPriVEntrV.h/cpp      # 守恒变量、原始变量、熵变量之间的相互转化
│   └──EulerEigValVec.h/cpp      # 计算最大特征速度和特征向量矩阵
├── CMakeLists.txt               # CMake构建配置文件
├── plot_results.m               # MATLAB可视化脚本
├── data/                        # 预计算的论文结果数据
│   ├── numsol.dat               # 数值解
│   ├── LF100000.dat             # 参考解/精确解
│   └── tolEntr.dat              # 离散总熵随时间变化
├── figure/                      # 论文中对应的结果图
│   ├── result.eps               # 矢量图
│   └── result.jpg               # 预览图
```
## 技术栈
- **编程语言**：C++11（核心算法实现）、MATLAB（数据可视化）
- **构建工具**：CMake
- **数值方法**：有限差分法、WENO/ENO 重构、SSP-RK3/RRK3 时间积分
- **核心理论**：熵稳定格式、相对论流体力学、双曲守恒律
- **开发工具**：CLion、VS Code、Git
