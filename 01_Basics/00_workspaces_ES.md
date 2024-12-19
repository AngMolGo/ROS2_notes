
<!-- Nota que indica los requisitos de instalación de ROS2 y colcon. -->
> [!NOTE]
> Para continuar con este tutorial es necesario haber instalado previamente [**_ROS2_**](https://docs.ros.org/en/jazzy/Installation/Alternatives/Ubuntu-Development-Setup.html) y el gestor de paquetes [**_colcon_**](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html#install-colcon).



## ¿Qué es un espacio de trabajo? (workspace)

Un workspace (o espacio de trabajo) es una forma de organizar los entornos de desarrollo de un proyecto. Consiste de mantener todos los archivos necesarios de un proyecto en un único directorio, administrados en un sistema de carpetas, manteniendo un orden lógico que facilita el desarrollo de la aplicación. 

Para crear un workspace basta con crear un directorio:

``` bash
$ mkdir ws_workspace    #Creamos un directorio para el ws
```

¡Listo!, ya hemos creado nuestro directorio (workspace) que va a albergar la organizción de nuestra aplicación.

En `ROS2`, los espacios de trabajo están conformados por 4 carpetas principales: `/build`, `/install`, `/log` y `/src`. Para crear estas carpetas es necesario usar una herramienta que usa `ROS2` para gestionar sus archivos y paquetes llamado ``colcon``.



## ¿Qué es Colcon?

**_Colcon_** es un gestor de paquetes por defecto de `ROS2`. Es una herramienta de construcción que compila los paquetes instalados en el workspace para poder ser usados en la aplicación. **_Colcon_** construye cada paquete de forma aislada, asegurando que los errores en un paquete no afecten a otros. De esta manera, evalúa las dependencias entre paquetes y construye en el orden correcto.

Para inicializar un workspace con su gestión de paquetes se empieza por generar su directorio `/src` y posteriormente compilar el entorno desde el directorio raíz:

``` bash
$ cd ws_workspace       # Cambiamos a la ruta del ws
$ mkdir src             # Creamos la carpeta /src
$ colcon build          # Compilamos el ws
```

Hasta ahora, `colcon build` creó 4 directorios en el espacio de trabajo:

```
ws_worspace/
├── build
├── install
├── log
└── src
```

Ahora este espacio de trabajo ya está listo para poder correr las aplicaciones desarrolladas. (Aunque todavía no tenemos una aplicación que se pueda ejecutar... :sweat_smile:). 

Ahora falta agregar un _**paquete**_ donde podamos desarrollar nuestros códigos.

## ¿Qué es un paquete? (package)

Los paquetes en `ROS2` es un manera de mantener el código organizado y de manera modular. La modularidad permite organizar mejor el código y permite compartirlo con otros desarrolladores e incorporarlo en nuevos códigos de manera fácil y rápida.

Los paquetes deben de tener la siguiente estructura básica para que `colcon` pueda considerarlo e identificarlo como un paquete:

``` bash
my_package/
├── CMakeLists.txt
├── include/my_package/
├── package.xml
└── src/
```

Los paquetes se crean, descargan o almacenan en el directorio `/src` del espacio de trabajo y es aqui donde el compilador busca los paquetes para poder integrarlos a la aplicación.

Para cargar una plantilla básica para nuestro paquete, primero debemos inicializar nuestra instalación de `ROS2` y posteriormente usar el comando `ros2 pkg create` para poder cargar una plantilla nueva:

``` bash
$ cd src        # Cambiamos al directorio donde se almacenan los paquetes 
$ source /opt/ros/jazzy/setup.bash  # Inicializamos ROS2 Jazzy
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 pkg_package # Creamos un paquete nuevo
```

De esta manera creamos un directorio nuevo con la estructura básica (CMake) para paquetes:

``` bash
$ tree pkg_package/ # Desplegamos el directorio del paquete
pkg_package/
├── CMakeLists.txt
├── include
│   └── pkg_package
├── LICENSE
├── package.xml
└── src

4 directories, 3 files
```

### Estructura de un paquete

Para entender un poco más esta estructura de organización, vamos a examinar qué es lo que hace cada parte del paquete:

`CMake` es una herramienta que se utiliza para gestionar el proceso de compilación del software y que `colcon` utiliza para compilar los paquetes. En el archivo `CMakeLists.txt` se definen las reglas y configuraciones para compilar, enlazar y generar un proyecto. Es como una "receta" que le dice a CMake cómo configurar el proceso de construcción.

Por otro lado, el archivo `package.xml` contiene todos los metadatos acerca del paquete que estamos creando. Aquí podemos encontrar la información acerca del paquete: el **nombre**, la **versión**, la **descripción** de qué es lo que hace, la **licencia** que usa, sus **dependencias** con otros paquetes, etc, etc.\
De este modo tú y otros desarrolladores de software puede saber qué hace y cómo incluir el paquete a diferentes proyectos.

Por otra parte, el directorio `/src` es una estructura convencional que usan los desarrolladores y que contiene todos los archivos fuente necesarios para construir el proyecto, de esta manera ayuda a mantener el proyecto organizado al separar el código fuente del resto de los archivos. En resumen, en este directorio irá nuestro código.

## Mi primer `¡Hola mundo!` en `ROS2`

Los dos lenguajes de programación por defecto para desarrollar nodos y paquetes en `ROS2` son: C++ y Python. Existen ligeras diferencias al momento de compilar y ejecutar los códigos de cada lenguaje.

### Python

Por un lado, Python es un lenguaje interpretado que ejecuta los programas directamente desde el código fuente, sin necesidad de un proceso de compilación previo. Esto permite desarrollar y probar aplicaciones de forma rápida y sencilla. Sin embargo, es importante asegurarse de que los archivos fuente tengan los permisos de ejecución adecuados antes de ejecutarlos, ya que el intérprete requiere acceso directo al código.

<!-- Python -->
<details>
  <summary > Pasos a seguir para ejecutar código hecho en Python </summary>

**1.** Nos vamos al directorio /src de nuestro paquete creado y vamos a crear un archivo `.py`:

``` bash
$ cd pkg_package/src
$ touch hola_mundo.py
```

Con tu editor de textos favorito copia el siguiente script en el archivo creado:

``` python
#!/usr/bin/env python3

if __name__ == '__main__':
    print("¡Hola mundo desde python!")
```

> [!IMPORTANT]
> Es importante que mencionar que antes de compilar el paquete, se tienen que cambiar los permisos de ejecución de los archivos `.py` desarrollados para que el compilador y el comando `ros2 run` puedan ejecutar el paquete sin problemas.
> 
> Se utiliza el comando `chmod +x <archivo>.py`.
> ``` bash
> chmod +x hola_mundo.py
> ```
>---

**2.** Para que el compilador pueda procesar el archivo creado, se tiene que declarar las dependencias del código y las instrucciones de procesamiento en los archivos `package.xml` y `CMakeLists.txt` del paquete.

Por ahora, nuestro script no utiliza ninguna dependencia por otro paquete para ejecutar su código. Es por eso que, por ahora, no vamos a agregar nada al archivo `package.xml`.

En el archivo `CMakeLists.txt`, antes de la instrucción `ament_package()`, se integrará la siguiente instrucción:
``` CMake
# Instalamos el script de Python como ejecutable directamente
install(
  PROGRAMS src/hola_mundo.py
  DESTINATION lib/${PROJECT_NAME}
)
```

Esta instrucción le permite a `ros2 run` reconocer al archivo `hola_mundo.py` como un programa ejecutable del paquete.

**3.** Para ejecutar el código hay que compilar el paquete desde el directorio raíz, luego cargar las variables de entorno y por último ejecutar el programa con `ros2 run`:

``` bash
$ cd ../../..   # Cambiamos a ws_workspace
$ colcon build  # Compilamos el paquete
$ . install/setup.bash # Cargamos las variables entorno
$ ros2 run pkg_package hola_mundo.py
```


Y tendremos la salida de nuestro programa desarrollado en python.

``` bash
¡Hola mundo desde python!
```
---
</details>

### C++

Por otro lado, C++ es un lenguaje compilado, lo que significa que el código fuente debe transformarse en un archivo binario ejecutable mediante un proceso de compilación antes de poder ser ejecutado. Este paso adicional genera un programa altamente optimizado, adecuado para tareas que requieren un alto rendimiento. En ROS2, este proceso se gestiona con herramientas como _**colcon build**_, lo que permite compilar proyectos completos de manera eficiente y estructurada.

<!-- C++ -->
<details>
  <summary> Pasos a seguir para ejecutar código hecho en C++ </summary>

**1.** Nos vamos al directorio /src de nuestro paquete creado y vamos a crear un archivo `.cpp`:

``` bash
$ cd pkg_package/src
$ touch hola_mundo.cpp
```

Con tu editor de textos favorito copia el siguiente script en el archivo creado:

``` cpp
#include <iostream>

int main() {
    std::cout << "¡Hola mundo desde C++!" << std::endl;
    return 0;
}

```

**2.** Para que el compilador pueda procesar el archivo creado, se tiene que declarar las dependencias del código y las instrucciones de procesamiento en los archivos `package.xml` y `CMakeLists.txt` del paquete.

Por ahora, nuestro código no utiliza ninguna dependencia por otro paquete para ejecutarse. Es por eso que, por ahora, no vamos a agregar nada al archivo `package.xml`.
 
En el archivo `CMakeLists.txt` se tienen que agregar instruciones importantes que le indican a `colcon` que tiene que compilar nuestro archivo `hola_mundo.cpp`, crear un ejecutable y luego agregarlo a la lista de ejecutables del paquete, y esto se integrará de acuerdo a las siguientes instrucciones:
``` CMake
add_executable(wellcome src/hola_mundo.cpp)  # "Agrega un ejecutable llamado 'wellcome' que ejecuta el código 'src/hola_mundo.cpp'"

install(
  TARGETS wellcome  # "Estos son los ejecutables que vas a instalar en la instalación del paquete."
  DESTINATION lib/${PROJECT_NAME}  # "Los vas a instalar en este directorio de la instalación del paquete."
  )
```

Estas instrucciones le permiten a `ros2 run` reconocer el ejecutable `wellcome`, ejecutable del archivo `hola_mundo.cpp`, como un ejecutable del paquete.

**3.** Para ejecutar el código hay que compilar el paquete desde el directorio raíz, luego cargar las variables de entorno y por último ejecutar el programa con `ros2 run`:

``` bash
$ cd ../../..   # Cambiamos a ws_workspace
$ colcon build  # Compilamos el paquete
$ . install/setup.bash # Cargamos las variables entorno
$ ros2 run pkg_package wellcome
```


Y tendremos la salida de nuestro programa desarrollado en python.

``` bash
¡Hola mundo desde C++!
```

---

</details>

En ROS2, los pasos de agregar `add_executable()` e `install(TARGETS DESTINATION)` en el archivo CMakeLists.txt son fundamentales para procesar y registrar adecuadamente los programas desarrollados en C++ y python.

El comando `add_executable()` es el responsable de indicar al compilador qué archivo fuente debe convertirse en un ejecutable (únicamente para archivos `.cpp`), asignándole un nombre único que será utilizado posteriormente. Por otro lado, el comando `install(TARGETS DESTINATION)` asegura que el ejecutable generado sea instalado en la ubicación adecuada dentro del sistema de archivos del paquete, permitiendo que herramientas como ros2 run puedan localizarlo y ejecutarlo correctamente.

## Resumen

#### Inicializar un workspace:

``` bash
mkdir -p ws_workspace/src       # Creamos el workspace
cd ws_workspace/src
source /opt/ros/jazzy/setup.bash    # Inicializamos ROS2
ros2 pkg create --build-type ament_cmake --license Apache-2.0 pkg_package   # Creamos un nuevo paquete
cd pkg_packege/src
```

---

#### Desarrollar código

<!-- C++ -->
<details>
  <summary> C++ </summary>

Desarrollamos nuestro código:

``` bash
touch hola_mundo.cpp
nano hola_mundo.cpp
```

Agregamos las instrucciones de instalación en el archivo `CMakeLists.txt`:

``` CMake
add_executable(wellcome src/hola_mundo.cpp)  # "Agrega un ejecutable llamado 'wellcome' que ejecuta el código 'src/hola_mundo.cpp'"

install(
  TARGETS wellcome  # "Estos son los ejecutables que vas a instalar en la instalación del paquete."
  DESTINATION lib/${PROJECT_NAME}  # "Los vas a instalar en este directorio de la instalación del paquete."
  )
```

</details>

<!-- Python -->
<details>
  <summary> Python </summary>

Desarrollamos nuestro código:

``` bash
touch hola_mundo.py
nano hola_mundo.py # Agregar al archivo "#!/usr/bin/env python3" en el encabezado del script
chmod +x hola_mundo.py # Cambiamos los permisos del archivo
```

Agregamos las instrucciones de instalación en el archivo `CMakeLists.txt`:

``` CMake
# Instalamos el script de Python como ejecutable directamente
install(
  PROGRAMS src/hola_mundo.py
  DESTINATION lib/${PROJECT_NAME}
)
```

</details>

---

#### Compilar el workspace

Se compila el workspace, cargamos variables de entorno y se ejecuta el código:

``` bash
cd ../../..             # Regresamos al directorio raíz
colcon build            # Compilamos el ws
. install/setup.bash    # Cargamos variables de entorno
ros2 run pkg_package hola_mundo.py  # Ejecutar script de python
ros2 run pkg_package wellcome       # Ejecutar aplicación de C++
```

Y el resultado será un workspace compilado listo para ejecutar sus aplicaciones instaladas:

```
ws_worspace/
├── build
├── install
├── log
└── src
    └── pkg_package
        ├── CMakeLists.txt
        ├── include
        │   └── pkg_package
        ├── LICENSE
        ├── package.xml
        └── src
            ├── Hola_mundo.py
            └── Hola_mundo.cpp
```

¡Con todo esto, ya tenemos listo nuestro espacio de trabajo y tenemos las herramientas necesarias para desarrollar nuestros códigos de ROS2!

---

\
__#############__ Tutorial oficial de ROS2 __#############__

Esta información fue desarrollada gracias al tutorial de ROS2 (Jazzy) en su sitio oficial:

1. Tutorial de colcon: https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html
2. Creando un workspace: https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html