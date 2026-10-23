# Laboratorio de Active Directory en Windows Server

## 🎯 Objetivo del proyecto

Desplegar un entorno de administración de sistemas en máquinas virtuales utilizando Windows Server y Windows 11.

El objetivo es centralizar la gestión de usuarios y grupos mediante Active Directory, configurar la resolución de nombres con DNS, compartir carpetas mediante SMB y aplicar permisos para separar el acceso a los recursos de diferentes departamentos.

Este proyecto se ha desarrollado como práctica personal de aprendizaje del ciclo formativo de Administración de Sistemas Informáticos en Red (ASIR).

---

## 🗺️ Arquitectura del laboratorio

El entorno está compuesto por dos máquinas virtuales conectadas mediante una red *Host-only* de VirtualBox.

| Máquina | Nombre | Dirección IPv4 | Función |
|---|---|---|---|
| Servidor | `SRV-DC01` | `192.168.56.10` | Controlador de dominio, DNS y servidor de archivos |
| Cliente | `PC-CLI01` | `192.168.56.20` | Equipo Windows 11 unido al dominio |

### Configuración de red

- **Virtualización:** Oracle VirtualBox.
- **Tipo de red:** Host-only.
- **Red:** `192.168.56.0/24`.
- **Máscara de subred:** `255.255.255.0`.
- **Dominio:** `empresa.test`.
- **DNS del cliente:** `192.168.56.10`.
- **Puerta de enlace:** no configurada en esta red aislada.

El servidor actúa como controlador de dominio y proporciona el servicio DNS necesario para localizar los servicios de Active Directory.

---

## 🛠️ Tecnologías utilizadas

- Oracle VirtualBox.
- Windows Server.
- Windows 11 Pro.
- Active Directory Domain Services (AD DS).
- DNS.
- SMB y recursos compartidos de Windows.
- Permisos NTFS.
- PowerShell.

---

## 🏢 Estructura de Active Directory

Se ha creado el dominio `empresa.test`, con el nombre NetBIOS `EMPRESA`.

Dentro del dominio se ha creado la unidad organizativa `Empresa`, organizada de la siguiente manera:

```text
empresa.test
└── Empresa
    ├── Usuarios
    │   ├── Ana García (ana.garcia)
    │   └── Carlos López (carlos.lopez)
    ├── Equipos
    └── Grupos
        ├── GG_Administracion
        └── GG_Ventas
```

### Grupos de seguridad

Se han creado dos grupos globales de seguridad para gestionar los permisos por departamento:

| Grupo | Finalidad |
|---|---|
| `GG_Administracion` | Gestionar el acceso a los recursos de Administración |
| `GG_Ventas` | Gestionar el acceso a los recursos de Ventas |

Los usuarios se asignan a sus respectivos grupos, evitando tener que configurar individualmente los permisos de cada carpeta para cada cuenta.

---

## 📁 Carpetas compartidas y permisos

En el servidor se han creado las siguientes carpetas:

```text
C:\EmpresaDatos
├── Administracion
└── Ventas
```

Se han publicado como recursos compartidos SMB para permitir el acceso desde el cliente del dominio.

| Departamento | Ruta de red | Grupo autorizado |
|---|---|---|
| Administración | `\\SRV-DC01\Administracion` | `GG_Administracion` |
| Ventas | `\\SRV-DC01\Ventas` | `GG_Ventas` |

### Configuración de permisos

Se han configurado dos niveles de permisos:

- **Permisos NTFS:** los grupos departamentales tienen permisos de modificación sobre sus respectivas carpetas.
- **Permisos del recurso compartido SMB:** se han concedido permisos de cambio y lectura al grupo correspondiente.
- **Administración del servidor:** se mantienen los permisos necesarios para SYSTEM y los administradores.

El acceso efectivo depende de los permisos del recurso compartido y de los permisos NTFS.

---

## Asignación automática de unidades de red mediante GPO

### Objetivo

Configurar la asignación automática de unidades de red según el grupo de seguridad al que pertenece cada usuario del dominio `empresa.test`.

### Configuración

Se ha creado la directiva `GPO_Mapeo_Unidades`, vinculada a la OU `Empresa/Usuarios`.

| Unidad | Recurso compartido | Grupo de seguridad |
| --- | --- | --- |
| H: | `\\SRV-DC01\Administracion` | `GG_Administracion` |
| V: | `\\SRV-DC01\Ventas` | `GG_Ventas` |

Las unidades se han configurado mediante **Configuración de usuario → Preferencias → Configuración de Windows → Asignaciones de unidades**, utilizando la acción `Actualizar` y destinatarios de nivel de elemento basados en grupos de seguridad.

### Pruebas realizadas

- La usuaria `ana.garcia` recibe la unidad H: de Administración.
- El usuario `carlos.lopez` recibe la unidad V: de Ventas.
- Se comprueba que cada usuario accede a su recurso compartido correspondiente.
- Se mantienen separados los accesos mediante grupos de seguridad y permisos de carpeta.

### Resultado

La asignación de unidades de red se realiza automáticamente al iniciar sesión, según la pertenencia del usuario a su grupo de seguridad.

### Evidencias

- Captura de la configuración de la GPO.
- Captura de las asignaciones de unidades H: y V:.
- Captura del Explorador de archivos de Ana.
- Captura del Explorador de archivos de Carlos.

---

## 🧪 Pruebas y validación

Después de configurar el servidor, se ha unido el equipo `PC-CLI01` al dominio `empresa.test`.

Se han realizado las siguientes comprobaciones:

- [x] Resolución DNS del servidor desde el cliente.
- [x] Consulta de los registros SRV de Active Directory.
- [x] Unión del equipo Windows 11 al dominio.
- [x] Inicio de sesión con usuarios del dominio.
- [x] Acceso de Ana García a la carpeta de Administración.
- [x] Comprobación de que Ana no puede acceder a Ventas.
- [x] Acceso de Carlos López a la carpeta de Ventas.
- [x] Comprobación de que Carlos no puede acceder a Administración.
- [x] Conexión de una unidad de red para facilitar el acceso a Administración desde el Explorador de archivos.

Las pruebas de acceso se han realizado desde el cliente Windows 11 para verificar que la configuración funciona desde el punto de vista del usuario.

---

## 📸 Evidencias del proyecto

Se incorporarán capturas de pantalla para documentar la configuración y los resultados de las pruebas.

Las evidencias se organizarán en la carpeta `screenshots/`:

- Arquitectura y configuración de red.
- Estructura de Active Directory y grupos.
- Cliente Windows 11 unido al dominio.
- Acceso autorizado a la carpeta de cada departamento.
- Acceso denegado a la carpeta del otro departamento.


---

## 📚 Conocimientos practicados

- Instalación y configuración básica de Windows Server.
- Gestión de dominios mediante Active Directory.
- Creación y organización de usuarios, unidades organizativas y grupos.
- Configuración y comprobación de DNS.
- Unión de equipos cliente a un dominio.
- Administración de recursos compartidos SMB.
- Diferenciación entre permisos NTFS y permisos de recursos compartidos.
- Aplicación del principio de acceso por grupos.
- Resolución de incidencias y validación de conectividad.
- Uso de PowerShell para consultar la configuración del sistema.

---

## 🚀 Mejoras futuras

- Configurar directivas de grupo (GPO).
- Automatizar la conexión de unidades de red según el usuario o departamento.
- Crear usuarios y grupos mediante scripts de PowerShell.
- Añadir más equipos clientes y probar la administración centralizada.
- Ampliar el laboratorio con servicios Linux y tecnologías de virtualización o cloud.

---

## 👨‍💻 Autor y contexto

Proyecto práctico personal desarrollado durante el primer curso del ciclo formativo de Grado Superior de Administración de Sistemas Informáticos en Red (ASIR).

El objetivo es adquirir experiencia práctica en administración de sistemas, redes, gestión de identidades y servicios de infraestructura.
