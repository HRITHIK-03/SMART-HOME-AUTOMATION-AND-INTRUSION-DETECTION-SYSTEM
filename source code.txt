#include <DFRobot_DHT11.h>
#define pir_sensor 5
#define buzzer 4
#define ir_sensor1 3
#define ir_sensor2 2
#define led1 7
#define led2 8
#define fan1 6
#define temp 10
DFRobot_DHT11 DHT;
int counter=0;
void setup() {
Serial.begin(9600);
Serial.println("lets begin.............................");
Serial.println("Synchronizing...");
Serial.println(".........home automation system....");
pinMode(led1,OUTPUT);
pinMode(led2,OUTPUT);
pinMode(fan1,OUTPUT);
pinMode(temp,INPUT);
pinMode(ir_sensor1,INPUT);
pinMode(ir_sensor2,INPUT);
pinMode(pir_sensor,INPUT);
pinMode(buzzer,OUTPUT);
}
void temp_auto(){ //temperature controll sys
DHT.read(temp);
Serial.print("temp:");
Serial.print(DHT.temperature);
Serial.print(" humi:");
Serial.println(DHT.humidity);
delay(1000);
if(DHT.temperature>26 and counter>0){
digitalWrite(fan1,HIGH);
}
else{
digitalWrite(fan1,LOW);
}}
void alarm(){    //alarm system
int val = digitalRead(pir_sensor);
//Serial.println(val);
if(val==HIGH and counter<=0){
digitalWrite(buzzer,HIGH);                    
Serial.println("breach");
delay(3000);
}
else if(val==LOW){
Serial.println("no breach");
digitalWrite(buzzer,LOW);                                
delay(3000);
}
}
void counter_sys(){  // counter system
int val1=digitalRead(ir_sensor1);
int val2=digitalRead(ir_sensor2);
if(val1==HIGH and val2==LOW){
counter++;
if(counter>=1)
{
digitalWrite(led2,HIGH);
}
Serial.print("person entered:  ");
Serial.println(counter);
digitalWrite(led1,HIGH);
digitalWrite(buzzer,HIGH);
delay(500);
digitalWrite(buzzer,LOW);
delay(500);
digitalWrite(led1,LOW);
delay(1000);
Serial.println();
}
else if(val2==HIGH and val1==LOW){
counter--;
if(counter<=0){
counter=0;
digitalWrite(led2,LOW);
Serial.println("no person");
}
Serial.print("person exit");
if(counter>=0){
Serial.println(counter);
digitalWrite(buzzer,HIGH);
delay(500);
digitalWrite(buzzer,LOW);
Serial.println();}
delay(2000);
}
}
void loop() {
counter_sys();
temp_auto();
alarm();
}