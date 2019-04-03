> 重视Project。make your hands dirty. you could learning more by doing more practice.  

> 批判思考自己能力与知识的局限性 Independent does not mean alone. Independent means taking ownership to think critically, recognize when you need help, and seek it out.

> 什么是记笔记？记笔记是记录自己的想法，记录是怎么完成任务、解决问题的。不是我看了什么就记下来，仅仅记下内容很快就忘。记录我怎么理解的，怎么解决的就可以加深印象，快速回顾。比如cpp中static的用法，罗列别人不如记一下你自己怎么理解的，eg1. 对于一个普通变量而言，加个static可以在将其放在静态区域内...而不是1.修饰普通变量，修改变量的存储区域和生命周期balabala这样陈词的罗列。

---------------------------------------------------
## 目录

* [刷题：interv](#Interv)
  * [cpp](#CPP_overview)
  * [sword to offer](#sword2offer)
  * [CSAPP & Concurrent & Networking](#later)
* [阅读 & MOOC：补充工作知识](#learning)
  * [MOOC](#MOOC)
  * [参考学习资料](#参考学习资料)
* [Project：提升能力](#Project)
  * [Project Now](#Project_now)
  * [do favor & project list](#Project_later)
* [工作计划](#work)
  * [专利](专利)
  * [论文](论文)
  * [Apollo项目](Apollo项目)

## Interv 
你需要：  
### CPP_overview
- [ ] cpp primer 快速结束
  
step1: 结合[这个文档](https://github.com/huihut/interview)，列一个问题清单  
step2: 找到问题对应的知识点和关键词，在cpp primer中进行深化  
step3: 回顾问题清单，复习总结  

### sword2offer
[1-68](sword2offer)

### Later
CSAPP concurrent networking
先刷完sword2offer再分解





## Learning

### MOOC
coursera self-driving 

### 参考学习资料
**[Apollo控制模块](https://blog.csdn.net/u013914471)**    
**[Apollo博客](https://blog.csdn.net/sunyoop)**   

more to see [参考资料](#参考资料)

## Project
### Project_now
OOD

### Project_List
- [ ] [rendering course on Mac~](https://github.com/ssloy/tinyrenderer/wiki)  
- computer graphics  
- and learn more  



## Work
- [ ] 科目1题库 105-300  
- *一天300题，刷两遍* 
- 过年闲着蛋疼时刷
### 专利 
- [x] 完成并提交一篇
- [ ] 构思一篇: 功能降级/  

### 论文  
- [ ] 完成并提交一篇
- [ ] 构思一篇：音频传输/SIL/PIL  

### Apollo项目  
- [ ] Apollo研究项目开发板调研报告
- [ ] vehicle calibration

**嵌入式的CAN报文收发**  
- [ ] 单目摄像头为什么不能测量深度，而双目可以？
- [ ] 双目摄像头是如何基线校正的？
- [ ] RGBD深度摄像头和双目摄像头相比有何优势？
- [ ] https://blog.csdn.net/csdnnews/article/details/86542188 

- [ ] Apollo技术汇报
- 进度 Apollo版本迭代 每个版本的更新内容
- 不影响项目进度可以做任何事情
- calibration list （方法、需要的材料、权限、资金来源）
- 车辆标定（油门、转向，核心内容，深入研究）
- 内容、资源、目标、时间节点 figure it out.

- [ ] 自动驾驶芯片 调研  
- 性能参数 开发环境支持 报价（估算） 
- 采埃孚-英伟达 CES2019 自动驾驶中央处理器

--------------------------------------------------------------------------------------
# 参考资料

**一些ros的参考资料**
https://industrial-training-master.readthedocs.io/en/latest/  

ROS一些困难与建议  
https://www.zhihu.com/question/296233185  
  
MAV planning tools using voxblox as the map representation.    
https://github.com/ethz-asl/mav_voxblox_planning  



---------------------------------------------------------------




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


Do not upload any file from working PC of BYD.

# Coding and Thinking

**实战组合式设计**
不使用编程语言的正则表达式库，实现一个基于特定语义规则的正则表达式引擎，对用户提供基本的 match 和 search 接口
https://gitbook.cn/gitchat/activity/5bc2ab0b54567e7a469a1018
