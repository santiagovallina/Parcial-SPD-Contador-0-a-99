# **Primer parcial SPD**
![](https://canaltic.com/blog/wp-content/uploads/2020/08/pc17.jpg)


------------
## **Integrantes**
#### -  Rodrigo Uribarri
#### -  Valentin Trivelli
#### -  Santiago Vallina

------------
## **Proyecto : Contador de 0 a 99 con Display 7 Segmentos y Multiplexación, parte 1 y 2**
![](https://scontent.feze10-1.fna.fbcdn.net/v/t39.30808-6/393693468_7152841641392974_6216531908837219452_n.jpg?_nc_cat=103&ccb=1-7&_nc_sid=5f2048&_nc_eui2=AeG6eSEJMNbXj7veTo5JSFwwGs0sG8faoawazSwbx9qhrFp374L-ZxEdrk21MyfzkdCLlkPoMcV8JAG4OccJ8oFH&_nc_ohc=mBTeJdXf9RcAX9zMShl&_nc_ht=scontent.feze10-1.fna&oh=00_AfBJXVkBc_e-CaPiVKJy0ZcYpUCneDi1Nm--hQqLTcjB5g&oe=6537B3BD)

------------

## **Descripción**
Es un contador de 0 a 99 que utiliza dos displays de 7 segmentos, dos botones para
controlar la cuenta y un interruptor deslizante (switch) de dos posiciones. El contador comienza en 0 y es capaz de aumentar o disminuir
su valor en una unidad con los botones.
Dependiendo de la posición del interruptor, el display debe mostrar o bien el contador o los números primos en el rango de 0 a 99.



------------

## **Funciones**

#### Defino los pines 
```cpp
#define A 7
#define B 8
#define C 9
#define D 10
#define E 11
#define F 12
#define G 13
#define SUBE 2
#define BAJA 3
#define SW 4
#define DECENA A5
#define UNIDAD A4
#define APAGADOS 0

#define POSITIVO 5
#define NEGATIVO 6

#define SENSOR A0
```

#### Void setup
```cpp
void setup() 
{
  // Configuro los pines
  pinMode(SUBE, INPUT_PULLUP);
  pinMode(BAJA, INPUT_PULLUP); // PULLUP -- SI NO APRETO BOTON LEE 1, SINO 0
  pinMode(SW, INPUT);
  pinMode(A, OUTPUT);
  pinMode(B, OUTPUT);
  pinMode(C, OUTPUT);
  pinMode(D, OUTPUT);
  pinMode(E, OUTPUT);
  pinMode(F, OUTPUT);
  pinMode(G, OUTPUT);
  pinMode(UNIDAD, OUTPUT);
  pinMode(DECENA, OUTPUT);
  
  pinMode(5, OUTPUT);
  pinMode(6, OUTPUT);
  
  pinMode(SENSOR, INPUT);
  // Apagago los displays al inicio
  digitalWrite(UNIDAD, LOW);
  digitalWrite(DECENA, LOW);

  // Muestro el dígito 0 al inicio
  printDigit(0);
  Serial.begin(9600);
}


int contadorDigital = 0; //almacena el valor actual del contador de numeros
int SW_antes = 1; // estado anterior del interruptor
bool mostrarPrimos = false; // alterna entre num primos
int PulsadorSubeAntes = 1;
int PulsadorBajaAntes = 1;

float lecturaSensor = 0;
```

#### Void loop
```cpp
/*
loop verifica el estado del interruptor SW para alternar entre
numeros primos y operar con los botones
*/
void loop()
{
  //funcion map para leer los valores del sensor
  lecturaSensor= map(analogRead(SENSOR),0,1023,-50,450);
  Serial.println(lecturaSensor);
  
  
 int SW_ahora = digitalRead(SW);
  
    if (SW_ahora == 0 && SW_ahora != SW_antes)
    {
        mostrarPrimos = !mostrarPrimos; // Alternar entre mostrar primos y operación con botones
        SW_antes = SW_ahora;
   	    if (mostrarPrimos)
   	    {
    	    /*
		    si mostrar primos es verdadero llama a la funcion mostrarNumeroPrimos
		    si es falso pasa a los pulsadores 
		    muestro el valor con printContador
		    */
    	    mostrarNumerosPrimos();
  	    }
    } 
    else 
    {
        int botonPresionado = keypressed();
    
        if (botonPresionado == SUBE) 
        {
            contadorDigital++;
            if (contadorDigital > 99) 
            {
                contadorDigital = 0;   
            }
          digitalWrite(6, 1); // si sube motor gira en un sentido
          digitalWrite(5, 0);
        } 
        else if (botonPresionado == BAJA) 
        {
            contadorDigital--;
            if (contadorDigital < 0) 
            {
                contadorDigital = 99;
            }
          digitalWrite(6, 0); // si bototn baja gira en otro sentido
          digitalWrite(5, 1);
        }
        printContador(contadorDigital);
    }
  delay(25);
}
```

#### Función mostrar números primos
```cpp
/*
la funcion de abajo muestra los nuumero primos desde 2 hasta 99
los verifica utilizando esPrimo 
*/
void mostrarNumerosPrimos() 
{
  for (contadorDigital = 2; contadorDigital <= 99; contadorDigital++) 
  {
    if (esPrimo(contadorDigital)) 
    {
      Serial.println(contadorDigital);
      printContador(contadorDigital);
      delay(1000); // Agregar un retraso para mostrar los números primos
    }
  }
}

```

#### Función para imprimir
```cpp
void printDigit(int digit) 
{
  digitalWrite(A, LOW);
  digitalWrite(B, LOW);
  digitalWrite(C, LOW);
  digitalWrite(D, LOW);
  digitalWrite(E, LOW);
  digitalWrite(F, LOW);
  digitalWrite(G, LOW);
  
  switch (digit) 
  {
    case 0:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(G, HIGH);
      break;
    case 1:
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      break;
    case 2:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      break;
    case 3:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(C, HIGH);
      break;
    case 4:
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    case 5:
      digitalWrite(A, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(C, HIGH);
      break;
    case 6:
      digitalWrite(A, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(C, HIGH);
      break;
    case 7:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      break;
    case 8:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    case 9:
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
  }
}

```

#### Función prende dígito
```cpp
void prendeDigito(int digito) 
{
  if (digito == UNIDAD) 
  {
    //enciendo el display de las unidades y apago el de las decenas
    digitalWrite(UNIDAD, LOW);
    digitalWrite(DECENA, HIGH);
  } else if (digito == DECENA) 
  {
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, LOW);
  } 
  else 
  {
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, HIGH);
  }
  delay(5);
}
```

#### Función mostrar dígito de las unidades
```cpp
void printContador(int contador) 
{
  // Mostrar el dígito de las unidades
  prendeDigito(APAGADOS);
  printDigit(contador % 10);//si contador es 27 con % muestro el 7en el display de las unidades
  prendeDigito(UNIDAD);
  
  prendeDigito(APAGADOS);
  printDigit(contador / 10);// si contador es 27 con / muesteo el 2 en el display de decenas
  prendeDigito(DECENA);
}

```

#### Función para leer el estado de los botones
```cpp
int keypressed(void) //keypressed lee el estado de los botones y determina cual se pulso
{
  //Leo el estado de los botones
  int PulsadorSube = digitalRead(SUBE);//digitalRead - lee si estas apretando o no un boton
  int PulsadorBaja = digitalRead(BAJA);

  // Verifico si se presionaron los botones y actualizar el estado anterior
  if (PulsadorSube == 1) 
  {
    PulsadorSubeAntes = 1;
  }

  if (PulsadorBaja == 1) 
  {
    PulsadorBajaAntes = 1;
  }

  // Determinar qué botón se presionó y retornar su valor correspondiente
  if (PulsadorSube == 0 && PulsadorSube != PulsadorSubeAntes) 
  {
    PulsadorSubeAntes = PulsadorSube;
    return SUBE;
  }
  if (PulsadorBaja == 0 && PulsadorBaja != PulsadorBajaAntes) 
  {
    PulsadorBajaAntes = PulsadorBaja;
    return BAJA;
  }
  return 0;
}

```

#### Función para determinar si es primo 
```cpp
/*
esPrimo verifica si un numero es primo. si no es divisible
por ningun numero distinto de 1 y si mismo se considera primo
*/
bool esPrimo(int numero) 
{
  if (numero <= 1) 
  {
    return false;
  }
  for (int i = 2; i * i <= numero; i++) 
  {
    if (numero % i == 0) 
    {
      return false;
    }
  }
  return true;
}

```


------------


## **Link al proyecto**
[Proyecto en tinkercad](https://www.tinkercad.com/things/0Wt6JzUxBMP-copy-of-copy-of-parcial-1-borrador/editel?sharecode=5LjgxSROw1uT5xxDbbGF1KB4Bn0fdWXFns52MOa6mmU "Proyecto en tinkercad")


------------

## **Fuentes** 
- [Clase 4 | SPD](https://www.youtube.com/watch?v=_Ry7mtURGDE&list=PL7LaR6_A2-E11BQXtypHMgWrSR-XOCeyD&index=4&t=1094s "Clase 4 | SPD")
- [Página oficial Arduino](https://www.arduino.cc/ "Página oficial Arduino")
