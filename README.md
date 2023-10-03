# **Primer parcial SPD**
![](https://canaltic.com/blog/wp-content/uploads/2020/08/pc17.jpg)


------------
## **Integrantes**
#### -  Rodrigo Urribarri
#### -  Valentin Trivelli
#### -  Santiago Vallina

------------
## **Proyecto : Contador de 0 a 99 con Display 7 Segmentos y Multiplexación**
![](https://scontent.feze10-1.fna.fbcdn.net/v/t39.30808-6/386074171_7084563294887476_7698564554994781599_n.jpg?_nc_cat=110&ccb=1-7&_nc_sid=49d041&_nc_eui2=AeFI1GBm2eufIwBTD7GOz75tTLTHMZgGBbFMtMcxmAYFsbvlUlDjUQzzpdJf9G-adWMz6Dc22Ls1ySJKiOU9NJWR&_nc_ohc=V9V75ujk8qoAX_kQUbk&_nc_ht=scontent.feze10-1.fna&oh=00_AfDLgS37bYr3AGZKJKjyucVt53JL_lno83PtvDaDjQB3kw&oe=652175EB)

------------

## **Descripción**
Es un contador de 0 a 99 que utiliza dos displays de 7 segmentos y tres botones para
controlar la cuenta. El contador comienza en 0 y es capaz de aumentar o disminuir
su valor en una unidad con los botones.


------------

## Funciones

#### Configuración de pines como entrada con resistencia pull-up para los botones
```cpp
pinMode(SUBE, INPUT_PULLUP);
  pinMode(BAJA, INPUT_PULLUP); //PULLUP -- SI NO APRETO BOTON LEE 1, SINO 0
  pinMode(RESET, INPUT_PULLUP);
```

#### Configuración de pines como salida para los segmentos de los displays
```cpp
  pinMode(A, OUTPUT);
  pinMode(B, OUTPUT);
  pinMode(C, OUTPUT);
  pinMode(D, OUTPUT);// OUTPUT el pin se comporta como salida digital
  pinMode(E, OUTPUT);
  pinMode(F, OUTPUT);
  pinMode(G, OUTPUT);
  pinMode(UNIDAD, OUTPUT);
  pinMode(DECENA, OUTPUT);
```

#### Apago los displays al inicio
```cpp
digitalWrite(UNIDAD, LOW);
  digitalWrite(DECENA, LOW);
```

#### muestro el dígito 0 al inicio
```cpp
printDigit(0);
  Serial.begin(9600);
```

#### Función loop
```cpp
{
  int botonPresionado = keypressed();

  // Verificar el botón y actualizar el contador
  if (botonPresionado == SUBE)
  {
    contadorDigital++;
    if (contadorDigital > 99)
    {
      contadorDigital = 0;
    }
  }
  else if (botonPresionado == BAJA)
  {
    contadorDigital--;
    if (contadorDigital < 0)
    {
      contadorDigital = 99;
    }
  }
  else if (botonPresionado == RESET)
  {
    contadorDigital = 0;
  }

	// Muestroel contador en los displays
  printContador(contadorDigital);
}
```

#### Función printDigit
```cpp
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
    {
      digitalWrite(A, HIGH);
	  digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
 
    case 1:
    {
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      break;
    }
    
    case 2:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      break;
    }
    
    case 3:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(C, HIGH);
      break;
    }
    case 4:
    {
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 5:
    {
      digitalWrite(A, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(C, HIGH);
      break;
    }
    case 6:
    {
      digitalWrite(A, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(C, HIGH);
      break;
    }
    case 7:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      break;
    }
    case 8:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 9:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
    }
  }
}
```

#### Función prendeDigito
```cpp
{
  if (digito == UNIDAD)
  {
    //enciendo el display de las unidades 
    //y apago el de las decenas
    digitalWrite(UNIDAD, LOW);
    digitalWrite(DECENA, HIGH);
    millis();
  }
  else if (digito == DECENA)
  {
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, LOW);
    millis();
  }
  else
  {
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, HIGH);
  }
  delay(5);
}

```

#### Función Mostrar contador
```cpp
{
  // Mostrar el dígito de las unidades
  prendeDigito(APAGADOS);
  printDigit(contador % 10);//si contador es 27 con % muestro el 7en el display de las unidades
  prendeDigito(UNIDAD); // Agregar un retraso para mostrar el dígito
  
  prendeDigito(APAGADOS);
  printDigit(contador / 10);// si contador es 27 con / muesteo el 2 en el display de decenas
  prendeDigito(DECENA); // Agregar un retraso para mostrar el dígito
}

```

#### Función keypressed
```cpp
{
  //Leo el estado de los botones
  int PulsadorSube = digitalRead(SUBE);//digitalRead - lee si estas apretando o no un boton
  int PulsadorBaja = digitalRead(BAJA);
  int PulsadorReset = digitalRead(RESET);

  // Verifico si se presionaron los botones y actualizar el estado anterior
  if (PulsadorSube == 1)
  {
    PulsadorSubeAntes = 1;
  }

  if (PulsadorBaja == 1)
  {
    PulsadorBajaAntes = 1;
  }

  if (PulsadorReset == 1)
  {
    PulsadorResetAntes = 1;
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
  if (PulsadorReset == 0 && PulsadorReset != PulsadorResetAntes)
  {
    PulsadorResetAntes = PulsadorReset;
    return RESET;
  }
  return 0;
}
```


------------

#### Link al proyecto
[Proyecto en tinkercad](http://https://www.tinkercad.com/things/767xKz6ffJj-parcial-1-/editel?sharecode=DOqrWk_7w3ryhX7fDc6UY9oX_hrwBWHwONy6Dyfgkc0 "Proyecto en tinkercad")




