
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

Ahora este espacio de trabajo ya está listo para poder correr las aplicaciones desarrolladas. (Aunque todavía no tiene una aplicación que se pueda ejecutar... :sweat_smile:) (...pero para eso sirven los paquetes)

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

### Estructura del paquete

Para entender un poco más esta estructura de organización, vamos a examinar qué es lo que hace cada parte del paquete:

`CMake` es una herramienta que se utiliza para gestionar el proceso de compilación del software y que `colcon` utiliza para compilar los paquetes. En el archivo `CMakeLists.txt` se definen las reglas y configuraciones para compilar, enlazar y generar un proyecto. Es como una "receta" que le dice a CMake cómo configurar el proceso de construcción.

Por otro lado, el archivo `package.xml` contiene todos los metadatos acerca del paquete que estamos creando. Aquí podemos encontrar la información acerca del paquete: el **nombre**, la **versión**, la **descripción** de qué es lo que hace, la **licencia** que usa, sus **dependencias** con otros paquetes, etc, etc.\
De este modo tú y cualquier otro desarrollador de software puede saber qué hace y cómo incluir el paquete a diferentes proyectos.

Por otra parte, el directorio `/src` es una estructura convencional usan los desarrolladores y que contiene todos los archivos fuente necesarios para construir el proyecto, de esta manera ayuda a mantener el proyecto organizado al separar el código fuente del resto de los archivos. Aquí irán nuestros archivos `.cpp` y `.py` de la apliación.

## Resumen

Para inicializar un workspace:

``` bash
mkdir -p ws_workspace/src
cd ws_workspace/src
source /opt/ros/jazzy/setup.bash
ros2 pkg create --build-type ament_cmake --license Apache-2.0 pkg_package
cd ..
colcon build
```

Y el resultado será un workspace compilado listo para trabajar:

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
```

¡Con todo esto, ya tenemos listo nuestro espacio de trabajo y tenemos las herramientas necesarias para desarrollar nuestra aplicación de robótica!

#### __#############__ Tutorial oficial de ROS2 __#############__

---

Esta información fue desarrollada gracias al tutorial de ROS2 (Jazzy) en su sitio oficial:

1. Tutorial de colcon: https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html
2. Creando un workspace: https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html