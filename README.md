# kws-stm32f7disco-cmsisNN
转载的https://github.com/ARM-software/ML-KWS-for-MCU的内容，修改了其中的一些配置

#### github源码
- 两个网址，使用第一个
https://github.com/ARM-software/ML-KWS-for-MCU

#### 下载、添加mbed以及开发板驱动
- 下载cmsis
```c
cd Deployment
git clone https://github.com/ARM-software/CMSIS_5.git
```

- 修改cmsis中内容
```c
cd CMSIS_5/CMSIS/DSP/Source/
```
将该路径下所有文件夹中对应文件夹的.c文件中的内容屏蔽（注释），否则之后的编译会上报变量重定义错误，例如：
```c
vi TransformFunctions/TransformFunctions.c 
```

- 创建mbed工程
```c
mbed new kws_realtime_test --mbedlib
```

- 添加开发板驱动已经mbed工具等
```c
cd kws_realtime_test
cp ../Examples/realtime_test/mbed_libs/*.lib .
mbed deploy
```

- 替换mbed文件夹下的内容
下载 https://os.mbed.com/users/mbed_official/code/mbed/ 中的.gz文件并拷贝到mbed工程的mbed目录下，删除原来的目录文件
```c
rm -rf 65be27845400
tar -zxf 65be27845400.tar.gz
```

- 查看mbed工程中文件
```c
mbed deploy
ls                                       
AUDIO_DISCO_F746NG  AUDIO_DISCO_F746NG.lib  BSP_DISCO_F746NG  BSP_DISCO_F746NG.lib  BUILD  LCD_DISCO_F746NG  LCD_DISCO_F746NG.lib  mbed  mbed_app.json  mbed.bld  mbed.lib  mbed_settings.py  mbed_settings.pyc
```

#### 编译
- 对象设置为f7disco开发板
```c
mbed compile -m DISCO_F746NG -t GCC_ARM \
  --source . --source ../Source --source ../Examples/realtime_test \
  --source ../CMSIS_5/CMSIS/NN/Include --source ../CMSIS_5/CMSIS/NN/Source \
  --source ../CMSIS_5/CMSIS/DSP/Include --source ../CMSIS_5/CMSIS/DSP/Source \
  --source ../CMSIS_5/CMSIS/Core/Include \
  --profile ../release_O3.json -j 8
```

#### 下载执行
- 直接将编译生成的`BUILD/DISCO_F746NG/GCC_ARM-RELEASE_O3/kws_realtime_test.bin` 文件拷贝到开发板上（开发板通过数据线连接后会在“我的电脑”下生成一个“DIS_F746NG”盘符）
