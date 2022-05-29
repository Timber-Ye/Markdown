# MATLAB/Simulink 直流电机仿真报告  

## 〇.任务要求  

构建直流电机模型，结合PWM驱动，分析电机启动、驱动电压下降、负载突增等输入下的电机响应，分析电机参数对动态响应的影响，不同PWM驱动微观续流的差异等。  

Parameter | Value | Note  
:-: | :-: | :-: | :-:  
$K_e$ | $1V/(rad/s)$ |反电动势系数|  
$K_t$ | $1N\cdot m/A$ | 转矩灵敏度|  
$R_a$ | $1.6\Omega$ | 电枢绕组的电阻|  
$L_a$ | $16mH$ | 电枢绕组的电感|    
$T_c$ | $1.6Nm$ |轻负载时的负载总阻转矩|  
$J_l$ | $0.02kgm^2$|轻负载转动惯量|  
$T_C$ | $8Nm$ | 重负载时的负载总阻转矩|  
$J_L$ | $0.4kgm^2$|重负载转动惯量|  
$f_k$ | $16kHz$ | PWM开关频率  

## 一.搭建模型  

### 1.1 直流电动机的基本关系式  

1. 电磁转矩与电枢反电势  
    + 电磁转矩$T_{em}$正比于电枢电流$I_a$:$T_{em}=K_t\times I_a$  


    + 电枢绕组中的感应电势(电枢反电势)$E_a$正比于电机转子角速度$\Omega$:$E_a=K_e\times \Omega$  

2. 转矩平衡方程式  

    由达朗贝尔原理:  
    $$  
    T_{em}=T_0+T_L+J\frac{d\Omega}{dt}=T_c+J\frac{d\Omega}{dt}  
    $$  

3. 电压平衡方程式  

    基尔霍夫电压定律:  
    $$  
    U_a=L_a\frac{dI_a}{dt}+I_aR_a+E_a  
    $$

    由电压平衡方程式可知:
    $$
    i_a(t)=\frac{U_D-E}{R_a}-(\frac{U_D-E}{R_a}-I_0)e^{-\frac{R_a}{L_a}t}
    $$

### 1.2 用电路系统等效替代机械系统  

由于电路系统更适于在计算机上进行仿真, 因此需要用一个电路系统等效替代电机的机械系统部分. 可以采用的等效方案是用GCL节点电路系统类比RJK旋转机械系统。下表列举了这两个系统广义变量以及惯性作用元件的对偶关系:  

|  RJK旋转机械系统 | GCL节点电路系统  
:- | :- | :-: |  
坐标  $\theta$ | $\psi$ |  
速度  $\Omega=\frac{d\theta}{dt}$ | $e = -\frac{d\psi}{dt}$|  
角动量  $P_\Omega=J\Omega$ | q|  
力矩  $T = \frac{dP_\Omega}{dt}$ | $i = \frac{dq}{dt}$|  
转动惯量  $J$ | $C$ |  
角加速度  $\alpha=J\frac{d\Omega}{dt}$ | $i = C\frac{du}{dt}$ 

故:
$$
T_{em}=T_0+T_L+J\frac{d\Omega}{dt}=T_c+J\frac{d\Omega}{dt} \Leftrightarrow I_{em}=I_c+C\frac{du}{dt}
$$

由上述对偶关系,可用干路电流等效电磁转矩$T_{em}$;可用一支路上的直流电流源$I_c$(DC current source)等效代替负载总阻转矩$T_c$;用一个电容代表动能储能的惯性元件(即负载), 当电容$F$在数值上等于惯性元件的转动惯量$J$大小时, 电容两端的的电压$u$在数值上也就等效负载的角速度$\omega$.

至此,获得电机模型仿真的全部参数如下表:

Parameter | Value | Note  
:-: | :-: | :-: | :-:  
$\alpha$ | $1$ |用VCVS表征反电动势与负载转速的关系, $\alpha$在数值上即$K_e$|  
$\beta$ | $1$ | 用CCCS代替电磁转矩与电枢电流的关系,$\beta$在数值上即$K_t$|  
$R_a$ | $1.6\Omega$ | 电枢绕组的电阻|  
$L_a$ | $16mH$ | 电枢绕组的电感|    
$I_c$ | $1.6A$ |恒流源替换轻负载时的负载总阻转矩|  
$C_l$ | $0.02F$|电容替代轻负载转动惯量|  
$I_C$ | $8A$ | 恒流源重负载时的负载总阻转矩|  
$C_L$ | $0.4F$|电容重负载转动惯量|  
$f_k$ | $16kHz$ | PWM开关频率 

### 1.3 直流电机模型搭建及评估

在MATLAB R2020a的Simulink搭建的仿真模型如下图所示:

<center>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/电机simulink模型.png" width = "60%" alt=""/>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/电机simulink模型封装.png" width = "20%" alt=""/>
<br>
<div style="color:orange; border-bottom: 1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 2px;">
    左图为搭建的模型, 右图为封装外观
</div>
</center>

下面采用系统辨识的方法验证仿真模型是否正确:

+ 理论值计算:

    电动机的机电时间常数:
    $$\tau_m=\frac{R_aJ}{K_eK_t}=0.032s$$
    电动机的电磁时间常数:
    $$
    \tau_e=\frac{L_a}{R_a}=0.01s
    $$

    以角速度$\Omega$为输出量, 令干扰力矩$T_c=0$, 则理论上直流电机的传递函数可以写为:
    $$
    \frac{\Omega(s)}{U_a(s)}=\frac{1/K_e}{\tau_e\tau_ms^2+\tau_ms+1}=\frac{1}{0.0003s^2+0.032s+1}
    $$

+ 系统辨识:

    输入M序列对仿真系统进行系统辨识,辨识结果为:
    $$
    \frac{\Omega(s)}{U_a(s)}=\frac{4939}{s^2+157.5s+4937}=\frac{1}{0.0002s^2+0.032s+1}
    $$

由于$\tau_e$较小, 认为$1/{\tau_e}$超过了控制系统的通频带。故$s^2$可以忽略,并且此时认为系统辨识结果与理论近似,搭建的模型符合理论模型.

### 1.4 PWM放大器输出级模型搭建

+ 绝缘栅双极型晶体管IGBT

    直接采用压控开关就能搭建出一个理想的IGBT模型:

<center>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/IGBT模型.png" width = "60%" alt=""/>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/IGBT封装.png" width = "20%" alt=""/>
<br>
<div style="color:orange; border-bottom: 1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 2px;">
    左图为搭建的模型, 右图为封装外观
</div>
</center>

+ 控制信号发生器

    由于双极性PWM驱动中需要输入两个反相位的方波, 根据需要搭建了如下的控制信号发生器,两个输出端分别输出两个互为反相位的方波信号:

<center>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/ControlSignal.png" width = "60%" alt=""/>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/ControlSignal封装.png" width = "30%" alt=""/>
<br>
<div style="color:orange; border-bottom: 1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 2px;">
    左图为搭建的模型, 右图为封装外观
</div>
</center>

+ H形桥式电路

    H形桥式电路是典型的脉宽调制型功率放大器的输出级, 电路由四个绝缘栅型双极型晶体管和四个续流二极管组成.

<center>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/H桥.png" width = "70%" alt=""/>
<br>
</center>

## 二.双极性输出PWM驱动分析电机特性

<center>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/双极性PWM输出.png" width = "70%" alt=""/>
<br>
<div style="color:orange; border-bottom: 1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 2px;">
    模型组合实现双极型PWM驱动
</div>
</center>

### 2.1 电机启动

设定电机轻负载启动,驱动电压$U_D=48V$, PWM占空系数$\gamma=75\%$,可以获得如下的仿真曲线

<center>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/双极型启动分析.png" width = "100%" alt=""/>
<br>
<div style="color:orange; border-bottom: 1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 2px;">
    左图为电枢电流;右图为电机转速
</div>
</center>

由电枢电流曲线发现,在电机启动后的$0.025s$之内,将达到一个远高于额定电流的启动电流.具体的产生原因是,电动机在通电的瞬间,转子还是静止不动的$\Omega=0\Rightarrow E=0$,由电压平衡方程式可知,电枢电流近似指数上升趋势.但是在电流上升过程中,电机的输出转矩也在不断增加,转子的角速度$\Omega\uparrow$,导致$E\uparrow$,所以电流不可能一直增大,而是达到一个峰值后恢复至静态工作值.

由此可知,对于一般的直流电机,不可以加额定电压直接启动,也不能频繁换向,否则将可能使电动机的绕组过热,损坏设备.

电动机在启动过程中,电枢电流由电机参数$R_a, L_a$, 负载以及驱动电压等共同决定. 通常要求启动电流$I_s \geq (2\sim2.5)I_N$,故常用的启动方法是电枢回路串联电阻启动或降低电枢电压启动.

### 2.2 驱动电压下降

启动完成后,可按照直流电动机静态特性进行分析.在电枢控制(磁通不变)情况下,电机的调节特性是指当输出转矩为一参变量时,转速$\Omega$与电枢电压$U_a$的关系:
$$
\Omega = \frac{U_a-\frac{T_{const}}{K_t}R_a}{K_e}
$$

又因为直流电机可以将所有高频交流分量完全衰减, 故模型中搭建的脉冲宽度调制型放大器在直流电动机的控制系统中可以等效为一个电压放大器,等效为放大系数为$K$的比例环节.此时,起作用的信号是缓变的直流分量$U_{av}=\rho U_D$, 在双极性输出的情况下$\rho = 2\gamma-1$.

理论上,当电机轻负载运行,驱动电压$U_D=48V$, PWM占空系数$\gamma=75\%$时,有:
$$
U_{av}=\rho U_D = (2\gamma-1)U_D=24V \\
\Omega = \frac{U_{av}-\frac{T_{const}}{K_t}R_a}{K_e}=21.44rad/s \\
E = K_e\Omega = 21.44V < U_{av} \\
$$
从仿真运行曲线测量也可得$\Omega_m=21.41rad/s$,与理论值近似.在电磁转矩平均值和负载转矩相平衡(准稳定状态)时,电枢电流仅仅取决于电磁转矩(输出转矩),这是由于电机本身的频率特性.故无论占空系数为多少,准稳态时,电枢电流的平均值$I_{av}$恒等于$T_{em}/K_t=T_c/K_t=1.6A$,仿真出来的电枢变化曲线也能够反映PWM放大器的这一特性.

在$t=0.8s$时刻将驱动电压由$48V$突降至$28V$, 并在$t=1.6s$时刻由$28V$突降至$5.12V$,得到如下的仿真结果:

<center>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/驱动电压突降.png" width = "100%" alt=""/>
<br>
<div style="color:orange; border-bottom: 1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 2px;">
    左图为电枢电流;右图为电机转速
</div>
</center>

1. 动态过程:驱动电压$U_D$突降,由于$E$不能够突变,$\Delta(U_D-E)$随之突降. $I_a$会迅速下降,同时$T_{em}\downarrow$, $T_{em}<T_c\Rightarrow \frac{d\Omega}{dt}<0$, 故$\Omega\downarrow, E\downarrow$. 当$I_a$下降至某一峰值,$\Delta(U_D-E)$开始增大, $I_a\uparrow, T_{em}\uparrow$直到$T_{em}=T_c$, 电机恢复准稳态运行.

2. 稳态过程:恢复稳态后依旧由电机调节特性方程有:
    $U_D(V)$ | $U_{av}=(2\gamma-1)U_D$ | $\Omega(rad/s)$(括号内为测量值)  
    :-: | :-: | :-: | :-:  
    $28$ | $14V$ | $11.44(11.41)$|
    $5.12$ | $2.56V$ | $0(0.03)$ |

    其中$5.12V$即启动电压/死区电压$U_s$. 这是由于阻转矩不为零造成的.

### 2.2 负载突增

设定电机轻负载启动,驱动电压$U_D=48V$, PWM占空系数$\gamma=75\%$. 在$t=0.8s$时刻将轻负载置换为重负载, 在$t=5.8s$时刻将重负载的总负载阻转矩提高至$15Nm$,得到如下的仿真曲线:

<center>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/负载突增.png" width = "100%" alt=""/>
<br>
<div style="color:orange; border-bottom: 1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 2px;">
    左图为电枢电流;右图为电机转速
</div>
</center>

1. 动态过程:
   
   值得注意的是, 在$t=0.8s$时刻电枢电流以及转速均发生了突变,有悖基本的电路和机械原理.这其实是由于仿真过程中,以切换电容来代替更换负载,新的电容没有携带任何电荷,两端电压为零且不可突变,这便是图中电流从$1.6A$突变为$0A$, 转速从$23(rad/s)$突变为$0$的原因.即实际过程中,更换负载也是必须先将原来的取下(取下的过程就可以看成转速突变为$0$),刚装上的新的负载转速也必定是从零开始的.

   同时,在$t=5.8s$时刻只增大了阻力矩,没有改变系统的转动惯量, 电流就不会发生突变了.转速下降和电流的增大原因是: 阻力矩$T_c\uparrow \Rightarrow T_{em}<T_c \Rightarrow \Omega\downarrow \Rightarrow E\downarrow \Rightarrow I_a\uparrow$

2. 稳态过程:恢复稳态后依旧由电机调节特性方程有:
    $T_c(Nm)$ | $\Omega(rad/s)$(括号内为测量值)  
    :-: | :-:  
    $1.6$ | $1$ |
    $8$ | $11.20(11.18)$ |
    $15$ | $0(-0.02)$ |

    其中$15Nm$即堵转力矩$T_s$. 

### 2.3 电机参数对动态响应的影响

由于电机可以看作为两个惯性环节的串联, 分析电机参数对动态响应的影响,只需要考虑参数是如何影响这两个惯性环节的时间常数即可.

1. 机电时间常数

        也叫机械时间常数, 是电动机从启动到转速达到空载转速的63.2%时所经历的时间。

    机电时间常数$\tau_m$是决定电动机的惯性和过渡过程时间(响应速度)的主要数据.表达式:
    $$
    \tau_m = \frac{R_aJ}{K_eK_t}=Jtan\beta = \frac{J\omega_0}{T_s}
    $$
    其中$\omega_0$为理想空载转速($rad/s$),$T_s$为堵转力矩, $tan\beta$为机械特性斜率的绝对值.

    由表达式可知, 当$R_a$增大, $K_e, K_t$减少时,机电时间常数增大,电机的动态响应过程时间延长.下图是分别当$R_a=1.6\Omega$和$3.2\Omega$时电机的启动过程曲线.

2. 电磁时间常数

        即电路中的时间常数,反映了电流充放电的快慢.在RL串联回路中表示电感的电流减小到原来的1/e需要的时间。

    表达式为:
    $$
    \tau_e=\frac{L_a}{R_a}
    $$
    在实际中,电磁时间常数远远小于机电时间常数,故电磁惯性环节的动态响应过程常常被忽略.从前面电机启动、驱动电压突增、负载突增这些动态过程中,都可以发现,电磁回路中状态的变化,电枢电流都能很快响应;而机电回路中状态的变化,电枢电流的响应都较为缓慢.

    由表达式可知,当$R_a$减小,$L_a$增大时, 电磁时间常数增大,电机的动态响应过程时间延长.下图是当$L_a=0.16H\Omega$时电机的启动过程曲线.


    当$L_a=0.16H\Omega$时电磁时间常数已经和机电时间常数具有可比性,电机不能再看作一个一阶系统,而是一个二阶系统,并且由响应曲线发现直流电机成为欠阻尼二阶系统.


## 三.三种PWM驱动的微观续流差异

### 3.1 单极性PWM驱动
在电机同侧的一对开关管如$T_1, T_2$的栅极上加相位相反的方波控制电压，使其工作在交替的开关状态。另一侧两个开关管中$T_4$的栅极一直加正电压使其饱和开通, $T_3$的栅极一直加负电压使其关断。这时的输出电压是正的方波电压。

<center>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/单极性PWM输出.png" width = "70%" alt=""/>
<br>
<div style="color:orange; border-bottom: 1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 2px;">
    模型组合实现单极型PWM驱动
</div>
</center>

### 3.2 受限单极性PWM驱动

在桥式电路中只有一个开关管工作在开关状态.

<center>
<img style="border-radius: 0.3125em;" 
src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/受限极性PWM输出.png" width = "70%" alt=""/>
<br>
<div style="color:orange; border-bottom: 1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 2px;">
    模型组合实现受限单极型PWM驱动
</div>
</center>

### 3.3 微观续流差异

1. 波形
    <center>
    <img style="border-radius: 0.3125em;" 
    src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/双极性PWM微观.png" width = "70%" alt=""/>
    <img style="border-radius: 0.3125em;" 
    src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/单极性PWM微观.png" width = "70%" alt=""/>
    <img style="border-radius: 0.3125em;" 
    src="https://gitee.com/yehanqiao/Markdown/raw/master/IMG/自动控制实践/受限单极性PWM微观.png" width = "70%" alt=""/>
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">
        从上到下依次为双极性PWM微观电流, 单极性PWM微观电流, 受限单极性PWM微观.
    </div>
    </center>

2. 电流脉动量

    当电路工作在准稳态时,在微观上,电流$i_a$的瞬时值是周期性变化的. 一个周期内电流最大值为$i_{amax}$,最小值为$i_{amin}$,定义电流的脉动量$\Delta I_a$为:
    $$
    \Delta I_a = i_{amax}-i_{amin}
    $$

    假设电流的波形是直线, 可以证明:
    $$
    \Delta I_a=\frac{AI_s\gamma(1-\gamma)}{\tau_e f_k}=\frac{AU_D(1-\rho^2)}{4L_af_k}
    $$
    其中,单极性输出时$A=1$, 双极性输出时$A=2$.

    下面是当驱动电压$U_D=48V$, PWM占空系数$\gamma=75\%$,空载状态下,三种驱动方式的电流脉动量的比较.在双极性输出时的脉动量比单极性输出时的脉动量大。

    驱动方式 | 电流脉动理论值$\Delta I_a$ | 电流脉动测量值$\Delta \dot{I_a}$  
    :-: | :-: | :-: | :-:  
    双极性 | $0.0070A$ | $0.0071A$|
    单极性 | $0.0035A$ | $0.0035A$ |
    受限单极性 |  —— |  $0.0035A$ |

