#include<FirebaseESP32.h>
    Serial.print('/');
    Serial.print('/');
    Serial.print(tiempoActual.dia);
    Serial.print(tiempoActual.dia);
    Serial.print(' ');
    Serial.print(' ');
    Serial.print(tiempoActual.hora);
    Serial.print(tiempoActual.hora);
    Serial.print(':');
    Serial.print(':');
    Serial.print(tiempoActual.minuto);
    Serial.print(tiempoActual.minuto);
    Serial.print(':');
    Serial.print(':');
    Serial.print(tiempoActual.segundo);
    Serial.print(tiempoActual.segundo);
    Serial.print(' ');
    Serial.print(' ');
    Serial.print(DiaSemana[tiempoActual.diaSemana]);
    Serial.print(DiaSemana[tiempoActual.diaSemana]);
    Serial.println();
    Serial.println();
    
    
   delay(1000); // Se actualiza cada segundo
   delay(1000); // Se actualiza cada segundo


   if (LDR <=600){
   if (LDR <=600){
    panel.write(pos_default);
    panel.write(pos_default);
}
}
if(tiempoActual.minuto>=2&& tiempoActual.minuto<12){
if(tiempoActual.minuto>=2&& tiempoActual.minuto<12){
  act_1=true;
  act_1=true;
 Serial.print("Estoy en if de 2 a 12");
 Serial.print("Estoy en if de 2 a 12");
}
}


  
  
if ( act_1==true){
if ( act_1==true){
       
       
         timeR=millis();
         timeR=millis();
        if(timeR-t > tiempo){
        if(timeR-t > tiempo){
         t=timeR;
         t=timeR;
         pos_default_add++;
         pos_default_add++;
          }
          }
        if(LDR > 800){
        if(LDR > 800){
       panel.write (pos_default_add);
       panel.write (pos_default_add);
       Serial.print("GRADOS SERVO");
       Serial.print("GRADOS SERVO");
       Serial.println(pos_default_add);
       Serial.println(pos_default_add);
        }
        }
}
}


if(pos_default_add >=150){
if(pos_default_add >=150){
  act_1=false;
  act_1=false;
}
}
if(pos_default_add >=150 && tiempoActual.minuto>=12){       
if(pos_default_add >=150 && tiempoActual.minuto>=12){       
         pos_default_add =35;
         pos_default_add =35;
         panel.write (pos_default);
         panel.write (pos_default);
         Serial.print("estoy en tiempo mayor a 12");
         Serial.print("estoy en tiempo mayor a 12");
         
         
}
}
 }
 }
float get_voltage(int n_muestras)
float get_voltage(int n_muestras)
{
{
  float voltage=0;
  float voltage=0;
  
  
  for(int i=0;i<n_muestras;i++)
  for(int i=0;i<n_muestras;i++)
  {
  {
    voltage =voltage+analogRead(A0) * (5.0 / 1023.0);    
    voltage =voltage+analogRead(A0) * (5.0 / 1023.0);    
  }
  }
  voltage=voltage/n_muestras;
  voltage=voltage/n_muestras;
  return(voltage);
  return(voltage);
}
}


 float sensarCorriente(){
 float sensarCorriente(){
  float voltajeSensor= analogRead(4)*(5.0 / 1023.0); //lectura del sensor   
  float voltajeSensor= analogRead(4)*(5.0 / 1023.0); //lectura del sensor   
  Serial.println(voltajeSensor);
  Serial.println(voltajeSensor);
  float I=(voltajeSensor-2.5)/Sensibilidad; //Ecuación  para obtener la corriente
  float I=(voltajeSensor-2.5)/Sensibilidad; //Ecuación  para obtener la corriente
  return I;
  return I;
 }
 }


 void controlFoco1(){
 void controlFoco1(){
  String foco1 = conseguirEstado1();
  String foco1 = conseguirEstado1();
   Serial.println(foco1);
   Serial.println(foco1);
  if(foco1 == "ON"){
  if(foco1 == "ON"){
    Serial.println("entre");
    Serial.println("entre");
  digitalWrite(FOCO1.PINFOCO,HIGH);
  digitalWrite(FOCO1.PINFOCO,HIGH);
}
}
else if(foco1 == "OFF"){
else if(foco1 == "OFF"){
  digitalWrite(FOCO1.PINFOCO,LOW);
  digitalWrite(FOCO1.PINFOCO,LOW);
}
}
 }
 }




void controlFoco2(){
void controlFoco2(){
 
 
   // FOCO2.estado=conseguirEstado1();
   // FOCO2.estado=conseguirEstado1();
  String foco2 = conseguirEstado2();
  String foco2 = conseguirEstado2();
   Serial.println(foco2);
   Serial.println(foco2);
  if(foco2 == "ON"){
  if(foco2 == "ON"){
  digitalWrite(FOCO2.PINFOCO,HIGH);
  digitalWrite(FOCO2.PINFOCO,HIGH);
}
}
else if(foco2 == "OFF"){
else if(foco2 == "OFF"){
  digitalWrite(FOCO2.PINFOCO,LOW);
  digitalWrite(FOCO2.PINFOCO,LOW);
}
}
 }
 }


boolean validarBateria(float voltaje){
boolean validarBateria(float voltaje){
  if(voltaje<voltajeTotalBateria*0.25){
  if(voltaje<voltajeTotalBateria*0.25){
    digitalWrite(27,HIGH);
    digitalWrite(27,HIGH);
  }
  }
  else{
  else{
    digitalWrite(27,LOW);
    digitalWrite(27,LOW);
  }
  }
}
}


 float get_corriente(int n_muestras)
 float get_corriente(int n_muestras)
{
{
  float voltajeSensor;
  float voltajeSensor;
  float corriente=0;
  float corriente=0;
  for(int i=0;i<n_muestras;i++)
  for(int i=0;i<n_muestras;i++)
  {
  {
    voltajeSensor = analogRead(4) * (5 / 4095.0);////lectura del sensor
    voltajeSensor = analogRead(4) * (5 / 4095.0);////lectura del sensor
    corriente=corriente+(voltajeSensor-2.5)/Sensibilidad; //Ecuación  para obtener la corriente
    corriente=corriente+(voltajeSensor-2.5)/Sensibilidad; //Ecuación  para obtener la corriente
  }
  }
  corriente=corriente/n_muestras;
  corriente=corriente/n_muestras;
  return(corriente);
  return(corriente);
}
}
 
 
void loop()
void loop()
{
{


  controlFoco1();
  controlFoco1();
    controlFoco2(); 
    controlFoco2(); 
 controlServo();//
 controlServo();//
   
   
}
}
