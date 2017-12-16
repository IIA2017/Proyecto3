# Proyecto3
Proyecto3

Configuraremos dos tarjetas de acceso y que se encienda un led rojo o verde, en función de si se hace un acceso autorizado o no.
Materiales necesarios:
•	Placa Waspmote
•	Placa de prototipado
•	1x miniUSB Cable
•	Luz led rojo
•	Luz led verde
•	2 resistencias
•	Módulo RFID/NFC
•	Antena RFID
•	3 pegatinas RFID


Se ha realizado el código de la placa Waspmote dividiéndolo principalmente en cuatro partes.
La primera de ella es la inicialización de las variables, las tarjetas y los pines de salida. 
Seguidamente se inicia el código logrando mostrar en pantalla si no se encuentra ninguna tarjeta próxima “Tarjeta no encontrada". En caso de que haya una tarjeta, saltamos al siguiente punto que será una comparación del tipo de tarjeta utilizada. En nuestro caso tenemos tres tarjetas, una que no tiene acceso, otra que si lo tiene y la tarjeta master. En función de haber sido denegado o válido el acceso se encenderán los leds correspondientes.
Finalmente se pedirá insertar tarjeta en pantalla. 




#include <WaspRFID13.h>  

uint8_t state=1;
ATQ ans;
UIdentifier UID;
UIdentifier myCard1 = {0xC9, 0xA7, 0x35, 0xC1}; 
UIdentifier myCard2 = {0x59, 0x4C, 0x36, 0xC1}; 
UIdentifier myCard3 = {0xE9, 0x8F, 0x36, 0xC1};
UIdentifier myCard4 ;
int hits;

//Laura Bretones
//Jose Angel Lopez
//Jose Carballo
//Paola Urquiza
//Vicente Guisado


void setup()
{
  pinMode(6, OUTPUT);
  pinMode(7, OUTPUT);   
  RFID13.ON(SOCKET0);
  USB.print(F("Proyecto 3"));
   
}



void loop()

{

USB.print(F("\r\n ++++++++++++++++++++++++++++++++++++"));
digitalWrite(DIGITAL7, LOW);
digitalWrite(DIGITAL6, LOW);

//////////////////////////
// 1. Inicio
//////////////////////////
  state = RFID13.init(UID, ans);

  if (state == 1)
  {
    USB.print(F("\r\n Tarjeta no encontrada"));
  }  
  else 
  {
    USB.print(F("\r\n UID: "));   
    RFID13.print(UID, 4); 

    if (state == 0)  
    {
/////////////////////////////////////////////////////
// 2. Comparar
/////////////////////////////////////////////////////
      if (RFID13.equalUIDs(myCard1, UID) == 0) 
      { // Si esta habilitada
        USB.print(F("\r\n  Tarjeta identificada (MASTER). Acceso habilitado. "));
        hits++; 

         digitalWrite(DIGITAL7, HIGH);
            
 
      }
      else if (RFID13.equalUIDs(myCard2, UID) == 0) 
      {
        USB.print(F("\r\n  Tarjeta identificada (NORMAL). Acceso habilitado. "));
        hits++; 

         digitalWrite(DIGITAL7, HIGH);

      } 
      else 
      { // No esta habilitada
        USB.print(F("\r\n  ** Tarjeta no válida. Acceso denegado. "));
          digitalWrite(DIGITAL6, HIGH);

  
      {
        }
      }
        delay(1000);
    }
///////////////////////////////////////////////    
/////Final 
///////////////////////////////////////////////
    else 
    { 
      USB.print(F("\r\n  ** Por favor introduzca tarjeta. "));

      digitalWrite(DIGITAL6, HIGH); 
      {      
      }    
    }
  }

  USB.print(F("\r\n  Accesos: ")); 
  USB.println(hits);
  digitalWrite(DIGITAL6, HIGH); 
  delay(100);

}
