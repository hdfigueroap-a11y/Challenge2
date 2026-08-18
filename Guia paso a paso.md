# Guía Paso a Paso — Construir la Topología en Packet Tracer

Esta guía explica **cómo armar y configurar** toda la topología en Packet Tracer, desde colocar los dispositivos hasta verificar que todo funcione. Para copiar los comandos completos de cada dispositivo, usa el otro documento (`configuracion_switches.md`) — aquí solo se referencian las secciones.

## Antes de empezar: dispositivos que necesitas

| Cantidad | Dispositivo         | Notas                                              |
|----------|----------------------|------------------------------------------------------|
| 3        | Router (ISR, ej. 1941)| Necesitan módulo serial WIC-2T (no viene por defecto) |
| 6        | Switch 2960          | S1, S2, S3, S4, S5, S6                                |
| 1        | Servidor (Server-PT)  | ServerDNS                                             |
| Varios   | PC-PT                 | Ver lista completa abajo                              |
| 3        | Laptop-PT             | Portátil IT, Empleado Externo 1 y 2 — necesitan módulo PT-LAPTOP-NM-1CFE |

---

## Paso 1: Colocar los dispositivos en el lienzo

Arrastra desde el panel inferior izquierdo (categorías Network Devices → Routers / Switches, y End Devices) lo siguiente, agrupado por sede para que sea más fácil ubicarlos:

**Bogotá:** R1, S1, S2, S3, PC Secretaría, PC Gerencia, PC CCTV, PC Secretaría Admin, PC Gerencia Admin

**Cali:** R2, S4, PC Contaduría, PC Tesorería, Portátil IT, Empleado Externo 1, Empleado Externo 2

**Cúcuta:** R3, S5, S6, PC15, PC16, ServerDNS

Nombra cada dispositivo igual que en el documento de configuración (clic en el nombre debajo del ícono) para no perderte después: S1, S2, S3, S4, S5, S6, R1, R2, R3.

## Paso 2: Agregar el módulo serial a los routers

Los routers de Packet Tracer no traen interfaz serial por defecto. Para R1, R2 y R3:

1. Clic en el router → pestaña **Physical**.
2. Apágalo con el botón de encendido (arriba a la izquierda del panel).
3. Arrastra el módulo **WIC-2T** a uno de los slots vacíos.
4. Enciende el router de nuevo.

R1 necesita 1 módulo (un puerto serial hacia R2). R2 necesita 1 módulo con 2 puertos, o revisa que el WIC-2T te dé los dos que usas (Se0/0/0 y Se0/0/1). R3 necesita 1 módulo (un puerto serial hacia R2).

## Paso 3: Agregar el módulo Ethernet a los portátiles

Para Portátil IT, Empleado Externo 1 y Empleado Externo 2:

1. Clic en el laptop → pestaña **Physical**.
2. Apágalo.
3. Arrastra **PT-LAPTOP-NM-1CFE** al slot vacío.
4. Enciende el equipo.

## Paso 4: Cablear la topología

Usa cable de **cobre recto (Copper Straight-Through)** para todo excepto los enlaces seriales. En Packet Tracer, si eliges el cable "Automático" (el rayo ⚡), él elige el tipo correcto sin que tengas que pensarlo — es la opción más simple.

| # | Desde       | Puerto        | Hasta       | Puerto        | Tipo de cable        |
|---|-------------|----------------|-------------|----------------|------------------------|
| 1 | S1          | Gi0/1          | R1          | Gi0/1          | Cobre (o automático)   |
| 2 | S1          | Gi0/2          | S2          | Gi0/2          | Cobre (o automático)   |
| 3 | S1          | Fa0/24         | S3          | Fa0/24         | Cobre (o automático)   |
| 4 | S2          | Fa0/1          | PC Secretaría | -            | Cobre recto            |
| 5 | S2          | Fa0/2          | PC Gerencia | -              | Cobre recto            |
| 6 | S3          | Fa0/1          | PC CCTV     | -              | Cobre recto            |
| 7 | S3          | Fa0/2          | PC Secretaría Admin | -     | Cobre recto            |
| 8 | S3          | Fa0/3          | PC Gerencia Admin | -       | Cobre recto            |
| 9 | R1          | Se0/0/0        | R2          | Se0/0/0        | **Serial (DTE-DCE)**   |
| 10| R2          | Gi0/1          | S4          | Gi0/1          | Cobre (o automático)   |
| 11| S4          | Fa0/1          | PC Contaduría | -            | Cobre recto            |
| 12| S4          | Fa0/2          | PC Tesorería | -              | Cobre recto            |
| 13| S4          | Fa0/3          | Portátil IT | -              | Cobre recto            |
| 14| S4          | Fa0/4          | Empleado Externo 1 | -       | Cobre recto            |
| 15| S4          | Fa0/5          | Empleado Externo 2 | -       | Cobre recto            |
| 16| R2          | Se0/0/1        | R3          | Se0/0/1        | **Serial (DTE-DCE)**   |
| 17| R3          | Gi0/1          | S5          | Gi0/1          | Cobre (o automático)   |
| 18| S5          | Fa0/2          | PC15        | -              | Cobre recto            |
| 19| S5          | Gi0/2          | S6          | Gi0/1          | Cobre (o automático)   |
| 20| S6          | Fa0/2          | PC16        | -              | Cobre recto            |
| 21| S6          | Gi0/2          | ServerDNS   | -              | Cobre recto            |

Los enlaces seriales (9 y 16) necesitan que ambos extremos tengan el módulo WIC-2T ya puesto (Paso 2) antes de poder cablearlos.

## Paso 5: Configurar los switches

Para cada switch (S1, S2, S3, S4, S5, S6):

1. Clic en el switch → pestaña **CLI**.
2. Pega el bloque de configuración completo de su sección correspondiente en `configuracion_switches.md` (secciones 1, 2, 3, 6, 7 y 8) — cada bloque ya incluye hostname, VLANs, interfaces y Port Security.

Hazlo en este orden para no perder conectividad mientras pruebas: **S1 primero** (es el núcleo de Bogotá), luego S2 y S3, después S4, y finalmente S5 y S6.

## Paso 6: Configurar los routers

Para R1, R2 y R3:

1. Clic en el router → pestaña **CLI**.
2. Pega el bloque de configuración de la sección 10 de `configuracion_switches.md` (interfaces, subinterfaces e IPs).
3. Revisa cuál lado de cada enlace serial es DCE con `show controllers serial 0/0/0` — en tu topología ya confirmamos que **R2 es DCE en los dos enlaces**, así que el `clock rate 64000` va en R2 (ya viene incluido en el bloque).

## Paso 7: Configurar el enrutamiento RIPv2

En R1, R2 y R3, pega el bloque correspondiente de la sección 11 de `configuracion_switches.md` (los tres bloques marcados `--- R1 ---`, `--- R2 ---`, `--- R3 ---`).

## Paso 8: Configurar las IPs en PCs, laptops y el servidor

Para cada equipo final (todas las PCs, los 3 portátiles y ServerDNS):

1. Clic en el equipo → pestaña **Desktop** → **IP Configuration**.
2. Selecciona **Static**.
3. Ingresa IP, máscara y gateway según la tabla de su sede en la sección 4 (Bogotá) o sección 9 (Cali/Cúcuta) de `configuracion_switches.md`.

## Paso 9: Verificar que todo funcione

1. En cualquier router, `show ip interface brief` — todas las interfaces que uses deben estar **up/up**. Si alguna dice "administratively down", le falta `no shutdown`.
2. `show ip route rip` — deberías ver las redes de las otras sedes marcadas con `R`.
3. Desde una PC, abre el símbolo de sistema (icono en Desktop) y haz ping primero a su propio gateway, luego a una IP de otra sede (ej. desde PC Gerencia en Bogotá, `ping 192.168.70.10` para llegar a PC15 en Cúcuta).
4. Si algo no responde, revisa el checklist que ya vimos antes en el chat (interfaces apagadas, módulo serial faltante, VLAN mal asignada, IP/gateway mal escrito).

---

Con estos 9 pasos queda toda la topología armada: 3 sedes, VLANs, enrutamiento entre sedes y Port Security aplicado.
