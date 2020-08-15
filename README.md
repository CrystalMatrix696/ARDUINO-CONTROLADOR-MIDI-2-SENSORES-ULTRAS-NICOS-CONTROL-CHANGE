# ARDUINO-CONTROLADOR-MIDI-2-SENSORES-ULTRAS-NICOS-CONTROL-CHANGE
ARDUINO CONTROLADOR MIDI CON DOS SENSORES ULTRASÓNICOS, CONTROL CHANGE.




//--------------------------------------------1. ULTRASONICO 1 Y 2, ABLETON CONTROL CHANGE INICIO------------------------------------------
//Código hecho por Daniel Marcial (@danielmarcial22)

//Este código también funciona para controlar con la mano mientras el ultrasónico permanece recostado
const int aMin = 5; //altura mínima a mapear
const int aMax = 50; //altura máxima a mapear
float distancia,distanciaAnterior;
long tiempo;
int valor;

//--------------------------------------------1. ULTRASONICO 1 Y 2, ABLETON CONTROL CHANGE FINAL------------------------------------------

void setup () {

  
  Serial.begin(31250); // HIDUINO; FINAL, OJO QUITAR DIAGONALES PARA HABILITAR, ESTA OPCION O LA DE ABAJO.
  //Serial.begin(57600); // PRIMERA PRUEBA O TEST CON MONITOR SERIE, UNICAMENTE POTENCIOMENTROS.
//Serial.begin(112500); // SEGUNDA PRUEBA POTENCIOMETROS, TERCERO PRUEBA PARA ULTRASONICOS.

 pinMode(6, OUTPUT); // trigger, aqui arduino envia un pulso al sensor, para que inicie la medicion
 pinMode(7, INPUT);  // echo, con este arduino recibe el pulso cuyo tiempo representa
                       //la duracion del viaje del sonido en el aire.
 pinMode(5, OUTPUT); // trigger, aqui arduino envia un pulso al sensor, para que inicie la medicion
 pinMode(4, INPUT);  // echo, con este arduino recibe el pulso cuyo tiempo representa
                       //la duracion del viaje del sonido en el aire
}


void loop () {
loopultra1 ();
loopultra2 (); 
}



void loopultra1 () {


//--------------------------------------------1. ULTRASONICO 1, ABLETON CONTROL CHANGE, VOID LOOP, INICIO------------------------------------------
    
      digitalWrite(6,LOW); 
      delayMicroseconds(5);
      digitalWrite(6, HIGH); //envio del pulso para iniciar medicion
      delayMicroseconds(10); //arduino espera 10 micro segundos
      tiempo=pulseIn(7, HIGH);  //se mide la duración del pulso
      distancia= 0.0177*tiempo; //multiplicamos el tiempo para convertirlo en dista
    
      if (abs(distanciaAnterior-distancia)>0.8) //resolución de 8 mm.
      {
        if (distancia<=aMax+2)
        {
        valor = map(distancia,aMin,aMax,0,127); //mapear alturas mínima y máxima a valores de 0 a 127  
        if (valor>127) valor=127; //limiter
        if (valor<0) valor=0; //limiter para los negativos
    Serial.write(177);//Control Change canal 1 =176
    Serial.write(30); // CC 30
    Serial.write(valor); //CC value
    }
    distanciaAnterior = distancia;
  }

  delay(5);
  
}

  //--------------------------------------------1. ULTRASONICO 1, ABLETON CONTROL CHANGE, VOID LOOP, FINAL------------------------------------------/*

void loopultra2 () {


//--------------------------------------------2. ULTRASONICO 2, ABLETON CONTROL CHANGE, VOID LOOP, INICIO------------------------------------------
    
      digitalWrite(5,LOW); //TRIGGER
      delayMicroseconds(5);
      digitalWrite(5, HIGH); //envio del pulso para iniciar medicion //TRIGGER
      delayMicroseconds(10); //arduino espera 10 micro segundos
      tiempo=pulseIn(4, HIGH);  //se mide la duración del pulso //ECHO
      distancia= 0.0177*tiempo; //multiplicamos el tiempo para convertirlo en dista
    
      if (abs(distanciaAnterior-distancia)>0.8) //resolución de 8 mm.
      {
        if (distancia<=aMax+2)
        {
        valor = map(distancia,aMin,aMax,0,127); //mapear alturas mínima y máxima a valores de 0 a 127  
        if (valor>127) valor=127; //limiter
        if (valor<0) valor=0; //limiter para los negativos
    Serial.write(177);//Control Change canal 1 =176
    Serial.write(31); // CC 30
    Serial.write(valor); //CC value
    }
    distanciaAnterior = distancia;
  }

  delay(5);
  
}

  //--------------------------------------------2. ULTRASONICO 2, ABLETON CONTROL CHANGE, VOID LOOP, FINAL------------------------------------------/*

 
