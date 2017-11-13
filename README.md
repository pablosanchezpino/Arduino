# Arduino
Código base tele operación Arduino

/*
 PROYECTO TESIS
 PROGRAMADOR: Pablo Sanchez Pino


Desconectar modulo Bluetooth mientras carga Sketch, luego conectar
ARDUINO  BLUETOOTH
Pin 0 (RX)  TXD
Pin 1 (Tx)  RXD
5V    VCC
GND   GND

Descargar aplicacion Android en Play Store:  ArduDroid 
*/
#define CARACTER_INICIO_CMD '*'
#define CARACTER_FINAL_CMD '#'
#define CARACTER_DIV_CMD '|'
#define ESCRITURA_DIGITAL_CMD 10
#define ESCRITURA_ANALOGA_CMD 11
#define TEXTO_CMD 12
#define LECTURA_ARDUDROID_CMD 13
#define MAX_COMMAND 20  
#define MIN_COMMAND 10 
#define LONGITUD_ENTRADA_STRING 40
#define ESCRITURA_ANALOGICA_MAX 255
#define PIN_ALTO 3
#define PIN_BAJO 2
#include <Servo.h>                // Incluye la libreria Servo
 
Servo servo1;                    // Crea el objeto servo1 con las caracteristicas de Servo



String inText;

void setup() {
  Serial.begin(9600);
  Serial.println("Tele Operacion Arduino");
  Serial.flush();
  servo1.attach(7,600,1500);
}

void loop()
{
  Serial.flush();
  int ard_command = 0;
  int pin_num = 0;
  int pin_value = 0;

  char get_char = ' ';  //lee serial

  // esperar a que los datos entren
  if (Serial.available() < 1) return; // si no hay datos en el serial retornar al Loop().

  // analizar entrada de indicador de inicio de comando
  get_char = Serial.read();
  if (get_char != CARACTER_INICIO_CMD) return; // si no hay indicaci�n de inicio del sistema, volver  loop ().
  // parse incoming command type
  ard_command = Serial.parseInt(); // read the command
  
  // analizar el tipo de comando entrante
  pin_num = Serial.parseInt(); // leer el pin
  pin_value = Serial.parseInt();  // leer el valor

  // 1) OBTENER COMANDO DE TEXTO PARA ARDUDROID
  if (ard_command == TEXTO_CMD){   
    inText =""; // borra variable para nueva entrada
    while (Serial.available())  {
      char c = Serial.read();  // recibe un byte de la memoria intermedia serie
      delay(5);
      if (c == CARACTER_FINAL_CMD) { // si la cadena completa ha sido leida
        // add your code here
        break;
      }              
      else {
        if (c !=  CARACTER_DIV_CMD) {
          inText += c; 
          delay(5);
        }
      }
    }
  }

  // 2) OBTENER DATOS DE digitalWrite ARDUDROID
  if (ard_command == ESCRITURA_DIGITAL_CMD){  
    if (pin_value == PIN_BAJO) pin_value = LOW;
    else if (pin_value == PIN_ALTO) pin_value = HIGH;
    else return; // error en el valor de PIN. regresar.
    set_digitalwrite( pin_num,  pin_value);  // Eliminar el comentario de esta funci�n para utilizarla
    return;  // regrese al inicio de loop()
  }

  // 3) GET analogWrite DATA FROM ARDUDROID
  if (ard_command == ESCRITURA_ANALOGA_CMD) {  
    analogWrite(  pin_num, pin_value ); 
    // add your code here
    return;  // Done. return to loop();
  }

  // 4) Enviar datos a ARDUDROID
  if (ard_command == LECTURA_ARDUDROID_CMD) { 
    // char send_to_android[] = " Coloca el texto aqu�." ;
    // Serial.println(send_to_android);   // Ejemplo: Env�o de texto
    Serial.print(" Analogo 0 = "); 
    Serial.println(analogRead(A0));  // Ejemplo: Leer y enviar valor anal�gico del Pin de Arduino
    return;  // Listoe. regrese al loop();
  }
}

// 2a) seleccionar el pin # solicitado para la acci�n digitalWrite
void set_digitalwrite(int pin_num, int pin_value)
{
  switch (pin_num) {
  case 13:
    pinMode(13, OUTPUT);
    digitalWrite(13, pin_value);  
    // adicione su c�digo aqu�, para este n�mero de pin del Arduino
    break;
  case 12:
    pinMode(12, OUTPUT);
    digitalWrite(12, pin_value);   
    // adicione su c�digo aqu�, para este n�mero de pin del Arduino
    break;
  case 11:
    pinMode(11, OUTPUT);
    digitalWrite(11, pin_value);         
    // adicione su c�digo aqu�, para este n�mero de pin del Arduino
    break;
  case 10:
    pinMode(10, OUTPUT);
    digitalWrite(10, pin_value);         
    // adicione su c�digo aqu�, para este n�mero de pin del Arduino
    break;
  case 9:
    pinMode(9, OUTPUT);
    digitalWrite(9, pin_value);         
    // adicione su c�digo aqu�, para este n�mero de pin del Arduino
    break;
  case 8:
    pinMode(8, OUTPUT);
    digitalWrite(8, pin_value);         
    // adicione su c�digo aqu�, para este n�mero de pin del Arduino
    break;
  case 7:
    servo1.write(0);                // Gira el servo a 0 grados  
    delay(2000);                     // Espera 700 mili segundos a que el servo llegue a la posicion
   
    servo1.write(90);               // Gira el servo a 90 grados  
    delay(2000);  
                      
    servo1.write(180);             //Gira el servo a 180 grados 
    delay(2000); 
  case 6:
    servo1.write(0);                // Gira el servo a 0 grados  
    delay(2000);                     // Espera 700 mili segundos a que el servo llegue a la posicion
   
    servo1.write(90);               // Gira el servo a 90 grados  
    delay(2000);  
                      
    servo1.write(180);             //Gira el servo a 180 grados 
    delay(2000);
    break;
  case 5:
    servo1.write(0);                // Gira el servo a 0 grados  
    delay(2000);                     // Espera 700 mili segundos a que el servo llegue a la posicion
   
    servo1.write(90);               // Gira el servo a 90 grados  
    delay(2000);  
                      
    servo1.write(180);             //Gira el servo a 180 grados 
    delay(2000);
    break;
  case 4:
    pinMode(4, OUTPUT);
    digitalWrite(4, pin_value);         
    // adicione su c�digo aqu�, para este n�mero de pin del Arduino
    break;
  case 3:
    pinMode(3, OUTPUT);
    digitalWrite(3, pin_value);         
    // adicione su c�digo aqu�, para este n�mero de pin del Arduino
    break;
  case 2:
    pinMode(2, OUTPUT);
    digitalWrite(2, pin_value); 
    // adicione su c�digo aqu�, para este n�mero de pin del Arduino
    break;      
    // por defecto
    // si nada m�s fue seleccionado, hacer el defecto (default)
    // default es opcional
  } 
}
