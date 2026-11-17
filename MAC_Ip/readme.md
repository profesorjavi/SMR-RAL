### MAC · IP · Subnetting · ARP · ICMP


## Índice

0. [Binario](binario.md)
1. [La idea general: el cartero de la red](#1-la-idea-general-el-cartero-de-la-red)
2. [Dirección MAC](#2-dirección-mac)
3. [Dirección IP](#3-dirección-ip)
4. [Subnetting](#4-subnetting)
5. [Protocolo ARP](#5-protocolo-arp)
6. [Protocolo ICMP](#6-protocolo-icmp)
7. [Chuleta final](#7-chuleta-final)
8. [Ejercicios y soluciones](#8-ejercicios-y-soluciones)

---

## 1. La idea general: el cartero de la red

Para que un paquete llegue a su destino necesita **dos direcciones**, igual que una carta:

| | Dirección MAC | Dirección IP |
|---|---|---|
| **Es como...** | El DNI de tu tarjeta de red | Tu dirección postal |
| **La pone...** | El fabricante | El administrador o el router (DHCP) |
| **¿Cambia?** | Normalmente no | Sí, si te "mudas" de red |
| **Sirve para...** | Llegar al equipo **dentro de tu red** | Llegar a **cualquier red** |

💡 **Resumen:** la IP te lleva hasta el barrio; la MAC te lleva hasta el buzón concreto.

---

## 2. Dirección MAC

### ¿Qué es?

Es la **dirección física** de una tarjeta de red (cable o wifi). Viene grabada de fábrica.

- Tiene **48 bits** = **6 bytes** = **12 cifras hexadecimales** (0-9 y A-F).
- Se escribe en parejas separadas por `:` o `-`.

```
3C:52:82:1A:B4:F0
└──┬───┘ └──┬───┘
  OUI     Número de serie
(fabricante)  (de esa tarjeta)
```

- **OUI** (3 primeros bytes): identifica al fabricante (Intel, TP-Link, Apple...).
- **Resto** (3 últimos bytes): número que el fabricante asigna a cada tarjeta.

### Cosas que conviene saber

- Trabaja en la **capa 2 (Enlace)** del modelo OSI.
- Solo se usa **dentro de la red local**. Para salir a otra red hace falta la IP.
- Existe una MAC especial de **broadcast**: `FF:FF:FF:FF:FF:FF` = *"¡hola a todos!"*.
- Aunque viene de fábrica, **se puede cambiar por software** (MAC spoofing). No es tan inmutable como parece.

### ¿Cómo la veo?

| Sistema | Comando | Busca la línea... |
|---|---|---|
| Windows | `ipconfig /all` | Dirección física |
| Linux | `ip link` | link/ether |

---

## 3. Dirección IP

### ¿Qué es?

Es la **dirección lógica** de un equipo en una red. En IPv4:

- **32 bits** repartidos en **4 octetos** (de 8 bits cada uno).
- Cada octeto va de **0 a 255**.
- Los humanos la leemos en decimal; los ordenadores, en binario.

```
Decimal:   192   .   168   .    1    .    10
Binario:   11000000.10101000.00000001.00001010
```

### Estática o dinámica

- **Estática:** la configuras tú a mano (servidores, impresoras, routers).
- **Dinámica:** te la da un servidor **DHCP** (casi todos los equipos de usuario).

### Máscara de subred

La máscara dice **dónde acaba la red y dónde empieza el equipo (host)**.

```
IP:       192.168.1.10
Máscara:  255.255.255.0   =  /24
          └────red────┘ └host┘
          (24 bits)     (8 bits)
```

- `/24` significa "24 unos seguidos" en la máscara.
- Si dos equipos están en la **misma red**, se hablan directamente.
- Si están en **redes distintas**, necesitan un **router**.

**Ejemplo (máscara /24):**

| Equipo A | Equipo B | ¿Misma red? |
|---|---|---|
| 192.168.1.10 | 192.168.1.20 | ✅ Sí (los dos son 192.168.1.x) |
| 192.168.1.10 | 192.168.2.5 | ❌ No (distinta red) |

### Puerta de enlace (gateway)

Es la **IP del router**, la "puerta de salida" hacia otras redes. Si no está bien puesta, tendrás red local pero **no Internet**.

### IPs públicas y privadas

| Rango privado | Máscara | Uso típico |
|---|---|---|
| 10.0.0.0 – 10.255.255.255 | /8 | Empresas grandes |
| 172.16.0.0 – 172.31.255.255 | /12 | Empresas medianas |
| 192.168.0.0 – 192.168.255.255 | /16 | Casa y aulas |

Las privadas **no salen a Internet tal cual**: el router las "traduce" con **NAT**. Todas las demás son **públicas**.

### Direcciones especiales

| Dirección | Nombre | En cristiano |
|---|---|---|
| `127.0.0.1` | Loopback | "Yo mismo" |
| `169.254.x.x` | APIPA | "No encontré DHCP y me he inventado una IP" |
| `0.0.0.0` | Sin dirección | "Aún no tengo IP" |

> 🧠 **IPv6** existe porque las IPv4 se acaban (32 bits dan "solo" unos 4.300 millones). Tiene 128 bits y se escribe en hexadecimal, por ejemplo `2001:db8::1`. Lo verás con más calma más adelante.

---

## 4. Subnetting

### ¿Para qué sirve?

**Cortar una red grande en trocitos** (subredes). Así conseguimos:

- Separar departamentos o aulas.
- Menos tráfico de broadcast (la red va más fluida).
- Más seguridad (cada trozo con sus reglas).
- Aprovechar mejor las direcciones.

### Las dos fórmulas que necesitas

> **Hosts útiles por subred = 2<sup>h</sup> − 2**
> (*h* = bits de host, es decir, los **ceros** de la máscara)

> **Número de subredes = 2<sup>n</sup>**
> (*n* = bits que le "robamos" a la parte de host)

❓ **¿Por qué restamos 2?** Porque en cada subred hay dos direcciones que **no se pueden asignar a equipos**:

- La **primera**: dirección de **red**.
- La **última**: dirección de **broadcast**.

### Chuleta de máscaras

| Prefijo | Máscara | Bits de host | Hosts útiles | Salto |
|---|---|---|---|---|
| /24 | 255.255.255.0 | 8 | 254 | 256 |
| /25 | 255.255.255.128 | 7 | 126 | 128 |
| /26 | 255.255.255.192 | 6 | 62 | 64 |
| /27 | 255.255.255.224 | 5 | 30 | 32 |
| /28 | 255.255.255.240 | 4 | 14 | 16 |
| /29 | 255.255.255.248 | 3 | 6 | 8 |
| /30 | 255.255.255.252 | 2 | 2 | 4 |

🪄 **Truco del salto:** `salto = 256 − último octeto de la máscara`. Ejemplo: 256 − 192 = **64**.

### Ejemplo paso a paso

**Enunciado:** tenemos la red `192.168.1.0/24` y queremos **4 subredes** (una por aula).

1. **¿Cuántos bits pedimos?** 4 subredes = 2² → pedimos **2 bits**.
2. **Nueva máscara:** /24 + 2 = **/26** → `255.255.255.192`.
3. **Salto:** 256 − 192 = **64**.
4. **Hosts por subred:** 2⁶ − 2 = **62**.

**Resultado:**

| Subred | Primer host | Último host | Broadcast |
|---|---|---|---|
| 192.168.1.0/26 | 192.168.1.1 | 192.168.1.62 | 192.168.1.63 |
| 192.168.1.64/26 | 192.168.1.65 | 192.168.1.126 | 192.168.1.127 |
| 192.168.1.128/26 | 192.168.1.129 | 192.168.1.190 | 192.168.1.191 |
| 192.168.1.192/26 | 192.168.1.193 | 192.168.1.254 | 192.168.1.255 |

Fíjate: cada subred empieza donde acaba la anterior, de **64 en 64**. 🎯

### ¿A qué subred pertenece una IP?

Busca el **múltiplo del salto** más grande que no se pase de tu número.

*Ejemplo:* `192.168.1.130/26` → salto 64 → múltiplos: 0, 64, **128**, 192 → pertenece a la subred **192.168.1.128/26**.

---

## 5. Protocolo ARP

**ARP** = *Address Resolution Protocol*.

### El problema

Tu PC conoce la **IP** del destino (`192.168.1.20`), pero para enviar la trama por la red local necesita su **MAC**. ¿Cómo la consigue? **Preguntando.**

### Cómo funciona

1. **ARP Request (broadcast):** el PC A grita a toda la red: *"¿Quién tiene la IP 192.168.1.20? ¡Contestad a 192.168.1.10!"* (destino MAC `FF:FF:FF:FF:FF:FF`).
2. Todos lo reciben, pero **solo el dueño de esa IP responde**. Los demás pasan del tema.
3. **ARP Reply (unicast):** el PC B responde solo a A: *"¡Soy yo! Mi MAC es BB:BB:BB:22:22:22"*.
4. A **guarda la pareja IP–MAC** en su **caché ARP** y ya puede enviarle tramas.

### La caché ARP

Para no preguntar siempre, cada equipo apunta lo que aprende en una tabla temporal (las entradas caducan pasados unos minutos).

```
> arp -a

Dirección IP      Dirección física     Tipo
192.168.1.1       3c-52-82-1a-b4-f0    dinámico
192.168.1.20      bb-bb-bb-22-22-22    dinámico
```

El comando `arp -a` funciona en Windows y en Linux.

### Detalles importantes

- Si el destino está en **otra red**, el PC **no pregunta por él**: pregunta por la MAC de la **puerta de enlace** (el router) y se la entrega a él.
- ARP **no tiene seguridad**: cualquiera puede contestar mintiendo. Eso es el **ARP spoofing**: alguien dice *"la IP del router soy yo"* y se cuela en medio de la comunicación. Los switches gestionables tienen defensas contra esto.

---

## 6. Protocolo ICMP

**ICMP** = *Internet Control Message Protocol*. Es el **sistema de avisos** de la red: informa de errores y comprueba si hay vida al otro lado.

- No transporta datos de usuario; viaja **dentro de paquetes IP**.
- Es el protocolo que usan **`ping`** y **`tracert`**.

### Mensajes más típicos

| Mensaje | Tipo | Para qué sirve |
|---|---|---|
| Echo Request | 8 | Ping: *"¿estás ahí?"* |
| Echo Reply | 0 | Ping: *"¡sí, aquí estoy!"* |
| Destination Unreachable | 3 | "No hay camino hasta ese destino" |
| Time Exceeded | 11 | Se acabó el TTL (lo usa `tracert`) |

### Herramientas

| Comando | Qué hace |
|---|---|
| `ping 192.168.1.1` | Comprueba si hay conexión y mide el tiempo de ida y vuelta |
| `tracert google.com` (Windows) | Muestra los routers por los que pasa el paquete |
| `traceroute google.com` (Linux) | Lo mismo en Linux |

### ¿Qué es el TTL?

*Time To Live.* Es un contador que cada **router resta 1**. Si llega a **0**, el paquete se descarta y se envía un aviso ICMP *Time Exceeded*. Así los paquetes no dan vueltas eternamente por la red. 🔄

### Diagnóstico en 5 pasos con ping

Cuando "no hay Internet", ve subiendo escalones:

| Paso | Comando | Pregunta que responde |
|---|---|---|
| 1 | `ping 127.0.0.1` | ¿Funciona la pila TCP/IP de mi equipo? |
| 2 | `ping` *(tu propia IP)* | ¿Está bien configurada mi tarjeta de red? |
| 3 | `ping` *(la puerta de enlace)* | ¿Llego hasta el router? |
| 4 | `ping 8.8.8.8` | ¿Salgo a Internet? |
| 5 | `ping google.com` | ¿Funciona el DNS? |

⚠️ **Ojo:** si un ping no responde, puede que el equipo de destino tenga el **firewall** bloqueando ICMP. ¡No siempre es que esté apagado!

---

## 7. Chuleta final

| Concepto | En una frase |
|---|---|
| **MAC** | Dirección física de la tarjeta (48 bits, hexadecimal). Capa 2, solo en la red local |
| **IP** | Dirección lógica de 32 bits (IPv4). Identifica al equipo y a su red |
| **Máscara** | Separa la parte de red de la parte de host |
| **Gateway** | La IP del router, la salida hacia otras redes |
| **Subnetting** | Dividir una red en trozos. Hosts útiles = 2<sup>h</sup> − 2 |
| **ARP** | Pregunta "¿quién tiene esta IP?" y obtiene su MAC |
| **ICMP** | Avisos y diagnóstico: `ping` y `tracert` |

---

## 8. Ejercicios y soluciones

### ✏️ Ejercicios

1. ¿Cuántos equipos caben en una red `/28`?
2. Divide `192.168.10.0/24` en **8 subredes**. ¿Qué máscara y qué salto tiene cada una?
3. ¿A qué subred pertenece la IP `192.168.1.130/26`?
4. ¿Están `10.0.0.5/24` y `10.0.1.5/24` en la misma red?
5. Tu PC no navega. Haces `ping 127.0.0.1` (OK) y `ping` a la puerta de enlace (sin respuesta). ¿Por dónde empezarías a buscar el fallo?

---

### ✅ Soluciones

*(¡No vale mirar antes de intentarlo! 😉)*

1. **14 equipos.** Con /28 quedan 4 bits de host → 2⁴ − 2 = 14.
2. 8 = 2³ → pedimos 3 bits → **/27** → máscara `255.255.255.224`, **salto 32** (30 hosts útiles por subred).
3. A la subred **192.168.1.128/26** (hosts del .129 al .190).
4. **No.** Una es `10.0.0.x` y la otra `10.0.1.x`: son redes distintas y necesitan un router.
5. El equipo está bien por dentro, pero **no llega al router**. Revisa: cable o wifi, que la IP y la máscara sean de la misma red que el router, que el equipo no tenga una IP `169.254.x.x` (sin DHCP) y que la puerta de enlace esté bien. Y recuerda que también podría ser un firewall bloqueando el ping.

