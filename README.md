# Color-detection-arduino-project


![image](https://user-images.githubusercontent.com/19898602/139021422-f3b62a3f-b59a-4a30-8216-7670fb8a5d49.png)


Hello, in this tutorial we’ll be using the TCS3200, TCS230 or GY-31, color sensor module with Arduino UNO board, 

and there will be project using a RGB LED to reproduce detected colors.

The module has an 8×8 photodiode array, 16 of them with Red filter, 16 with blue one, 16 with green one and 16 without a filter (clear), 

we select what filter to use and read its value, and in the code we combine them depending on the applciation or project.

The light is detected by the photodiodes and the output is a frequency proportional to the current flowing through the photodiodes

which is related to the filter used and the object’s color detected.

![image](https://user-images.githubusercontent.com/19898602/139021531-fbdaf7f3-2e00-4d87-9fdc-fe366b4292b0.png)


-The module has (Vcc/GND) pins, they are redundant.

– S0/S1 pins control the output frequency scalling

– S2/S3 pins control which set of photodiodes we gonna measure (Red/Green/Blue/Clear)

– Out is the output signal and LED pin controls the LEDs on the front.



![image](https://user-images.githubusercontent.com/19898602/139021596-15b27d47-61f6-4e3d-97aa-9e689adaf985.png)


This option permits the module to be used with different measuring techniques, and types of microcontrollers, 

in the tutorial and codes I kept using it at 100%, you can change it if you want it depends just on the logic level of their pins (HIGH/LOW).




![image](https://user-images.githubusercontent.com/19898602/139021677-6332c403-0cda-4bbd-8e04-3bcaf40b8e7b.png)

By also controlling the logic levels of S2/S3 we can select which filter or no filter to use, in the code I go through Red/Green/Blue, 

if your application require one or two filters only you can do it too.

For the LED pin, if it’s not connected, the LEDs will light up automatically, but if you want to turn them off you can put the LED pin to LOW, 

(it depends on your application and condition).

For the Out pin, as the signal given by the sensor is a frequency,   we measure the duration as they are related (Duration=1/Frequency), 

so the higher the frequency of a color, the lower is the duration measured, which means that the object detected has that color (check the tutorial).


# Wirings

![image](https://user-images.githubusercontent.com/19898602/139022123-dba22b23-6401-463e-b9da-c0bf0810d502.png)


![image](https://user-images.githubusercontent.com/19898602/139021856-bf9db39f-80bb-441d-9f56-0f1ba1f808fe.png)


# Codes

Codes are below too

Code 1: Is just direct wiring and reading the output signal for Red/Blue/Green, you can use it to calibrate your module, for example place some object and check the sensor values, because those values are related to the lighting conditions, exposure, and the object itself.

Code 2: Now we have an idea about the values for each color, we can start identifying them, the easy method is that during first tests you’ll notice that the color of the object has the lowest value, which is a duration, (higher frequency), check tests below.

.Code 3: Here we are using the RGB LED, and we try to reproduce the values of the colors given by the sensor, and most of the time it reproduce the color of the object detected, here what you should know: the min/max value given by the sensor for each color, and that it’s inverted (the lowest the value, the higher is the color).

analogWrite(LED_R,map(Red,15,60,255,0));
This is the function we used, so we generate a PWM signal with « analogWrite », on the pin (LED_R) which is the Red pin for the RGB LED connected to pin D3, 

the value we write is proportional to « Red » value which is the Red value given by the sensor, and it has 15 as minimum and 60 as maximum (those are my proper values, try measuring your own), and this value is converted to 255-0, 

so the 15 will be « equal » to 255 and 60 will be « equal » to 0For example if the module gives us « 15 », the LED_R will receive « 255 » value which is the highest (5V), because as said the lower the value the higher is the color)…


```javascript
/* This code works with GY-31 TCS3200 TCS230 color sensor module
 * It select a photodiode set and read its value (Red Set/Blue set/Green set) and displays it on the Serial monitor
 * Refer to www.surtrtech.com for more details
 */

#define s0 8       //Module pins wiring
#define s1 9
#define s2 10
#define s3 11
#define out 12

int data=0;        //This is where we're going to stock our values

void setup() 
{
   pinMode(s0,OUTPUT);    //pin modes
   pinMode(s1,OUTPUT);
   pinMode(s2,OUTPUT);
   pinMode(s3,OUTPUT);
   pinMode(out,INPUT);

   Serial.begin(9600);   //intialize the serial monitor baud rate
   
   digitalWrite(s0,HIGH); //Putting S0/S1 on HIGH/HIGH levels means the output frequency scalling is at 100% (recommended)
   digitalWrite(s1,HIGH); //LOW/LOW is off HIGH/LOW is 20% and LOW/HIGH is  2%
   
}

void loop()                  //Every 2s we select a photodiodes set and read its data
{

   digitalWrite(s2,LOW);        //S2/S3 levels define which set of photodiodes we are using LOW/LOW is for RED LOW/HIGH is for Blue and HIGH/HIGH is for green
   digitalWrite(s3,LOW);
   Serial.print("Red value= "); 
   GetData();                   //Executing GetData function to get the value

   digitalWrite(s2,LOW);
   digitalWrite(s3,HIGH);
   Serial.print("Blue value= ");
   GetData();

   digitalWrite(s2,HIGH);
   digitalWrite(s3,HIGH);
   Serial.print("Green value= ");
   GetData();

   Serial.println();

   delay(2000);
}

void GetData(){
   data=pulseIn(out,LOW);       //here we wait until "out" go LOW, we start measuring the duration and stops when "out" is HIGH again
   Serial.print(data);          //it's a time duration measured, which is related to frequency as the sensor gives a frequency depending on the color
   Serial.print("\t");          //The higher the frequency the lower the duration
   delay(20);
}
```
At last I would like to tell you something about custom PCB

Yes PCB are the heart of the electronics based project usually we hesitate to try custom PCB and opt to homemade solutions

like breadboard or Zero PCB earlier I also was in the same boat, I hesitate to try custom PCB my belief was they are much expensive.

but then I came to know about [JLCPCB.COM](https://jlcpcb.com/IAT) and I was totally surprised how low price PCB's are they offering 

there PCB quality is best in market, now I always go with PCB for my project and [JLCPCB.COM](https://jlcpcb.com/IAT) is my trusted 

If new user signup now using this link [JLCPCB.COM](https://jlcpcb.com/IAT) you will get coupons worth of 27$ from [JLCPCB.COM](https://jlcpcb.com/IAT)

you can also try there PCB service with any doubt for more details you can visit their website [JLCPCB.COM](https://jlcpcb.com/IAT)
You can also try there new purple colour for PCB without any extra cost.
![image](https://user-images.githubusercontent.com/19898602/134336832-cb9953e9-02a6-4ff7-9d27-2caad10fe7c7.png)
![image](https://user-images.githubusercontent.com/19898602/130722577-c30b7b43-ea89-4847-9c6b-058f9fabeda3.png)![image](https://user-images.githubusercontent.com/19898602/130722585-b5268db1-5f17-428f-ba60-b823140f2a70.png)



![How to make Color Sorting Machine Arduino Based](https://user-images.githubusercontent.com/19898602/139022951-776fa074-e8d1-4dd9-a8a2-3fdadcc0a71c.gif)


