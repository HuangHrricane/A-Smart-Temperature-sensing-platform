# A-Smart-Temperature-sensing-platform
#include "U8glib.h"

#include <Wire.h>

#include "I2Cdev.h"

#include <SHT2x.h>

U8GLIB_SSD1306_128X64 u8g(U8G_I2C_OPT_NONE);

float tem;

int i=0;

void draw(void) {

u8g.setFont(u8g_font_unifont);

u8g.setPrintPos(0,22);

u8g.print("tem:");

u8g.print(tem);/*这里是打印温度值*/

if(tem<20)

{ u8g.setPrintPos(i,57);

u8g.print("It is a little cold");/*这里是提示*/

}

if(tem>=20&&tem<30)

{ u8g.setPrintPos(i,57);

u8g.print("you need add some hot water");

}

if(tem>=30&&tem<40)

{u8g.setPrintPos(i,45);

u8g.print("comfortable for drinking ");

}/*这里是提示*/

if(tem>=40&&tem<50)

{u8g.setPrintPos(i,57);

u8g.print("the temperrory is getting by");

}

if(tem>50)

{u8g.setPrintPos(i,45);

u8g.print(" a little hot, isn't it?");

}

if(tem>=50&&tem<70)

{u8g.setPrintPos(i,57);

u8g.print("you'd better not");

}

if(tem>=70)

{u8g.setPrintPos(i,57);

u8g.print("Dangerous");

}

}

void setup()

{

Serial.begin(9600);

if ( u8g.getMode() == U8G_MODE_R3G3B2 )

u8g.setColorIndex(255);     // white字体颜色

else if ( u8g.getMode() == U8G_MODE_GRAY2BIT )

u8g.setColorIndex(3);         // max intensity

else if ( u8g.getMode() == U8G_MODE_BW )

u8g.setColorIndex(1);         // pixel on

}

void loop(void) {

tem = SHT2x.GetTemperature();

//u8g.setFont(u8g_font_7x13);/*函数用于设置字体，没有默认字体，必须定义;*/

//u8g.setPrintPos(0,22);/*函数用于指定字符的坐标位置，X表示水平位坐标，Y表示纵向坐标，最上一行的Y值不能为0，值要大于显示字体的高度；*/

//u8g.print(tem);/*函数用于显示字符，静态不动的字符要加””,变化数据则不需要;*/

u8g.firstPage(); 

do {

draw();

} while( u8g.nextPage() );

Serial.println(tem);

delay(5);

i=i-10;

}
