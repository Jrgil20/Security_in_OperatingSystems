# Implementación de la seguridad en sistemas operativos

En este documento, se abordarán las diferentes estrategias y técnicas para implementar la seguridad en sistemas operativos. La seguridad es un aspecto crítico en el diseño y operación de sistemas informáticos, y es esencial para proteger los datos y recursos de los usuarios.

## Contenidos

1. **Introducción a la seguridad en sistemas operativos**
2. **Mecanismos de control de acceso**
3. **Autenticación y autorización**
4. **Seguridad en el manejo de archivos**
5. **Protección de la memoria**
6. **Seguridad en la red**
7. **Actualizaciones y parches de seguridad**
8. **Monitoreo y auditoría de seguridad**
9. **Casos de estudio y mejores prácticas**

## 1. Introducción a la seguridad en sistemas operativos

La seguridad en sistemas operativos se refiere a las medidas y controles implementados para proteger el sistema y los datos que maneja. Esto incluye la protección contra accesos no autorizados, la integridad de los datos y la disponibilidad del sistema.

## 2. Mecanismos de control de acceso

Los mecanismos de control de acceso son fundamentales para garantizar que solo los usuarios autorizados puedan acceder a los recursos del sistema. Estos mecanismos incluyen:

- **Listas de control de acceso (ACL)**
- **Modelos de control de acceso basado en roles (RBAC)**
- **Modelos de control de acceso obligatorio (MAC)**

### Implementación práctica de mecanismos de control de acceso

La implementación de mecanismos de control de acceso en la práctica puede variar dependiendo del sistema operativo y del entorno específico. A continuación, se presentan algunos ejemplos de cómo se pueden implementar estos mecanismos:

#### Listas de control de acceso (ACL)

Las ACL permiten definir permisos detallados para usuarios y grupos específicos sobre archivos y directorios. En sistemas Unix/Linux, se pueden gestionar utilizando comandos como `setfacl` y `getfacl`. En Windows, se pueden configurar a través de la interfaz gráfica o mediante comandos como `icacls`.

```bash
# Ejemplo en Linux
setfacl -m u:usuario:rwx archivo.txt
getfacl archivo.txt
```

```powershell
# Ejemplo en Windows
icacls archivo.txt /grant usuario:(OI)(CI)F
icacls archivo.txt
```

#### Control de acceso basado en roles (RBAC)

RBAC asigna permisos a roles en lugar de a usuarios individuales. Esto simplifica la gestión de permisos en entornos grandes. En sistemas como Kubernetes, RBAC se configura mediante archivos YAML.

```yaml
# Ejemplo en Kubernetes
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    namespace: default
    name: pod-reader
rules:
- apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "watch", "list"]
```

#### Control de acceso obligatorio (MAC)

MAC impone políticas de seguridad que no pueden ser modificadas por usuarios individuales. En Linux, SELinux y AppArmor son ejemplos de sistemas MAC.

```bash
# Ejemplo de SELinux
sudo setenforce 1
sudo semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"
sudo restorecon -R /web
```

Estos ejemplos ilustran cómo se pueden aplicar diferentes mecanismos de control de acceso en la práctica para mejorar la seguridad de los sistemas operativos.

## 3. Autenticación y autorización

La autenticación es el proceso de verificar la identidad de un usuario, mientras que la autorización determina los permisos que tiene el usuario autenticado. Los métodos comunes de autenticación incluyen:

- **Contraseñas**
- **Autenticación de dos factores (2FA)**
- **Certificados digitales**

### Implementación de autenticación y autorización

La autenticación y autorización son procesos esenciales para la seguridad en sistemas operativos. A continuación, se describen algunos métodos y técnicas utilizados:

#### Autenticación

1. **Contraseñas**: El método más común de autenticación. Las contraseñas deben ser robustas y almacenadas de forma segura utilizando técnicas de hashing como bcrypt o SHA-256.
   
   ```bash
   # Ejemplo de generación de hash de contraseña en Linux
   echo -n "password" | sha256sum
   ```

2. **Autenticación de dos factores (2FA)**: Añade una capa adicional de seguridad al requerir un segundo factor, como un código enviado al teléfono móvil del usuario.
   
   ```bash
   # Ejemplo de configuración de 2FA en Linux con Google Authenticator
   sudo apt-get install libpam-google-authenticator
   google-authenticator
   ```

3. **Certificados digitales**: Utilizados para la autenticación basada en certificados, donde se verifica la identidad del usuario mediante un certificado emitido por una autoridad de certificación (CA).

   ```bash
   # Ejemplo de generación de certificado en OpenSSL
   openssl req -new -x509 -days 365 -key private.key -out certificate.crt
   ```

#### Autorización

1. **Listas de control de acceso (ACL)**: Permiten definir permisos detallados para usuarios y grupos específicos sobre archivos y directorios.

   ```bash
   # Ejemplo en Linux
   setfacl -m u:usuario:rwx archivo.txt
   getfacl archivo.txt
   ```

2. **Control de acceso basado en roles (RBAC)**: Asigna permisos a roles en lugar de a usuarios individuales, simplificando la gestión de permisos en entornos grandes.

   ```yaml
   # Ejemplo en Kubernetes
   apiVersion: rbac.authorization.k8s.io/v1
   kind: Role
   metadata:
     namespace: default
     name: pod-reader
   rules:
   - apiGroups: [""]
     resources: ["pods"]
     verbs: ["get", "watch", "list"]
   ```

3. **Control de acceso obligatorio (MAC)**: Impone políticas de seguridad que no pueden ser modificadas por usuarios individuales. En Linux, SELinux y AppArmor son ejemplos de sistemas MAC.

   ```bash
   # Ejemplo de SELinux
   sudo setenforce 1
   sudo semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"
   sudo restorecon -R /web
   ```

#### Criptografía en autenticación y autorización

La criptografía juega un papel crucial en la autenticación y autorización. Los sistemas operativos utilizan técnicas criptográficas para proteger las credenciales de los usuarios y asegurar la comunicación.

1. **Hashing de contraseñas**: Las contraseñas se almacenan como hashes en lugar de texto plano para evitar que sean fácilmente recuperadas en caso de una brecha de seguridad.

2. **Cifrado de datos**: Los datos sensibles, como las credenciales de autenticación, se cifran utilizando algoritmos como AES para protegerlos durante la transmisión y el almacenamiento.

3. **Firmas digitales**: Utilizadas para verificar la integridad y autenticidad de los datos y las comunicaciones, asegurando que no han sido alterados.

#### Identificación de usuarios root

En sistemas operativos Unix/Linux, el usuario root tiene un UID (User ID) de 0. Los sistemas operativos verifican el UID del usuario para determinar si tiene privilegios de root.

```bash
# Comprobación del UID del usuario actual
id -u
```

Si el comando `id -u` devuelve 0, el usuario tiene privilegios de root.

Estos métodos y técnicas aseguran que solo los usuarios autenticados y autorizados puedan acceder a los recursos del sistema, protegiendo así la integridad y seguridad del sistema operativo.

## 4. Seguridad en el manejo de archivos

La seguridad en el manejo de archivos implica proteger los archivos del sistema contra accesos no autorizados y modificaciones. Esto se puede lograr mediante:

- **Permisos de archivo**
- **Cifrado de archivos**
- **Copias de seguridad**

### Implementación de la seguridad en el manejo de archivos

La seguridad en el manejo de archivos es crucial para proteger la integridad y confidencialidad de los datos almacenados en un sistema operativo. A continuación, se describen algunas técnicas y mecanismos utilizados en Linux y Windows para asegurar los archivos:

#### Permisos de archivo

En sistemas Unix/Linux, los permisos de archivo se gestionan mediante un modelo de permisos que incluye lectura (r), escritura (w) y ejecución (x) para el propietario del archivo, el grupo y otros usuarios. Estos permisos se pueden modificar utilizando comandos como `chmod`.

```bash
# Ejemplo de modificación de permisos en Linux
chmod 750 archivo.txt
```

En Windows, los permisos de archivo se gestionan a través de la interfaz gráfica o mediante comandos como `icacls`.

```powershell
# Ejemplo de modificación de permisos en Windows
icacls archivo.txt /grant usuario:(R,W)
```

#### Cifrado de archivos

El cifrado de archivos asegura que los datos solo puedan ser leídos por usuarios autorizados. En Linux, herramientas como `gpg` y `ecryptfs` se utilizan para cifrar archivos y directorios.

```bash
# Ejemplo de cifrado de archivo en Linux
gpg -c archivo.txt
```

En Windows, el Sistema de Archivos Cifrados (EFS) permite cifrar archivos y carpetas para protegerlos.

```powershell
# Ejemplo de cifrado de archivo en Windows
cipher /e archivo.txt
```

#### Copias de seguridad

Las copias de seguridad son esenciales para recuperar datos en caso de pérdida o corrupción. En Linux, herramientas como `rsync` y `tar` se utilizan para crear copias de seguridad.

```bash
# Ejemplo de creación de copia de seguridad en Linux
tar -czvf backup.tar.gz /ruta/a/archivos
```

En Windows, se pueden utilizar herramientas como el Historial de archivos y la Copia de seguridad de Windows para crear copias de seguridad.

```powershell
# Ejemplo de creación de copia de seguridad en Windows
wbadmin start backup -backupTarget:D: -include:C: -allCritical -quiet
```

#### Restricciones de particiones

En algunos sistemas Linux, las particiones pueden estar configuradas para restringir el acceso desde otros sistemas operativos. Esto se puede lograr mediante el uso de sistemas de archivos específicos y configuraciones de montaje.

```bash
# Ejemplo de montaje de partición con restricciones en Linux
mount -o ro,nosuid,nodev /dev/sda1 /mnt
```

En Windows, las particiones suelen ser accesibles desde otros sistemas operativos, a menos que se utilicen sistemas de archivos específicos como NTFS con configuraciones de permisos restrictivas.

Estas técnicas y mecanismos ayudan a asegurar que los archivos en un sistema operativo estén protegidos contra accesos no autorizados y modificaciones, garantizando así la integridad y confidencialidad de los datos.

## 5. Protección de la memoria

La protección de la memoria es crucial para evitar que procesos maliciosos accedan a áreas de memoria que no les corresponden. Las técnicas incluyen:

- **Segmentación**
- **Paginación**
- **Protección de pila y heap**

### Implementación de la protección de la memoria

La protección de la memoria es fundamental para evitar que procesos maliciosos accedan a áreas de memoria que no les corresponden. Las técnicas más comunes incluyen la segmentación y la paginación.

#### Segmentación

La segmentación divide la memoria en segmentos de diferentes tamaños, cada uno con su propio propósito (código, datos, pila, etc.). Cada segmento tiene permisos específicos que controlan el acceso.

```c
// Ejemplo de configuración de un segmento en C
struct segment_descriptor {
    unsigned int base;
    unsigned int limit;
    unsigned char type;
};

struct segment_descriptor code_segment = {0x00000000, 0x000FFFFF, 0x9A}; // Código ejecutable
struct segment_descriptor data_segment = {0x00100000, 0x001FFFFF, 0x92}; // Datos de lectura/escritura
```

#### Paginación

La paginación divide la memoria en páginas de tamaño fijo y utiliza una tabla de páginas para gestionar el acceso. Cada entrada en la tabla de páginas contiene información sobre los permisos y la ubicación de la página en la memoria física.

```c
// Ejemplo de configuración de una tabla de páginas en C
unsigned int page_table[1024] __attribute__((aligned(4096)));

void setup_page_table() {
    for (int i = 0; i < 1024; i++) {
        page_table[i] = (i * 0x1000) | 3; // Dirección base de la página | permisos (lectura/escritura)
    }
}
```

### Prevención de vulnerabilidades

Aunque la segmentación y la paginación son efectivas, pueden ser vulneradas mediante técnicas avanzadas como ataques de desbordamiento de búfer o explotación de vulnerabilidades en el kernel. Para mitigar estos riesgos, se implementan medidas adicionales:

1. **ASLR (Address Space Layout Randomization)**: Aleatoriza las direcciones de memoria asignadas a los procesos, dificultando la predicción de la ubicación de segmentos y páginas.

2. **Protección de pila y heap**: Utiliza canarios de pila y técnicas de protección de heap para detectar y prevenir desbordamientos de búfer.

3. **NX (No-eXecute) Bit**: Marca regiones de memoria como no ejecutables, evitando la ejecución de código inyectado en áreas de datos.

4. **SMEP (Supervisor Mode Execution Prevention)**: Previene que el código en modo kernel ejecute código en modo usuario, protegiendo contra ciertas clases de exploits.

Estas técnicas combinadas refuerzan la protección de la memoria y ayudan a prevenir accesos no autorizados y vulnerabilidades.

## 6. Seguridad en la red

La seguridad en la red implica proteger los datos que se transmiten a través de la red. Las medidas incluyen:

- **Firewalls**
- **Sistemas de detección de intrusos (IDS)**
- **Cifrado de datos en tránsito**
 
## 7. Actualizaciones y parches de seguridad

Mantener el sistema operativo actualizado con los últimos parches de seguridad es esencial para protegerlo contra vulnerabilidades conocidas.

## 8. Monitoreo y auditoría de seguridad

El monitoreo y la auditoría de seguridad permiten detectar y responder a incidentes de seguridad. Las herramientas incluyen:

- **Sistemas de información y gestión de eventos de seguridad (SIEM)**
- **Registros de auditoría**

## Auditoría simple en Linux

La auditoría de seguridad en Linux se puede realizar utilizando el demonio `auditd`, que es parte del paquete `audit`. A continuación se describen los pasos básicos para configurar y realizar una auditoría simple en Linux:

### Instalación de auditd

Primero, asegúrate de que `auditd` esté instalado en tu sistema. En la mayoría de las distribuciones de Linux, puedes instalarlo utilizando el gestor de paquetes correspondiente.

```bash
# En Debian/Ubuntu
sudo apt-get install auditd

# En CentOS/RHEL
sudo yum install audit

# En Fedora
sudo dnf install audit
```

### Configuración de auditd

El archivo de configuración principal de `auditd` es `/etc/audit/auditd.conf`. Puedes editar este archivo para ajustar la configuración según tus necesidades. Sin embargo, para una auditoría simple, la configuración predeterminada suele ser suficiente.

### Definición de reglas de auditoría

Las reglas de auditoría se definen en el archivo `/etc/audit/audit.rules`. A continuación se muestra un ejemplo de cómo auditar accesos a un archivo específico:

```bash
# Auditar accesos al archivo /etc/passwd
-w /etc/passwd -p wa -k passwd_changes
```

En este ejemplo:
- `-w /etc/passwd` especifica el archivo a auditar.
- `-p wa` indica que se auditarán los eventos de escritura (write) y de atributo (attribute) del archivo.
- `-k passwd_changes` es una clave que se puede utilizar para identificar los eventos en los registros de auditoría.

### Inicio del servicio auditd

Una vez configurado, inicia el servicio `auditd` y habilítalo para que se ejecute al inicio del sistema.

```bash
sudo systemctl start auditd
sudo systemctl enable auditd
```

### Visualización de registros de auditoría

Los eventos de auditoría se registran en el archivo `/var/log/audit/audit.log`. Puedes utilizar el comando `ausearch` para buscar eventos específicos en los registros.

```bash
# Buscar eventos relacionados con la clave passwd_changes
sudo ausearch -k passwd_changes
```

### Ejemplo de auditoría

A continuación se muestra un ejemplo de cómo auditar la ejecución de un comando específico, como `useradd`:

```bash
# Auditar la ejecución del comando useradd
-a always,exit -F arch=b64 -S execve -F path=/usr/sbin/useradd -k useradd_exec
```

En este ejemplo:
- `-a always,exit` especifica que se auditarán siempre las salidas de las llamadas al sistema.
- `-F arch=b64` indica la arquitectura del sistema (64 bits en este caso).
- `-S execve` especifica la llamada al sistema a auditar.
- `-F path=/usr/sbin/useradd` indica la ruta del comando a auditar.
- `-k useradd_exec` es una clave para identificar los eventos en los registros de auditoría.

Estos pasos te permitirán realizar una auditoría simple en Linux utilizando `auditd`. Puedes ajustar y ampliar las reglas de auditoría según tus necesidades específicas de seguridad.

## Auditoría de seguridad en Windows

La auditoría de seguridad en Windows es una herramienta poderosa que permite mantener la integridad del sistema. Como parte de una estrategia de seguridad integral, es importante determinar el nivel de auditoría adecuado para el entorno. La auditoría debe identificar ataques (exitosos o no) que representen una amenaza para la red y ataques contra recursos valiosos según la evaluación de riesgos.

### Configuración de la auditoría de seguridad

Para configurar la auditoría de seguridad en Windows, se pueden seguir los siguientes pasos:

1. **Acceder a la Política de Seguridad Local**:
    - Abre el menú de inicio y busca "Política de seguridad local".
    - Navega a "Configuración de seguridad" > "Políticas locales" > "Política de auditoría".

2. **Definir las políticas de auditoría**:
    - En la ventana de "Política de auditoría", selecciona las categorías de eventos que deseas auditar, como "Inicio de sesión", "Acceso a objetos", "Cambios de política", etc.
    - Configura las opciones de auditoría para cada categoría, eligiendo entre "Éxito", "Error" o ambos.

3. **Configurar auditoría avanzada**:
    - Para una configuración más detallada, utiliza la "Política de auditoría avanzada" en "Configuración de seguridad" > "Políticas avanzadas de auditoría".
    - Aquí puedes definir subcategorías específicas y reglas detalladas para la auditoría.

### Visualización de registros de auditoría

Los eventos de auditoría se registran en el Visor de eventos de Windows. Para acceder a estos registros:

1. **Abrir el Visor de eventos**:
    - Abre el menú de inicio y busca "Visor de eventos".
    - Navega a "Registros de Windows" > "Seguridad".

2. **Filtrar y analizar eventos**:
    - Utiliza las opciones de filtrado para buscar eventos específicos.
    - Analiza los eventos para identificar posibles amenazas y actividades sospechosas.

### Ejemplo de auditoría de inicio de sesión

Para auditar los intentos de inicio de sesión en un sistema Windows:

1. **Configurar la política de auditoría**:
    - En "Política de auditoría", habilita la auditoría para "Inicio de sesión" y selecciona "Éxito" y "Error".

2. **Revisar los eventos de inicio de sesión**:
    - En el Visor de eventos, filtra los eventos de seguridad por el ID de evento 4624 (inicio de sesión exitoso) y 4625 (inicio de sesión fallido).

### Beneficios de la auditoría de seguridad

La auditoría de seguridad en Windows ofrece varios beneficios, incluyendo:

- **Detección de intrusiones**: Identificación de intentos de acceso no autorizados.
- **Cumplimiento normativo**: Aseguramiento de que las políticas de seguridad cumplen con las regulaciones.
- **Análisis forense**: Provisión de información detallada para investigar incidentes de seguridad.
- **Mejora de la seguridad**: Identificación de vulnerabilidades y áreas de mejora en la seguridad del sistema.

Implementar una auditoría de seguridad efectiva es crucial para proteger los recursos y datos de la organización, y para mantener un entorno de red seguro y gestionable.

## 9. Casos de estudio y mejores prácticas

En esta sección se presentarán casos de estudio y mejores prácticas para la implementación de la seguridad en sistemas operativos.

## Funciones de seguridad del núcleo de Linux

Para cualquier administrador de sistemas Linux que pretenda mantener un entorno seguro y fiable, es esencial conocer el funcionamiento interno del núcleo Linux y sus funciones de seguridad. Linux cuenta con varias características de seguridad del núcleo que lo diferencian de otros sistemas operativos, contribuyendo a su mayor seguridad.

### Aislamiento del espacio de usuario y del núcleo

La separación del espacio de usuario y el espacio del kernel impide que las aplicaciones de nivel de usuario dañen el kernel y otros procesos o interfieran con ellos. También se impide el acceso no autorizado si el código del espacio de usuario intenta acceder directamente al espacio del núcleo.

### Protección de memoria

El núcleo Linux aplica la protección de memoria a través de las unidades de gestión de memoria (MMU). La MMU asigna a cada proceso una zona de memoria virtual, que los mantiene separados entre sí. La MMU provoca un error de segmentación si un proceso intenta acceder a una memoria que no forma parte de la región que le ha sido asignada, protegiendo así contra cualquier fallo de seguridad.

### Permisos de usuario

Linux implementa un sólido modelo de permisos que proporciona a los usuarios y procesos diferentes niveles de acceso. Los elementos clave de esta arquitectura son la propiedad de los archivos y los permisos. Estos permisos son aplicados por el núcleo, impidiendo que usuarios y grupos no autorizados accedan a archivos y carpetas.

### ASLR

Una técnica de seguridad incorporada, Address Space Layout Randomization (ASLR) defiende el sistema contra ataques de desbordamiento de búfer. Cada vez que el sistema arranca, asigna aleatoriamente el área de direcciones de memoria de los procesos en ejecución, lo que dificulta a los piratas informáticos predecir dónde se utilizará la memoria para ejecutar código malicioso.

### Auditoría del núcleo

Linux proporciona la capacidad de auditar la actividad del kernel mediante el uso de programas como auditd, permitiendo la monitorización de eventos del kernel relacionados con la seguridad. La auditoría del núcleo ayuda a supervisar los cambios del sistema, la actividad de los usuarios y las posibles brechas de seguridad cuando se configura correctamente.

### Arranque seguro

El arranque seguro, que garantiza que sólo el software firmado y de confianza, incluido el núcleo, pueda ejecutarse durante el proceso de arranque, está soportado por un gran número de sistemas Linux contemporáneos. Al impedir que los rootkits y los bootkits interfieran en el proceso de arranque, esta característica los defiende de ellos.

### Seccomp

El Modo de Computación Segura es una potente técnica de seguridad que limita el número de llamadas al sistema que un proceso puede realizar. Disminuye drásticamente la superficie de ataque del núcleo, reduciendo el efecto de las debilidades potenciales en ciertas funciones del sistema.

### Módulo de seguridad Linux

LSM (Linux Security Module) ofrece un marco que permite la integración de módulos de seguridad adicionales en el kernel de Linux. Estos módulos pueden aumentar las funciones de seguridad del núcleo y proporcionar a los administradores un mayor control sobre la restricción de acceso y la aplicación de políticas.

## Seguridad en Windows: Seguridad del sistema

### Arranque seguro y arranque de confianza

El arranque seguro y el arranque de confianza ayudan a evitar que el malware y los componentes dañados se carguen cuando se inicia un dispositivo.

El arranque seguro se inicia con la protección de arranque inicial y, a continuación, el arranque de confianza recoge el proceso. Juntos, el arranque seguro y el arranque de confianza ayudan a garantizar que el sistema arranque de forma segura.

### Arranque medido

Arranque medido mide todas las opciones de configuración y código importantes durante el arranque de Windows. Esto incluye: el firmware, el administrador de arranque, el hipervisor, el kernel, el kernel seguro y el sistema operativo. Arranque medido almacena las medidas en el TPM en la máquina y las pone a disposición en un registro que se puede probar de forma remota para comprobar el estado de arranque del cliente.

La característica de arranque medido proporciona software antimalware con un registro de confianza (resistente a la suplantación y la alteración) de todos los componentes de arranque que se iniciaron antes. El software antimalware puede usar el registro para determinar si los componentes que se ejecutaron antes son de confianza o si están infectados con malware. El software antimalware del equipo local puede enviar el registro a un servidor remoto para su evaluación. El servidor remoto puede iniciar acciones de corrección, ya sea interactuando con el software en el cliente o a través de mecanismos fuera de banda, según corresponda.

### Servicio de atestación de estado del dispositivo

El proceso de atestación de estado del dispositivo Windows admite un paradigma de confianza cero que cambia el foco de perímetros estáticos basados en la red a usuarios, recursos y recursos. El proceso de atestación confirma que el dispositivo, el firmware y el proceso de arranque están en un buen estado y no se han alterado antes de que puedan acceder a los recursos corporativos. Las determinaciones se realizan con los datos almacenados en el TPM, que proporciona una raíz segura de confianza. La información se envía a un servicio de atestación, como Azure Attestation, para comprobar que el dispositivo se encuentra en un estado de confianza. A continuación, una herramienta MDM como Microsoft Intune revisa el estado del dispositivo y conecta esta información con Microsoft Entra ID para el acceso condicional.

### Configuración y auditoría de directivas de seguridad de Windows

Microsoft proporciona un sólido conjunto de directivas de configuración de seguridad que los administradores de TI pueden usar para proteger los dispositivos Windows y otros recursos de su organización.

### Acceso asignado

Algunos dispositivos de escritorio de una empresa tienen un propósito especial. Por ejemplo, un equipo en la sala de espera que los clientes usan para ver el catálogo de productos. O bien, un equipo que muestra contenido visual como una señal digital. El cliente de Windows ofrece dos experiencias de bloqueo diferentes para uso público o especializado: un quiosco de una sola aplicación que ejecuta una sola aplicación de Plataforma universal de Windows (UWP) en pantalla completa encima de la pantalla de bloqueo o un quiosco de varias aplicaciones que ejecuta una o varias aplicaciones desde el escritorio.

Las configuraciones de quiosco multimedia se basan en acceso asignado, una característica de Windows que permite a un administrador administrar la experiencia del usuario limitando los puntos de entrada de la aplicación expuestos al usuario.