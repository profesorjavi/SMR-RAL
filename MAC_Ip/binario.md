# Conversión de números decimales a binarios

## 1. Introducción al sistema binario

Los ordenadores utilizan internamente el **sistema binario** para representar y procesar la información.

A diferencia del sistema decimal, que utilizamos habitualmente y que dispone de diez dígitos (del 0 al 9), el sistema binario solamente utiliza dos dígitos:

- **0:** representa un estado apagado o ausencia de señal.
- **1:** representa un estado encendido o presencia de señal.

Cada uno de estos dígitos se denomina **bit** (*Binary Digit*).

| Sistema | Base | Dígitos utilizados           |
| ------- | ---- | ---------------------------- |
| Decimal | 10   | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 |
| Binario | 2    | 0, 1                         |

Aunque utilizamos el sistema decimal en nuestra vida cotidiana, los ordenadores trabajan con el sistema binario porque sus circuitos electrónicos pueden representar fácilmente dos estados diferentes.

## 2. El valor de las posiciones en el sistema binario

El sistema decimal y el binario son sistemas de numeración posicionales. Esto significa que el valor de cada cifra depende de la posición que ocupa dentro del número.

En el sistema decimal, cada posición representa una potencia de 10:

| Posición | 10³  | 10² | 10¹ | 10⁰ |
| -------- | ---- | --- | --- | --- |
| Valor    | 1000 | 100 | 10  | 1   |

Por ejemplo, el número decimal 345 se calcula de la siguiente manera:

[\
345 = 3 \times 100 + 4 \times 10 + 5 \times 1\
]

En el sistema binario ocurre exactamente lo mismo, pero utilizando potencias de 2.

Las posiciones de un número binario tienen los siguientes valores:

| Potencia | 2⁷  | 2⁶ | 2⁵ | 2⁴ | 2³ | 2² | 2¹ | 2⁰ |
| -------- | --- | -- | -- | -- | -- | -- | -- | -- |
| Valor    | 128 | 64 | 32 | 16 | 8  | 4  | 2  | 1  |

**Importante:** el valor de las posiciones se calcula siempre de derecha a izquierda, comenzando por (2^0).

Por ejemplo, el número binario `1011` representa:

[\
1 \times 8 + 0 \times 4 + 1 \times 2 + 1 \times 1\
]

[\
8 + 0 + 2 + 1 = 11\
]

Por tanto:

[\
1011_2 = 11\_{10}\
]

Los subíndices indican el sistema de numeración utilizado: base 2 para binario y base 10 para decimal.

## 3. Conversión de decimal a binario

Para convertir un número decimal a binario podemos utilizar dos métodos.

### 3.1. Método de las divisiones sucesivas entre 2

Este método consiste en dividir sucesivamente el número decimal entre 2 hasta obtener un cociente igual a cero.

En cada división debemos apuntar el resto, que siempre será 0 o 1.

**Ejemplo: convertir el número decimal 25 a binario.**

Realizamos las siguientes divisiones:

| División | Cociente | Resto |
| -------- | -------- | ----- |
| 25 ÷ 2   | 12       | 1     |
| 12 ÷ 2   | 6        | 0     |
| 6 ÷ 2    | 3        | 0     |
| 3 ÷ 2    | 1        | 1     |
| 1 ÷ 2    | 0        | 1     |

Una vez terminadas las divisiones, debemos leer los restos **de abajo hacia arriba**.

Obtenemos:

[\
25\_{10} = 11001_2\
]

**Pasos que debemos seguir:**

1. Dividir el número decimal entre 2.
2. Apuntar el resto de la división.
3. Dividir el cociente obtenido entre 2.
4. Repetir el proceso hasta que el cociente sea 0.
5. Leer todos los restos de abajo hacia arriba.

### 3.2. Método de descomposición en potencias de 2

Este método consiste en descomponer el número decimal como una suma de potencias de 2.

Para ello, buscamos las potencias de 2 que sumadas nos proporcionen el número que queremos convertir.

**Ejemplo: convertir el número decimal 19 a binario.**

Primero, identificamos las potencias de 2 que necesitamos:

[\
19 = 16 + 2 + 1\
]

A continuación, colocamos un 1 en las posiciones de las potencias utilizadas y un 0 en las posiciones que no utilizamos.

| Potencia     | 2⁴ | 2³ | 2² | 2¹ | 2⁰ |
| ------------ | -- | -- | -- | -- | -- |
| Valor        | 16 | 8  | 4  | 2  | 1  |
| ¿Se utiliza? | 1  | 0  | 0  | 1  | 1  |

Por tanto:

[\
19\_{10} = 10011_2\
]

**Resumen del método:**

- Si utilizamos una potencia de 2, escribimos un 1.
- Si no utilizamos una potencia de 2, escribimos un 0.
- Leemos los bits de izquierda a derecha, desde la potencia mayor hasta la menor.

## 4. Conversión de binario a decimal

Para convertir un número binario a decimal debemos multiplicar cada bit por el valor de su posición y sumar todos los resultados.

**Ejemplo: convertir el número binario 11001 a decimal.**

Primero, identificamos el valor de cada posición:

| Bit      | 1  | 1  | 0  | 0  | 1  |
| -------- | -- | -- | -- | -- | -- |
| Potencia | 2⁴ | 2³ | 2² | 2¹ | 2⁰ |
| Valor    | 16 | 8  | 4  | 2  | 1  |

Multiplicamos cada bit por su valor:

[\
1 \times 16 + 1 \times 8 + 0 \times 4 + 0 \times 2 + 1 \times 1\
]

Sumamos los resultados:

[\
16 + 8 + 0 + 0 + 1 = 25\
]

Por tanto:

[\
11001_2 = 25\_{10}\
]

**Pasos que debemos seguir:**

1. Asignar a cada posición su correspondiente potencia de 2.
2. Multiplicar cada bit por el valor de su posición.
3. Sumar todos los resultados.
4. El resultado será el número decimal.

## 5. El sistema binario y las direcciones IP

En redes informáticas utilizamos continuamente el sistema binario, especialmente cuando trabajamos con direcciones IPv4 y máscaras de subred.

Una dirección IPv4 está formada por **32 bits**, agrupados en cuatro bloques de 8 bits denominados octetos.

Por ejemplo:

`192.168.1.10`

Cada uno de los cuatro números decimales se puede representar mediante 8 bits.

| Decimal | Binario  |
| ------- | -------- |
| 192     | 11000000 |
| 168     | 10101000 |
| 1       | 00000001 |
| 10      | 00001010 |

Por tanto, la dirección IP anterior en binario sería:

`11000000.10101000.00000001.00001010`

### 5.1. ¿Por qué cada octeto tiene 8 bits?

Un octeto tiene 8 posiciones binarias.

Los valores de sus posiciones son:

| Bit      | 128 | 64 | 32 | 16 | 8  | 4  | 2  | 1  |
| -------- | --- | -- | -- | -- | -- | -- | -- | -- |
| Potencia | 2⁷  | 2⁶ | 2⁵ | 2⁴ | 2³ | 2² | 2¹ | 2⁰ |

Si todos los bits están a 0:

[\
00000000_2 = 0\_{10}\
]

Si todos los bits están a 1:

[\
11111111_2 = 128+64+32+16+8+4+2+1\
]

[\
11111111_2 = 255\_{10}\
]

Por tanto, cada octeto de una dirección IPv4 puede representar valores comprendidos entre **0 y 255**.

Este concepto será fundamental para entender posteriormente las direcciones IP y las máscaras de subred.

---

## 6. Ejercicios propuestos

### Ejercicio 1. Conversión de decimal a binario

Convierte los siguientes números decimales a binario utilizando el método de las divisiones sucesivas entre 2.

| Número decimal | Número binario |
| -------------- | -------------- |
| 5              | <br>           |
| 8              | <br>           |
| 10             | <br>           |
| 15             | <br>           |
| 20             | <br>           |
| 25             | <br>           |
| 32             | <br>           |
| 45             | <br>           |
| 64             | <br>           |
| 100            | <br>           |

### Ejercicio 2. Conversión de binario a decimal

Convierte los siguientes números binarios a decimal.

| Número binario | Número decimal |
| -------------- | -------------- |
| 101            | <br>           |
| 1000           | <br>           |
| 1010           | <br>           |
| 1111           | <br>           |
| 10000          | <br>           |
| 11001          | <br>           |
| 101010         | <br>           |
| 1100100        | <br>           |
| 11111111       | <br>           |

### Ejercicio 3. Conversión de octetos de direcciones IP

Convierte los siguientes números decimales a binario utilizando siempre 8 bits.

| Decimal | Binario |
| ------- | ------- |
| 10      | <br>    |
| 25      | <br>    |
| 100     | <br>    |
| 127     | <br>    |
| 128     | <br>    |
| 192     | <br>    |
| 200     | <br>    |
| 255     | <br>    |

### Ejercicio 4. Comprueba tus conocimientos

Responde a las siguientes preguntas:

1. ¿Cuántos dígitos utiliza el sistema binario?
2. ¿Qué valor representa la posición situada más a la derecha?
3. ¿Cuál es el valor decimal del número binario `1011`?
4. ¿Cuál es el valor binario del número decimal 16?
5. ¿Cuántos bits tiene una dirección IPv4?
6. ¿Cuántos bits tiene cada octeto?
7. ¿Cuál es el valor decimal máximo que podemos representar con 8 bits?
8. ¿Por qué los ordenadores utilizan el sistema binario?

---

## 7. Resumen

| Operación         | Procedimiento                                                          |
| ----------------- | ---------------------------------------------------------------------- |
| Decimal → Binario | Dividir sucesivamente entre 2 y leer los restos de abajo hacia arriba. |
| Decimal → Binario | Descomponer el número en potencias de 2.                               |
| Binario → Decimal | Multiplicar cada bit por su potencia de 2 y sumar los resultados.      |

**Recuerda:**

- El sistema decimal utiliza 10 dígitos.
- El sistema binario utiliza únicamente 0 y 1.
- Cada posición binaria representa una potencia de 2.
- Un bit puede tener dos valores: 0 o 1.
- Un octeto tiene 8 bits y puede representar valores entre 0 y 255.
- Las direcciones IPv4 tienen 32 bits distribuidos en cuatro octetos.

