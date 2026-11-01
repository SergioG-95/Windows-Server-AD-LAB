# Monitorización y supervisión de Windows Server

## 1. Objetivo

Supervisar el estado y el rendimiento del servidor Windows Server, comprobar el funcionamiento de los servicios principales y revisar los registros de eventos para identificar posibles incidencias.

## 2. Herramientas utilizadas

- **Administrador de tareas:** consulta del consumo de CPU, memoria y procesos.
- **Servicios (`services.msc`):** comprobación del estado y tipo de inicio de los servicios.
- **Visor de eventos (`eventvwr.msc`):** revisión de errores y advertencias del sistema y las aplicaciones.
- **Monitor de rendimiento (`perfmon`):** seguimiento de contadores de rendimiento.

## 3. Comprobación de recursos del sistema

Desde el Administrador de tareas, en la pestaña **Rendimiento**, se revisó el consumo de recursos del servidor.

En el momento de la comprobación se observaron aproximadamente:

- Memoria RAM utilizada: 2 GB de los 4 GB asignados a la máquina virtual.
- Procesador: consumo variable, sin observar una carga elevada sostenida.

También se revisaron los procesos activos para identificar los que utilizaban más memoria.

## 4. Comprobación de servicios

Desde la consola de servicios se comprobó el estado de los servicios principales del dominio:

| Servicio | Estado | Tipo de inicio |
|---|---|---|
| Active Directory Domain Services | En ejecución | Automático |
| DNS Server | En ejecución | Automático |

Ambos servicios se encontraban funcionando correctamente durante la revisión.

## 5. Monitor de rendimiento

Se utilizó `perfmon` para añadir y consultar los siguientes contadores:

| Objeto | Contador | Instancia |
|---|---|---|
| Procesador | % de tiempo de procesador | _Total |
| Memoria | MBytes disponibles | — |
| Disco físico | % de tiempo de disco | _Total |

En una de las mediciones se obtuvieron estos valores:

| Contador | Valor observado |
|---|---:|
| Uso del procesador | 5,669 % |
| Memoria disponible | 1.794 MB |
| Tiempo de disco | 0,000 % |

Estos datos representan una medición puntual y no el comportamiento permanente del servidor.

![Monitor de rendimiento de Windows Server](../screenshots/W%20Server%20-%20Monitor%20de%20rendimiento.png)

## 6. Revisión del Visor de eventos

Se revisaron los registros de Windows, especialmente **Sistema** y **Aplicación**, para localizar errores y advertencias.

### 6.1. Registro Sistema: DNS Client Events, ID 1014

Se identificó un evento relacionado con un tiempo de espera agotado al intentar resolver el nombre externo `go.microsoft.com`.

Se realizaron pruebas de resolución DNS:

- `srv-dc01.empresa.test`: resolución correcta hacia `192.168.56.10`.
- `go.microsoft.com`: consulta sin respuesta.

**Diagnóstico:** la resolución DNS interna del dominio funciona. La consulta externa no se resuelve porque la red Host-only utilizada en VirtualBox no dispone de salida a Internet ni de una puerta de enlace configurada.

Esta limitación es propia de la configuración aislada del laboratorio.

### 6.2. Registro Aplicación: Perflib, ID 1016

Se encontró una advertencia relacionada con la biblioteca `dnsperf.dll`, utilizada por los contadores de rendimiento del servicio DNS.

El evento indica que el procedimiento de recopilación devuelve un puntero que no está alineado en un límite de 8 bytes, lo que puede ocasionar problemas al leer determinados datos de rendimiento.

Se observaron tres eventos: uno reciente y dos de semanas anteriores.

**Diagnóstico:** no se detectó un fallo del servicio DNS ni de la resolución interna. Se deja el evento en observación, sin modificar la biblioteca.

### 6.3. Registro Aplicación: AppModel-State, ID 23 y 24

Se encontraron eventos relacionados con la reparación de ubicaciones de estado de aplicaciones de Windows.

Los paquetes afectados fueron:

- `Microsoft.Windows.StartMenuExperienceHost`
- `Microsoft.Windows.ShellExperienceHost`

El código `-2147024894` corresponde al error `0x80070002`, que indica que no se encuentra el archivo especificado.

**Diagnóstico:** son eventos relacionados con componentes de la interfaz de Windows. No se observaron problemas funcionales al iniciar sesión ni al utilizar el menú Inicio y el escritorio. No se realizaron cambios porque no había síntomas que justificasen una reparación.

## 7. Conclusiones

Durante esta práctica se han realizado tareas básicas de monitorización y supervisión de Windows Server:

- Consulta del consumo de CPU, memoria y procesos.
- Comprobación del estado de los servicios Active Directory y DNS.
- Configuración y consulta de contadores de rendimiento.
- Revisión de los registros del sistema y las aplicaciones.
- Identificación y diagnóstico de advertencias sin impacto funcional observado.

La práctica permite familiarizarse con herramientas habituales de administración de sistemas y aprender a distinguir entre incidencias que requieren intervención y advertencias que conviene mantener bajo observación.

**Nota:** los valores de rendimiento corresponden a una medición puntual del laboratorio y los eventos descritos se han evaluado dentro de la configuración virtual utilizada.
