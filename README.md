[toc]

# Arduino与蜂鸣器奏响欢乐斗地主

## tone()函数

Arduino 的 `tone()`函数可以在一个引脚上产生一个特定频率的方波,占空比 50%，持续时间可以设定。而 `noTone()`函数则关闭该引脚上的脉冲信号输出。在这个引脚上连接一个蜂鸣器，就能发出 Tone()函数指定频率的声音。如果这个引脚已经在播放音乐， 改变 tone()的频率值，蜂鸣器就会改变发声音调。



## 简谱知识

<img src="https://gitee.com/Dream-and-dreaming/images/raw/master/img/欢乐斗地主.jpg" alt="欢乐斗地主" style="zoom: 5525%;" />

### 音的高低

基本音符：1，2，3，4，5，6，7

![image-20210409165112521](https://gitee.com/Dream-and-dreaming/images/raw/master/img/image-20210409165112521.png)

此外，还有倍高音与倍低音

倍高音是在数字上加2个点，倍低音是在数字下方加2个点

### 音的长短

在简谱中，把 `1 2 3 4 5 6 7` 称为4分音符

- 在数字下方加1条横线，则变为8分音符（音长减小一半），加2条横线变为16分音符（音长再减小一半）
- 在数字右边加1条横线，变为2分音符（音长增加一倍）
- 在数字右边加1个点，音长在原来基础上增加1/2



### arduino中音的高低表示方法

在arduino中，有一个 `pitches.h`文件，其定义了“1 2 3 4 5 6 7”这几个音符的频率以及它们相应高低音的频率，内容如下：

```c
/*************************************************
 * Public Constants
 *************************************************/

#define NOTE_B0  31
#define NOTE_C1  33
#define NOTE_CS1 35
#define NOTE_D1  37
#define NOTE_DS1 39
#define NOTE_E1  41
#define NOTE_F1  44
#define NOTE_FS1 46
#define NOTE_G1  49
#define NOTE_GS1 52
#define NOTE_A1  55
#define NOTE_AS1 58
#define NOTE_B1  62
#define NOTE_C2  65
#define NOTE_CS2 69
#define NOTE_D2  73
#define NOTE_DS2 78
#define NOTE_E2  82
#define NOTE_F2  87
#define NOTE_FS2 93
#define NOTE_G2  98
#define NOTE_GS2 104
#define NOTE_A2  110
#define NOTE_AS2 117
#define NOTE_B2  123
#define NOTE_C3  131
#define NOTE_CS3 139
#define NOTE_D3  147
#define NOTE_DS3 156
#define NOTE_E3  165
#define NOTE_F3  175
#define NOTE_FS3 185
#define NOTE_G3  196
#define NOTE_GS3 208
#define NOTE_A3  220
#define NOTE_AS3 233
#define NOTE_B3  247
#define NOTE_C4  262
#define NOTE_CS4 277
#define NOTE_D4  294
#define NOTE_DS4 311
#define NOTE_E4  330
#define NOTE_F4  349
#define NOTE_FS4 370
#define NOTE_G4  392
#define NOTE_GS4 415
#define NOTE_A4  440
#define NOTE_AS4 466
#define NOTE_B4  494
#define NOTE_C5  523
#define NOTE_CS5 554
#define NOTE_D5  587
#define NOTE_DS5 622
#define NOTE_E5  659
#define NOTE_F5  698
#define NOTE_FS5 740
#define NOTE_G5  784
#define NOTE_GS5 831
#define NOTE_A5  880
#define NOTE_AS5 932
#define NOTE_B5  988
#define NOTE_C6  1047
#define NOTE_CS6 1109
#define NOTE_D6  1175
#define NOTE_DS6 1245
#define NOTE_E6  1319
#define NOTE_F6  1397
#define NOTE_FS6 1480
#define NOTE_G6  1568
#define NOTE_GS6 1661
#define NOTE_A6  1760
#define NOTE_AS6 1865
#define NOTE_B6  1976
#define NOTE_C7  2093
#define NOTE_CS7 2217
#define NOTE_D7  2349
#define NOTE_DS7 2489
#define NOTE_E7  2637
#define NOTE_F7  2794
#define NOTE_FS7 2960
#define NOTE_G7  3136
#define NOTE_GS7 3322
#define NOTE_A7  3520
#define NOTE_AS7 3729
#define NOTE_B7  3951
#define NOTE_C8  4186
#define NOTE_CS8 4435
#define NOTE_D8  4699
#define NOTE_DS8 4978
```

其中，用 `C D E F G H A B` （注意顺序）来分别代表 `1 2 3 4 5 6 7`

- 用 `C4`代表代表 1，`C3`代表 1的低音，`C2`代表超低音，以此类推
- 用 `C5`代表 1的高音，`C6`代表超高音，以此类推

<img src="https://gitee.com/Dream-and-dreaming/images/raw/master/img/07_pitches.h中的音调定义.png" alt="07_pitches.h中的音调定义" style="zoom:25%;" />



## 代码

```c
#include "pitches.h"


//欢乐斗地主
//定义音高低
short melody[] = {
  NOTE_G4, NOTE_G4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_G4, NOTE_C4, NOTE_C4, NOTE_D4, NOTE_C4, NOTE_A3, NOTE_G3, NOTE_A3, NOTE_A3, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_D4, NOTE_C4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_D4, NOTE_G4, NOTE_G4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_G4, NOTE_D4, NOTE_D4, NOTE_E4, NOTE_D4, NOTE_C4, NOTE_A3, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_D4, NOTE_E4, NOTE_A4, NOTE_E4, NOTE_G4, NOTE_D4, NOTE_E4, NOTE_D4, NOTE_A3, NOTE_C4, NOTE_A4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_G4, NOTE_A4, NOTE_E4, NOTE_G4, NOTE_G4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_G4, NOTE_D4, NOTE_D4, NOTE_E4, NOTE_D4, NOTE_C4, NOTE_D4, NOTE_C4, NOTE_A3, NOTE_G3, NOTE_D4, NOTE_C4, NOTE_E4, NOTE_A4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_G4, NOTE_A4, NOTE_E4, NOTE_G4, NOTE_G4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_G4, NOTE_D4, NOTE_D4, NOTE_E4, NOTE_D4, NOTE_C4, NOTE_D4, NOTE_C4, NOTE_A3, NOTE_G3, NOTE_C4, NOTE_C4, 0, NOTE_G3, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_G4, NOTE_G4, NOTE_G4, NOTE_E3, NOTE_G4, NOTE_G3, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_G3, NOTE_A3, NOTE_E4, NOTE_D4, NOTE_D4, NOTE_D4, NOTE_C4, NOTE_D4, NOTE_E4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_A4, NOTE_C5, NOTE_A4, NOTE_G4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_D4, NOTE_A4, NOTE_E4, NOTE_D4, NOTE_G4, NOTE_E4, NOTE_D4, NOTE_C4, NOTE_C4, NOTE_A3, NOTE_C4, NOTE_G3, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_G4, NOTE_G4, NOTE_G4, NOTE_E4, NOTE_G4, NOTE_G3, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_D4, NOTE_C4, NOTE_D4, NOTE_E4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_A4, NOTE_C5, NOTE_A4, NOTE_G4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_D4, NOTE_D4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_G3, NOTE_E4, NOTE_D4, NOTE_C4, NOTE_D4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_A4, NOTE_D5, NOTE_C5, NOTE_G4, NOTE_C5, NOTE_E4, NOTE_E4, NOTE_D4, NOTE_C4, NOTE_C4, NOTE_A3, NOTE_D4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_G3, NOTE_A3, NOTE_A3, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_G4, NOTE_A4, NOTE_E4, NOTE_G4, NOTE_D4, NOTE_E4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_A4, NOTE_A4, NOTE_A4, NOTE_C5, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_D4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_G3, NOTE_D4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_C4, NOTE_E4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_A4, NOTE_C5, NOTE_A4, NOTE_G4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_D4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_G3, NOTE_D4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_C4, NOTE_D4, NOTE_D4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_G4, NOTE_A4, NOTE_C5, NOTE_A4, NOTE_C5, NOTE_A3, NOTE_A3, NOTE_C4, NOTE_A3, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_G4, NOTE_A4, NOTE_G4, NOTE_D4, NOTE_E4, NOTE_A3, NOTE_A3, NOTE_C4, NOTE_A3, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_D4, NOTE_E4, NOTE_D4, NOTE_D4, NOTE_E4, NOTE_D4, NOTE_C4, NOTE_D4, NOTE_A3, NOTE_A3, NOTE_C4, NOTE_A3, NOTE_G3, NOTE_A3, NOTE_C4, NOTE_D4, NOTE_A4, NOTE_G4, NOTE_G4, NOTE_A4, NOTE_G4, NOTE_D4, NOTE_E4, NOTE_E4, NOTE_G4, NOTE_A4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_A4, NOTE_A3, NOTE_A3, NOTE_G3, NOTE_A3, NOTE_C4, 0, 0, NOTE_E4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_A4, 0, 0, NOTE_A4, NOTE_A4, NOTE_A4, NOTE_A4, NOTE_G4, NOTE_D4, NOTE_E4, NOTE_A3, NOTE_A3, NOTE_G3, NOTE_A3, NOTE_C4, 0, 0, NOTE_E4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_A4, 0, 0, NOTE_A4, NOTE_A4, NOTE_A4, NOTE_A4, NOTE_G4, NOTE_E4, NOTE_A4, NOTE_E4, NOTE_D4, NOTE_E4, NOTE_G4, NOTE_C5, NOTE_B4, NOTE_G4, NOTE_A4
};

//定义音长，8表示8分音符
byte noteDurations[] = {
  8, 16, 16, 8, 8, 2, 8, 16, 16, 8, 8, 2, 8, 16, 16, 8, 8, 8, 8, 8, 8, 16 / 3, 16, 8, 8, 2, 8, 16, 16, 8, 8, 2, 8, 16, 16, 8, 8, 2, 8, 8, 8, 8, 8, 8, 8, 8, 16 / 3, 16, 8, 8, 2, 8, 16, 16, 8, 8, 8 / 3, 8, 8, 16, 16, 8, 8, 2, 8, 16, 16, 8, 8, 8, 8, 8, 8, 8 / 3, 8, 2, 8, 16, 16, 8, 8, 8 / 3, 8, 8, 16, 16, 8, 8, 2, 8, 16, 16, 8, 8, 8, 8, 8, 8, 8 / 3, 8 / 3, 4, 8, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 4, 8, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 4, 8, 16, 16, 8, 8, 16, 16, 16, 16, 8, 16, 16, 8, 16, 16, 16, 16, 16, 16, 8, 16, 16, 4, 8, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 4, 8, 16, 16, 8, 8, 16, 16, 16, 16, 4, 8, 16, 16, 8, 8, 16, 16, 16, 16, 8, 16, 16, 16, 16, 16, 16, 8, 8, 8, 8, 4, 32 / 3, 32, 16, 16, 16, 16, 8, 8, 8, 4, 8, 16, 16, 8, 16, 16, 16, 16, 16, 16, 4, 8, 16, 16, 8, 8, 16, 16, 16, 16, 4, 8, 16, 16, 8, 8, 16, 16, 16, 16, 8, 16, 16, 8, 16, 16, 8, 16, 16, 8, 8, 16, 16, 16, 16, 4, 8, 16, 16, 8, 8, 16, 16, 16, 16, 8, 16, 16, 8, 16, 16, 8, 8, 16, 16, 16, 16, 4, 16, 16, 16, 16, 8, 16, 16, 8, 8, 4, 8, 16, 16, 8, 8, 8, 8, 8, 8, 8, 16, 16, 8, 8, 2, 8, 16, 16, 8, 8, 8, 8, 8, 8, 8, 16, 16, 8, 8, 2, 8, 16, 16, 8, 8, 8, 8, 8, 8, 8, 16, 16, 8, 8, 8 / 3, 16, 16, 8 / 3, 8, 4, 4, 1 / 2, 8, 16, 16, 8, 8, 4, 4, 8, 16, 16, 8, 8, 4, 4, 16 / 3, 16, 8, 8, 4, 4, 1 / 2, 8, 16, 16, 8, 8, 4, 4, 8, 16, 16, 8, 8, 4, 4, 16 / 3, 16, 8, 8, 4, 4, 1 / 2, 16 / 3, 16, 8, 8, 8, 4, 8, 1 / 2
};


int itoneLen;

void setup() {
  itoneLen = sizeof(melody) / sizeof(melody[0]);
  pinMode(LED_BUILTIN, OUTPUT);
  for (int thisNote = 0; thisNote < itoneLen; thisNote++) {

    
    int noteDuration = 1.5 * 1000 / noteDurations[thisNote]; // 全音符音长1.5*1000ms，四分音符则除以4
    
    tone(8, melody[thisNote], noteDuration);  //使用8号引脚

    int pauseBetweenNotes = noteDuration * 1.30;   //响完一个音后的停顿时间
    delay(pauseBetweenNotes);
    noTone(8);

  }
}

void loop() {
  // no need to repeat the melody.
}
```


