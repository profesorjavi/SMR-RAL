En un cable UTP Ethernet con conector RJ45 hay 8 hilos de cobre agrupados en 4 pares trenzados. Cada hilo tiene una función dentro de la transmisión de datos, aunque su función concreta depende de la velocidad Ethernet y del tipo de conexión.

Para explicarlo a alumnos de SMR, vamos a utilizar como referencia una conexión Ethernet con la norma T568B.

## 1. ¿Para qué sirve cada hilo?

![CAT.5E UTP 24AWG 4 Pairs PVC Gray Jacket Bare Solid Copper Bulk Cable - Network Cable and LAN Cable](https://images.openai.com/static-rsc-4/yX9lAi_mFNk_0HHb9Si-j8u3uSD-ICwlFfX4otQedKPLWZBwi6_zVhyA9y7d6auFXeJ6uACbjtZSSg7GhfMxUC27Ohki9s6R0HPtOD39raJG_ZC2hLn3Q53QoriNS2lc5Emf0QKihecq1owN_9twomXrK2Ex-B-hRsD3w4yPZKY?purpose=inline)

[fibermania.en.made-in-china.com](https://fibermania.en.made-in-china.com/product/xQpUduWOJAcD/China-CAT-5E-UTP-24AWG-4-Pairs-PVC-Gray-Jacket-Bare-Solid-Copper-Bulk-Cable.html?utm_source=chatgpt.com)

| Pin | Color según T568B | Función en 10/100 Mbps                  |
| --- | ----------------- | --------------------------------------- |
| 1   | Blanco/naranja    | Transmisión de datos (+)                |
| 2   | Naranja           | Transmisión de datos (−)                |
| 3   | Blanco/verde      | Recepción de datos (+)                  |
| 4   | Azul              | No se utiliza para datos en 10/100 Mbps |
| 5   | Blanco/azul       | No se utiliza para datos en 10/100 Mbps |
| 6   | Verde             | Recepción de datos (−)                  |
| 7   | Blanco/marrón     | No se utiliza para datos en 10/100 Mbps |
| 8   | Marrón            | No se utiliza para datos en 10/100 Mbps |

Importante: los signos (+) y (−) indican los dos conductores de una señal diferencial. No significan que sean, respectivamente, el positivo y el negativo de una alimentación eléctrica convencional.

## 2. ¿Por qué los hilos se agrupan en pares?

Los hilos no trabajan de forma aislada: se utilizan en parejas para transmitir una señal diferencial.

Par naranja: pines 1 y 2

Blanco/naranja

Naranja

Transmisión en 10/100BASE-TX.

Par verde: pines 3 y 6

Blanco/verde

Verde

Recepción en 10/100BASE-TX.

El trenzado reduce las interferencias electromagnéticas y la diafonía, mejorando la fiabilidad de la comunicación.

## 3. ¿Qué ocurre con Gigabit Ethernet (1 Gbps)?

Aquí está la diferencia importante:

- 10 Mbps y 100 Mbps: Ethernet utiliza dos pares para los datos.
- 1 Gbps (1000BASE-T): utiliza los cuatro pares simultáneamente para transmitir y recibir datos.

Por tanto, a 1 Gbps los ocho hilos participan en la comunicación. Los pares azul y marrón ya no quedan libres para los datos.

## 4. ¿Y para qué sirven los hilos que no transmiten datos en 100 Mbps?

En una conexión 10/100BASE-TX, los pares azul y marrón no se utilizan para transportar datos Ethernet. Sin embargo, pueden emplearse para otros fines compatibles con la instalación, como la alimentación mediante PoE, dependiendo del equipo y del estándar utilizado.

Además, PoE (Power over Ethernet) permite alimentar dispositivos, como puntos de acceso Wi-Fi o cámaras IP, a través del cable de red.


Comprobar