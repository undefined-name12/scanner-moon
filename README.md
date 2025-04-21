# MOON SCANNER
Un escáner y un Bruter con distribución de habilidades de trabajo

## Características
```
Escaneo inteligente
Alta velocidad de fuerza bruta (17-21 ataques SSH por segundo)
Alta velocidad de escaneo
Distribución de carga
Sockets tolerantes a fallos
Modo interproceso
SSH
```

## La arquitectura Moon scanner
### Balanceo de carga
```sirena
grafo TD;
Moon scannerScanner --> |126.207.193.116| Moon scannerbruter1;
Moon scannerScanner --> |159.27.168.28| Moon scannerbruter2;
Moon scannerScanner --> |156.213.208.17| Moon scannerbruter3;
Moon scannerScanner --> |11.77.180.157| Moon scannerbruter4; ```

## Moon scannerBruter
### ola rápida

Moon scannerbruter inicia una ola rápida generando un grupo limitado de hasta 200 gorutinas, cada una intentando conectarse al servidor con contraseñas diferentes. En caso de limitación de velocidad, la gorutina se une a una cola de ola lenta. Si el resultado indica una contraseña inválida o limitación de velocidad, la gorutina cede su posición, permitiendo que se unan otras gorutinas con contraseñas diferentes. Al descubrir la contraseña correcta, no se permiten nuevas gorutinas y la ola lenta se cancela. Si la ola rápida finaliza sin la contraseña correcta y con contraseñas en la cola lenta, la ola lenta se inicia.

```sirena
gráfico TD
subgráfico Moon scannerBruter
A[rapidwave] -->|Genera Grupo| B[gorutina grupo 200 tasa]
B -->|1| C[contraseña123]
B -->|2| D[contraseñadébil]
B -->|3| E[Emily]
B -->|4| I[123456]
B -->|...| F[...]
B -->|200| G[bolas]
C -->|velocidad limitada| I1[cola de onda lenta]
I -->|velocidad limitada| I1[cola de onda lenta]

fin

Servidor del subgrafo
H[Objetivo]
C -->|Se conecta a| H
D -->|Se conecta a| H
E -->|Se conecta a| H
F -->|Se conecta a| H
G -->|Se conecta a| H
I -->|Se conecta a| H

fin
```

### onda lenta
Slowwave está diseñado para validar contraseñas obtenidas de una fuente con velocidad limitada mediante un método tradicional de fuerza bruta de SSH. Intenta sistemáticamente cada contraseña una por una, determinando su corrección y confirmando su validez. El proceso implica acceder a la cola lenta, donde se almacenan las posibles contraseñas. Al recuperar una contraseña, Slowwave establece una conexión con el objetivo. Si la contraseña es correcta, procesa el resultado. En caso de una contraseña incorrecta, el ciclo continúa reintentando contraseñas a través de la cola lenta, hasta que no se encuentra ninguna.

```sirena
grafo TD
subgrafo Moon scannerBruter
A[Slowwave] -->|acceso| B[cola lenta]
B -->|Si está vacío| O[Termina]
B -->|recibir contraseña| C[contraseña]
C -->|conecta al objetivo| D[objetivo]
D -->|Correcto| E[Contraseña encontrada]
E --> O
D -->|Incorrecto| B
Fin
```
## REFERENCIA

Información del sistema
```
CPU: Intel i7-12700H (20) de 12.ª generación a 2,688 GHz
Núcleos: 14
SO: Arquitectura
Arquitectura: AMD64
RAM: 16 GB
Ancho de banda: 100 Mbps
```

### Referencia: Diccionario de 10 000 pasadas ¡Ataque! [imagen](https://i.ibb.co/WNTnjSY9/332048560-11d7a186-490a-4e1c-9047-d8a754a2208d.png)
