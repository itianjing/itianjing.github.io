# 昇腾NPU与NVIDIA GPU利用率

在AI算力部署与监控中，昇腾NPU（以910C为代表）与NVIDIA GPU的利用率指标是衡量设备性能、排查瓶颈的核心依据。但大家很容易混淆不同利用率指标的含义，导致无法精准对比两者算力效率。本文结合昇腾910C NPU与NVIDIA GPU的实际应用场景，系统梳理各类利用率指标的定义、计算逻辑、查看方法及对标关系，帮助大家快速掌握核心要点，规避认知误区。



## 一、核心前提：利用率指标的本质差异

昇腾NPU与NVIDIA GPU的利用率指标，核心差异集中在「度量对象」和「计算口径」上：

- 前者聚焦AI专用计算单元的真实算力；后者侧重设备整体忙闲状态；两者不能直接等同，需按场景精准对标。

昇腾910C作为双芯封装（2×910B Die）的高性能NPU，其指标体系更贴合AI计算场景，而NVIDIA GPU的指标则更偏向设备级综合监控。



## 二、昇腾910C NPU利用率指标详解

昇腾910C的利用率指标通过npu-exporter采集，核心分为「AI算力类」和「设备综合类」，对应不同的监控场景，且需结合其双芯、128GB HBM显存、400W TDP的硬件特性理解。

### （一）核心算力指标：aicore_usage_ratio（AI Core利用率）

这是昇腾910C最核心的AI算力指标，也是与NVIDIA GPU真实算力对比的关键。

- 度量对象：仅针对910C的AI Core计算单元（每个Die含64个AI Core，双芯共128个），包含Cube矩阵运算单元、Vector向量运算单元、Scalar标量运算单元，是AI专用计算引擎。

- 计算口径：通过芯片内置PMU（性能监控单元）统计，反映AI Core计算单元在采样周期内的活跃时钟周期占比（取值0~1），不受显存拷贝、CPU等待、IO操作影响，能精准体现AI算力的真实忙碌程度。

- 适用场景：AI模型训练/推理的性能调优、算力饱和判断。910C训练时该指标>70%为优秀，50%~70%为一般，<50%为偏低；推理时>40%为优秀。

### （二）设备综合指标：npu_chip_info_overall_utilization（芯片整体利用率）

该指标是昇腾910C的设备级综合利用率，对应NVIDIA GPU的gpu_util，用于判断设备整体忙闲状态。

- 度量对象：覆盖全芯片核心及组件，包括AI Core、AI CPU、Ctrl CPU、Data CPU、DMA引擎、HBM控制器、PCIe控制器等，是全芯片的加权综合忙闲占比。

- 计算口径：统计采样周期内全芯片各类组件的活跃时间占比，包含系统开销、数据搬运、调度等待等非AI计算操作，容易出现“指标偏高但AI Core闲置”的情况。

- 适用场景：设备运维监控、资源调度，判断设备是否在线、是否有任务在运行，不适合衡量AI算力效率。



### （三）910C的硬件结构补充

理解910C的利用率指标，需明确其硬件组成：910C是双Die合封结构，每个Die除64个AI Core外，还包含AI CPU（ARM架构，负责算子调度）、Ctrl CPU（负责设备控制、电源管理）、Data CPU/DMA引擎（负责数据搬运）、DVPP（数字视觉预处理模块，硬件加速编解码）等独立于AI Core的核心组件。这些组件的忙碌状态，都会体现在overall_utilization中，也是其与NVIDIA GPU结构的核心差异之一。

![910c架构图](https://miles-typora-project-1304924938.cos.ap-hongkong.myqcloud.com/910c%E6%9E%B6%E6%9E%84%E5%9B%BE.jpg)



## 三、NVIDIA GPU利用率指标详解

NVIDIA GPU的利用率指标主要通过nvidia-smi、DCGM（Data Center GPU Management）工具采集，核心分为「设备综合类」「算力核心类」，其指标命名与计算逻辑和昇腾910C有明确对标关系。

### （一）设备综合指标：gpu_util（GPU利用率）

这是最常用但最易误解的指标，对应昇腾910C的npu_chip_info_overall_utilization。

- 官方定义：采样周期内（通常1秒或1/6秒），GPU上有至少一个CUDA kernel（核函数）正在执行的时间占比（取值0~100%）。

- 核心特点：只判断“设备是否在忙”，不区分忙的类型——无论用了多少CUDA Core、是否用到Tensor Core，哪怕只有少量数据拷贝、小kernel频繁启动，都算“忙”，因此容易出现“gpu_util 100%但实际算力闲置”的虚高现象。

- 适用场景：设备运维、任务调度，判断GPU是否闲置，不适合衡量AI算力效率。

### （二）核心算力指标：sm_util（SM利用率）

这是NVIDIA GPU的真实算力指标，对应昇腾910C的aicore_usage_ratio，是衡量AI算力效率的关键。

- 度量对象：SM（Streaming Multiprocessor），每个SM包含多个CUDA Core和Tensor Core，是GPU的核心计算单元。

- 计算口径：反映SM单元在采样周期内的实际忙碌比例，直接体现CUDA Core的利用效率，是真正的“算力利用率”，不受非计算操作影响。


查看方法：无需额外安装工具，通过原生nvidia-smi即可查看，命令如下：
```      
实时查看1次： nvidia-smi dmon -s u -c 1（输出中“sm”列即为SM利用率）
        
持续刷新查看：nvidia-smi dmon -s u
```


      

## 四、NPU与GPU利用率对标关系

结合昇腾910C与NVIDIA GPU的指标特性，两者的利用率指标需按“场景分类对标”，避免直接混淆，具体对应关系如下表所示：

|监控场景|昇腾910C（npu-exporter）|NVIDIA GPU（nvidia-smi/DCGM）|对标说明|
|---|---|---|---|
|AI真实算力对比|aicore_usage_ratio（AI Core利用率）|sm_util（SM利用率）|✅ 完全对标，均反映核心计算单元的真实算力效率，是性能调优的核心指标|
|设备整体忙闲对比|npu_chip_info_overall_utilization（芯片整体利用率）|gpu_util（GPU利用率）|✅ 完全对标，均为设备级综合忙闲占比，包含非计算开销，易虚高，适合运维监控|
|显存状态对比|mem_used_ratio（显存使用率）|DCGM_FI_DEV_FB_USED_PERCENT（显存使用率）|✅ 口径一致，可直接对比，反映显存占用情况|
|显存带宽对比|mem_bandwidth_usage_ratio（显存带宽利用率）|DCGM_FI_DEV_MEM_COPY_UTIL（显存带宽利用率）|✅ 基本等价，反映显存数据传输效率|
## 五、常见误区与实践建议

### （一）核心误区

1. 误区1：用gpu_util（NVIDIA）与aicore_usage_ratio（NPU）直接对比——两者度量对象完全不同，前者是设备忙闲，后者是真实算力，对比无意义。

2. 误区2：认为gpu_util 100%就是算力跑满——实际可能只是小kernel频繁启动或数据拷贝，SM利用率可能很低，需结合sm_util判断。

3. 误区3：忽略910C双芯特性——910C双Die封装，每张卡对应2个NPU device，每个device独立统计利用率，监控和告警需按双芯设计。

4. 误区4：混淆NPU的AI Core与其他核心——910C的AI Core只是芯片的一部分，AI CPU、Ctrl CPU等核心的忙碌会提升overall_utilization，但不代表AI算力提升。

### （二）实践应用建议

1. 性能调优场景：优先关注“aicore_usage_ratio ↔ sm_util”，这两个指标能精准反映AI算力瓶颈，若指标偏低，需排查数据预处理、batch size、算子融合等问题。
2. 运维监控场景：关注“overall_utilization ↔ gpu_util”，用于判断设备是否闲置、资源调度是否合理，无需过度关注其数值高低，重点看“是否持续为0”（设备闲置）或“持续100%但算力低”（存在瓶颈）。

