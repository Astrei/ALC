#include <Wire.h>
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x20,16,2);
const int ButtonPins[] = {2,3,4,5,6,7}; //Обозначаем порты кнопок
const int NumButtons= 6; 
const int LimitSwitchPins[] = {8,9,10,11,12,13}; //Обозначаем порты концевых выключателей
const int NumSwitches= 6; 
const int engineStartUpPin = 14; //Порт реле Вверх
const int engineStartDownPin = 15; //Порт реле Вниз
const int SecurityCircuitPin = 1; //Порт цепи безопасности
boolean halt = false; //True, когда лифт остановлен
boolean task =false; //True когда есть вызов на этаж
int Floor = 0; //Этаж
int Button = 0; //Кнопка

void setup()
   { pinMode(engineStartUpPin, OUTPUT);
    pinMode(engineStartDownPin, OUTPUT);
     setupButtons ();
 setupLimitSwitch ();
pinMode(SecurityCircuitPin, INPUT);
  lcd.init();                     // инициализация LCD
  lcd.backlight();                // включаем подсветку
  lcd.clear();                    // очистка дисплея
  lcd.setCursor(1, 0);            // устанавливаем курсор на 1 строку, 0 символ
  lcd.print("Arduino Lift");          // вывод надписи
 lcd.setCursor(0, 1);
 lcd.print("Controller V1.0"); 
  delay (500);
  lcd.clear(); 
  StopLift ();
   }
void loop()
{ if (digitalRead(SecurityCircuitPin == HIGH)) //Если цепь безопасности в порядке, начинаем работу, если нет, то стоп.
{ if (halt == true) { //Если лифт не двигается
  if (LimitSwitchPressDetected ()==true)//то проверяем нажат ли концевой выключатель (на этаже лифт или нет)
          { if (ButtonPressDetected ()==true) { //Если лифт на этаже и нажата кнопка, то запускаем лифт
          task=true;
            executeTask();
           if (Floor!=Button){
          lcd.setCursor(0, 0);           
  lcd.clear();
  lcd.print(
  "Moved to");
  lcd.setCursor(9, 0);          
  lcd.print(Button);
lcd.setCursor(11, 0);          
  lcd.print("Floor");
  delay(200);
}}
else {lcd.setCursor(0, 0);   
lcd.clear();
  lcd.print("Lift on a Floor:");
  lcd.setCursor(0, 1);          
  lcd.print(Floor);
delay(200);}
            }
    else {if (task==false){
   StartLiftAtNearFloor();//если лифт между этажами, то после нажатия любой кнопки, он стартует наближайший нижний этаж
     lcd.clear();
   lcd.setCursor(0, 0);           
  lcd.print("Move to near");
lcd.setCursor(0, 1);
 lcd.print("Floor");}delay(200);}
 }
 else { if (task==true){if (LimitSwitchPressDetected()==true && Floor==Button){StopLift ();
 task=false;
}}
         else {
 if (LimitSwitchPressDetected()==true) {StopLift ();}
}}
 } 
 else {StopLift ();
 
  lcd.setCursor(0, 0);           
  lcd.print("Security Circuit");
  lcd.setCursor(0, 1);          
  lcd.print("broken! Stopped");
  delay(200);
  loop;
}}
void changeDirection () {
      digitalWrite (engineStartUpPin, LOW);
      digitalWrite (engineStartDownPin, HIGH);
      halt = false;
      delay (2000);
}


void executeTask(){ //собственно, определяем направление движения лифта

    if (Floor!=Button)
    {if (Floor<Button){
          StartLift ();}
    else {changeDirection();
          }
          }
          else {StopLift ();}
        }
       
  

void StopLift () {
     digitalWrite (engineStartUpPin,LOW);
      digitalWrite (engineStartDownPin, LOW);
      halt = true;
      delay (2000);
}

void StartLiftAtNearFloor(){
 if (ButtonPressDetected ()==true)  {
 digitalWrite (engineStartDownPin, HIGH);
 halt = false;
 delay (2000);
  }
else {StopLift ();}}

void StartLift(){ //Пуск лифта вверх
 digitalWrite (engineStartDownPin, LOW);
  digitalWrite (engineStartUpPin, HIGH);
    halt = false;
    delay (2000);
}

void setupButtons () //Опрашивает порты кнопок
{
    for (int i = 0 ; i < NumButtons; i++)
        pinMode (ButtonPins[i], INPUT);
}

void setupLimitSwitch () //Опрашивает порты концевиков
{
    for (int i = 0 ; i < NumSwitches; i++)
        pinMode (LimitSwitchPins[i], INPUT);
}

boolean ButtonPressDetected () { //возвращает true, если нажата кнопка
  return (ButtonStates ());
}
boolean LimitSwitchPressDetected () { //возвращает true если нажат концевик
  return (LimitSwitchStates ());
}
boolean ButtonStates () {
      for (int i = 0 ; i < NumButtons; i++) {
         if (digitalRead(ButtonPins[i])== HIGH) {
Button = i+1;
return true;
                  }
           
           }
}
boolean LimitSwitchStates () {
      for (int i = 0 ; i < NumSwitches; i++) {
         if (digitalRead(LimitSwitchPins[i])== HIGH) {
Floor = i+1;
return true;
                  }
               
           
           }
}
