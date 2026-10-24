# Ficha técnica: Mapeo de unidades de red mediante GPO

## 1. Objetivo

Configurar una directiva de grupo (GPO) para asignar automáticamente unidades de red a los usuarios del dominio `empresa.test`, según el departamento al que pertenecen.

De esta forma, cada usuario dispone de acceso directo a la carpeta compartida de su departamento al iniciar sesión, sin tener que conectarse manualmente.

## 2. Entorno

| Elemento | Configuración |
|---|---|
| Servidor de dominio | `SRV-DC01` |
| Dirección IP del servidor | `192.168.56.10` |
| Equipo cliente | `PC-CLI01` |
| Dominio | `empresa.test` |
| GPO | `GPO_Mapeo_Unidades` |
| Ubicación de la GPO | `Empresa/Usuarios` |

## 3. Configuración de la directiva

Se creó la GPO `GPO_Mapeo_Unidades` y se vinculó a la OU `Empresa/Usuarios`.

Dentro de la directiva se utilizó la siguiente ruta de configuración:

**Configuración de usuario → Preferencias → Configuración de Windows → Asignaciones de unidades (Drive Maps).**

Se configuró la acción **Actualizar** para las unidades de red y se activó la reconexión para que las unidades estuvieran disponibles en los siguientes inicios de sesión.

## 4. Unidades de red asignadas

| Departamento | Unidad | Ruta de red | Grupo autorizado |
|---|---|---|---|
| Administración | `H:` | `\\SRV-DC01\Administracion` | `GG_Administracion` |
| Ventas | `V:` | `\\SRV-DC01\Ventas` | `GG_Ventas` |

## 5. Destinatarios de nivel de elemento

Para que cada unidad se asigne únicamente a los usuarios autorizados, se configuraron los **Destinatarios de nivel de elemento** mediante grupos de seguridad.

- **Unidad H:** se asigna a los usuarios pertenecientes a `EMPRESA\GG_Administracion`.
- **Unidad V:** se asigna a los usuarios pertenecientes a `EMPRESA\GG_Ventas`.

Esta configuración permite aplicar cada asignación según la pertenencia del usuario al grupo correspondiente.

## 6. Pruebas de funcionamiento

Las pruebas se realizaron desde el equipo cliente `PC-CLI01`, iniciando sesión con cuentas del dominio.

| Usuario | Grupo | Resultado esperado |
|---|---|---|
| Ana García (`ana.garcia`) | `GG_Administracion` | Se asigna la unidad `H:` |
| Carlos López (`carlos.lopez`) | `GG_Ventas` | Se asigna la unidad `V:` |

Se comprobó que Ana recibía la unidad de Administración y Carlos la unidad de Ventas. Las unidades se correspondían con los grupos de seguridad de cada usuario.

## 7. Evidencias

### 7.1. Configuración de la GPO

La siguiente captura muestra la configuración de la directiva de grupo utilizada para el mapeo de las unidades de red.

![Configuración de la GPO de mapeo de unidades](../screenshots/W%20Server%20-%20Configuraci%C3%B3n%20de%20la%20GPO.png)

### 7.2. Comprobación desde el cliente

En el equipo cliente se verificó que cada usuario recibía la unidad de red correspondiente a su departamento.

![Carpeta compartida en el cliente](../screenshots/W%2011%20-%20carpeta%20compartida%20en%20cliente.png)

## 8. Conocimientos adquiridos

- Creación y vinculación de directivas de grupo (GPO).
- Configuración de unidades de red mediante preferencias de usuario.
- Uso de los destinatarios de nivel de elemento.
- Aplicación de asignaciones según grupos de seguridad de Active Directory.
- Verificación de directivas y recursos compartidos desde un equipo cliente.
