> Independent does not mean alone. Independent means taking ownership to think critically, recognize when you need help, and seek it out.

# TO DO list
- [ ] 单目摄像头为什么不能测量深度，而双目可以？
- [ ] 双目摄像头是如何基线校正的？
- [ ] RGBD深度摄像头和双目摄像头相比有何优势？


- [ ] Apollo技术汇报
- 进度 Apollo版本迭代 每个版本的更新内容
- 不影响项目进度可以做任何事情
- calibration list （方法、需要的材料、权限、资金来源）
- 车辆标定（油门、转向，核心内容，深入研究）
- 内容、资源、目标、时间节点 figure it out.

- [ ] 自动驾驶芯片 调研  
- 性能参数 开发环境支持 报价（估算） 
- 采埃孚-英伟达 CES2019 自动驾驶中央处理器




### 论文 & Patent
> - one by one, show your best.  
> - do more, think deeper.

**专利2 自动驾驶车辆自动过停车杆装置**
边构思，边画图
- 如果停车装置可以识别自动驾驶车辆车牌，但还是无法保证车辆安全过停车杆
**不要讨论停车杆自动抬起的方案**
**重点是识别车杆完全抬起**
- 对于限定宽度给出提高车辆定位精度的方案，防止剐蹭到路沿  

车底的亮度传感器
传感线圈中央处布置一个射灯；车底布置一个亮度传感器（light sensor）

图1 侧向示意图
图2停车场布置示意图
图3 装置工作流程示意图
图4 亮度传感器在车底盘的布置



**论文1 车道线检测？行人检测？or a ROS project?**
~参考论文：Towards End-to-End Lane Detection: an Instance Segmentation Approach~
先总结udacity工作，结合Matlab模型，给出代码生成
先出结果，后撰写论文
**AEB with matlab**  
https://ww2.mathworks.cn/help/driving/examples/sensor-fusion-using-synthetic-radar-and-vision-data.html
https://ww2.mathworks.cn/help/driving/examples/radar-signal-simulation-and-processing-for-automated-driving.html
https://ww2.mathworks.cn/help/driving/examples/model-radar-sensor-detections.html

--------------------------------------------------
- [ ] 通过系统级仿真优化HEV电气设计
https://ww2.mathworks.cn/videos/hev-modeling-106515.html?s_tid=srchtitle

- [ ] 快速搭建新能源汽车整车模型及其性能优化
https://ww2.mathworks.cn/videos/hev-modeling-simulation-and-performance-optimization-1506714632188.html?s_tid=srchtitle

- [ ] 搭建快速而准确的新能源汽车动力总成系统模型及其在系统设计优化和控制系统开发中的应用
https://ww2.mathworks.cn/videos/building-fast-and-accurate-new-energy-vehicle-powertrain-models-for-system-and-control-development-1510051647675.html?s_tid=srchtitle
------------------------------------------------------------



### done list
- [x] VPS故障
- step1 重新生成SS和SSR配置 and test it
- step2 写工单，报故障
- step3 换IP
- [x] 使用基于模型的设计方法开发混合动力总成系统 - 视频 - MATLAB & Simulink  
https://ww2.mathworks.cn/videos/model-based-design-for-hybrid-electric-powertrain-systems-82510.html?s_tid=srchtitle
- [x] 项目计划书  
- [ ] ~EDA(FPGA)课程rush掉https://www.icourse163.org/learn/HIT-1003359013?tid=1003590011#/learn/content~ 
- [x] ~查询SIL资料或论文，写一写文字https://github.com/lgsvl/simulator~  
- [x] 百度地图API程序学习与调试，跑一下，截图两张
- [x] 注册两个百度账号
- [x] Apollo项目修改
> 目标：通过部署百度Apollo、AutoWare等开源平台，提高对传感器标定、线控的量产技术积累，调整相关配置参数，匹配适应比亚迪客车及客户运营场景的自动驾驶系统.
内容：1.对车辆道路移植时需要重新标定传感器问题提出解决策略；2.针对特殊场景行驶受限问题提出控制方案；3.开发行车轨迹、乘客监控等简单物联网应用.
意义：1.建立自动驾驶系统硬件批量部署、硬件在环测试能力，降低开发成本；2.可针对不同应用场景及客户需求定制开发自动驾驶客车，有利于市场推广.

### 百度地图轨迹线
百度地图jsAPI实时画出运行轨迹，车头实时指向行驶方向，设置角度偏移
https://blog.csdn.net/skywqnan/article/details/79036262

读取本地坐标文件，在百度地图画运动轨迹（2018新华三“物联网终端行为分析”第三题）
https://blog.csdn.net/wanzhen4330/article/details/83868385

# ROS
https://industrial-training-master.readthedocs.io/en/latest/

ROS一些困难与建议
https://www.zhihu.com/question/296233185

MAV planning tools using voxblox as the map representation.  
https://github.com/ethz-asl/mav_voxblox_planning

# IoT
环境监控场景的 IoT 物联网开发实战  
https://gitbook.cn/gitchat/activity/5c2340c5ef105b18c8f7a048  
  
30 分钟搭建空气质量检测仪产品原型  
https://gitbook.cn/gitchat/activity/5c2f1f03d294ce254aa6fef7

book: JavaScript on Things  
https://www.manning.com/books/javascript-on-things  

book: IoT Projects  
https://www.packtpub.com/application-development/internet-things-programming-projects

book list: packthub iot books  
https://search.packtpub.com/?sortBy=prod-packt-main-MostRecent&refinementList%5Breleased%5D%5B0%5D=Available&refinementList%5Bcategory%5D%5B0%5D=IoT%20%26%20Hardware
### iot awesome list
https://github.com/HQarroum/awesome-iot

### Embedded System & RTOS

### Front end
Front-End Developer Handbook 2018
https://https:www.matlabexpo.com/cn/2018/proceedings/proceedings.html


Do not upload any file from working PC of BYD.

# Coding and Thinking

**实战组合式设计**
不使用编程语言的正则表达式库，实现一个基于特定语义规则的正则表达式引擎，对用户提供基本的 match 和 search 接口
https://gitbook.cn/gitchat/activity/5bc2ab0b54567e7a469a1018
