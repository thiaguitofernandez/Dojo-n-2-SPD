# Ejercicio Dojo 2 SPD

## Integrantes 
- Benjamin Andres Aguilera
- Pablo Nicolas Aguilar
- Thiago Patricio Fernandez Lado
- Josue Damacio



## Proyecto: Semaforo
![Tinkercad](./img/)


## Descripción
El objetivo de este 

## Función principal
definir puertos y variables
~~~
#define	ARRIBA_DERECHA 13
#define ARRIBA 12
#define ARRIBA_IZQUIERDA 11
#define CENTRO 10
#define ABAJO_IZQUIERDA 9
#define ABAJO 8
#define ABAJO_DERECHA 7
#define estacion_final 6
#define estacion_tercera 5
#define estacion_segunda 4
#define estacion_inicial 3
#define buzzer 14
#define pulsador 15

int i = 4;
int encendido = LOW;

void setup()
{
  pinMode(ARRIBA_DERECHA, OUTPUT);
  pinMode(ARRIBA, OUTPUT);
  pinMode(ARRIBA_IZQUIERDA, OUTPUT);
  pinMode(CENTRO, OUTPUT);
  pinMode(ABAJO_IZQUIERDA, OUTPUT);
  pinMode(ABAJO, OUTPUT);
  pinMode(ABAJO_DERECHA, OUTPUT);
  pinMode(estacion_final,OUTPUT);
  pinMode(estacion_tercera,OUTPUT);
  pinMode(estacion_segunda,OUTPUT);
  pinMode(estacion_inicial,OUTPUT);
  pinMode(buzzer,OUTPUT);
  pinMode(pulsador,INPUT);//pulldown externo
  Serial.begin(9600);
}
~~~

Aqui se definen los LEDs como los segmentos del display segun su ubicacion y se les agregan un puerto de asignacion utilizando 2 puertos de salidas analogicas como salidas digitales como los puertos 14 y 15 y se abre el monitor en serie
~~~
void loop()
{	
    if (digitalRead(pulsador) == HIGH){
  	    encendido = digitalRead(pulsador);
    }
    if (encendido == HIGH ){
        i--;
        Serial.print("llegando a la estacion: ");
  	    eleccion_de_numero(i);
	      if(i < 0){
            i = 4;
        }
    }
}
~~~
Dentro del loop tenemos 2 if los cuales sirven para condicionar cuando comienza a hacerse el recorido. El primer if se encarga de leer el pulsador y si este es HIGH guarda este valor en la varible encendido. El segundo if se encarga de decidir si comienza el recorrido o no y solo lo hara una vez se halla cumplido almenos una vez el if anterior el cual habilita la bandere condicion del segundo if.
Para el recorrido se utiliza una funcion llamada eleccion de numero el cual recibe "i"  mediante parametro para poder funcionar. "i" es una variable con valores numeris la cual se inicializa en 4 y se la resta 1 cada vez que se repite el loop y cuando esta llega a 0 se le resetea su valorr devuelta a 4.

~~~
void eleccion_de_numero(int i){
    switch (i)
    {
    case 0:
        Prende_numero_cero();
        prende_led(estacion_final);
        Serial.println("estacion final");
        sonido(1500,500,500,1000);
        apaga_led(estacion_final);
        Apaga_numero_cero();
        break;
    case 1:
        Prende_numero_uno();
        prende_led(estacion_tercera);
        Serial.println("estacion cuarta");
        sonido(1500,400,500,1000);
        apaga_led(estacion_tercera);
        break;
    case 2:
        Prende_numero_dos();
        prende_led(estacion_segunda);
        Serial.println("estacion tercera");
        sonido(1500,300,500,1000);
        apaga_led(estacion_segunda);
        Apaga_numero_dos();
        break;
    case 3:
        Prende_numero_tres();
        prende_led(estacion_inicial);
        Serial.println("estacion inicial");
        sonido(1500,200,500,1000);
        apaga_led(estacion_inicial);
        Apaga_numero_tres();
        break;
    }
}
~~~
la funcion de "eleccion de numero" funciona recibiendo un valor numerico por parametro el cual luego se evalua dentro de un switch para los numeros 0, 1, 2 y 3 utilizandolos para medir la cantidad de estaciones de distancia para llegar a destino 
~~~
void sonido(int tiempo, int potencia, int suena, int espera){
  int contador = 0;
  while (contador != (tiempo / (suena+espera)) ){
  		tone(buzzer, potencia, suena);
  		delay(espera);
  		contador++;
  }
  contador = 0;
}
~~~
sonido es una funcion la cual se encagar de manejar el buzzer la cual lo hara sonar la cantidad de veces necesarias hasta que el parametro tiempo halla ocurido. Para saber esto se recibe el timpo por el cual el sonido debe reproducirse cada vez y el tiempo que debe esperar para volver a sonar utilizando un contador que empieza en 0 el cual se le añade 1 cada vez que suena el buzzer hasta que esta formula sea verdadera (contador = (tiempo / (suena+espera)
~~~
void Prende_numero_cero(){
  	prende_led(ARRIBA);
    prende_led(ARRIBA_DERECHA);
    prende_led(ABAJO_DERECHA);
    prende_led(ABAJO);
    prende_led(ABAJO_IZQUIERDA);
    prende_led(ARRIBA_IZQUIERDA);
}
void Apaga_numero_cero(){
  	apaga_led(ARRIBA);
  	apaga_led(ARRIBA_DERECHA);
  	apaga_led(ABAJO_DERECHA);
  	apaga_led(ABAJO);
  	apaga_led(ABAJO_IZQUIERDA);
  	apaga_led(ARRIBA_IZQUIERDA);
}

void Prende_numero_uno(){
    prende_led(ABAJO_DERECHA); 
}

void Prende_numero_dos(){
    prende_led(ABAJO_IZQUIERDA);
}
void Apaga_numero_dos(){
    apaga_led(ARRIBA);
    apaga_led(CENTRO);
    apaga_led(ABAJO_IZQUIERDA);
    apaga_led(ABAJO); 
}

void Prende_numero_tres(){
    prende_led(ARRIBA);
    prende_led(ARRIBA_DERECHA);
    prende_led(CENTRO);
    prende_led(ABAJO_DERECHA);
    prende_led(ABAJO);
 
}
~~~
cada una de estas funciones prende los segmentos necesarios para que se forme el numero deseado siemprer y cuando sean usadas desde mayor a menor ya que si un led no se encuentra encendido este no se encendera
~~~
void prendeLed(int led, int  tiempo){  	
	digitalWrite(led, HIGH);//encender LED
  	delay(tiempo); // esperar por tiempo
  }

void apagaLed(int led,int tiempo){
  	digitalWrite(led, LOW);//endender LED
    delay(tiempo); // esperar por tiempo
}
~~~
Con estas dos funciones los led se prenden y se apagan.Estas ocurren en diferentes funciones para habilitar hacer cosas mientras una led se encuenta prendida.



pd:recorrido de vuelta a implementar
~~~


## Link al proyecto :eggplant:
- [proyecto](https://www.tinkercad.com/things/lRzCahleti8)




