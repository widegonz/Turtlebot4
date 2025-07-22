# TurtleBot4 - Simulación

Este tutorial tiene como objetivo guiarte en el proceso de simulación del robot TurtleBot 4 utilizando Ignition Gazebo.

## Tabla de Contenidos

## Tabla de Contenidos

- [Objetivos del Tutorial](#objetivos-del-tutorial)
-  [Requisitos Previos](#requisitos-previos)  
1. [Instalación de Paquetes de Simulación](#1-instalación-de-paquetes-de-simulación)
    - Instalar herramientas de desarrollo útiles 
    - Instalar Ignition Gazebo (versión Fortress)
    - Instalar el paquete Debian del simulador 
    - Instalación desde código fuente (opcional)
2. [Lanzamiento de la Simulación en el Mundo `warehouse`](#2-lanzamiento-de-la-simulación-en-el-mundo-warehouse)  
    - 2.1 Lanzamiento en el Mundo `maze`
    - 2.2 Lanzamiento en el Mundo `depot`
    - Control desde Terminal
- [Ignition GUI Plugins](#ignition-gui-plugins)  
3. [Visualización del TurtleBot 4 en RViz2](#3-visualización-del-turtlebot-4-en-rviz2)  
    - Visualización del Modelo del Robot
    - Visualización de nodos con rqt_graph 
4. [¿Cómo visualizar el LiDAR en Gazebo?](#4-cómo-visualizar-el-lidar-en-gazebo)  
    - ¿Cómo cambiar el rango del RPLiDAR?  
5. [Generación de un Mapa usando el Paquete `slam_toolbox`](#5-generación-de-un-mapa-usando-el-paquete-slam_toolbox)  
    - Guardar el Mapa
    - Video de Referencia
6. [Referencias](#6-referencias)


## Objetivos del Tutorial

Este tutorial tiene los siguientes objetivos principales:

- Explicar el proceso de instalación de los paquetes necesarios para la simulación del TurtleBot 4.
- Guiar al usuario en el uso de Ignition Gazebo como entorno de simulación para el TurtleBot 4.
- Mostrar cómo lanzar escenarios de prueba como almacén, depósito y laberinto.
- Visualizar el modelo del TurtleBot 4 y sus sensores, incluyendo RPLIDAR, en RViz2 e Ignition Gazebo.
- Introducir al usuario a la generación de mapas mediante SLAM Toolbox.
- Proporcionar enlaces y recursos de referencia oficiales para continuar aprendiendo.

## Requisitos Previos

Contar con ROS 2 Humble instalado, así como los paquetes necesarios relacionados con la simulación del TurtleBot 4, los cuales se instalarán a medida que se avance en el tutorial.

## 1. Instalación de Paquetes de Simulación

El metapaquete `turtlebot4_simulator` contiene los paquetes necesarios para simular el TurtleBot 4 en el entorno Ignition Gazebo.  
Sigue los siguientes pasos cuidadosamente.

### Instalar herramientas de desarrollo útiles

```bash
sudo apt install ros-dev-tools
```

Instalar Ignition Gazebo (versión Fortress)

```bash
sudo apt-get update && sudo apt-get install wget
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" > /etc/apt/sources.list.d/gazebo-stable.list'
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
sudo apt-get update && sudo apt-get install ignition-fortress
```

Instalar el paquete Debian del simulador

```bash
sudo apt update
sudo apt install ros-humble-turtlebot4-simulator
```

La instalación del paquete ros-humble-turtlebot4-simulator es necesaria si deseas simular un robot TurtleBot 4 en tu sistema. Este paquete incluye los archivos y herramientas requeridos para la simulación del TurtleBot 4 en Ignition Gazebo, una plataforma ampliamente utilizada en robótica.

Instalación desde código fuente (opcional)
Si prefieres instalar el metapaquete desde el código fuente, realiza lo siguiente:

```bash
mkdir -p ~/turtlebot4_ws/src  
cd ~/turtlebot4_ws/src
git clone https://github.com/turtlebot/turtlebot4_simulator.git -b humble
```

Instalar dependencias
```bash
cd ~/turtlebot4_ws
rosdep install --from-path src -yi
```

Compilar los paquetes
```bash
source /opt/ros/humble/setup.bash
colcon build --symlink-install
```

## 2. Lanzamiento de la Simulación en el Mundo `warehouse`

### ⚠️ Nota importante

Al ejecutar la simulación por primera vez, es común encontrarse con un error relacionado con la solicitud de mundos: `"Requesting list of worlds"`.  
Puedes encontrar más información al respecto en el siguiente enlace:  
https://github.com/gazebosim/gz-sim/issues/38

Para evitar este error, ejecuta el siguiente comando en la terminal **antes de lanzar la simulación**:

```bash
export IGN_IP=127.0.0.1
```

Este comando establece la variable de entorno `IGN_IP` en `127.0.0.1`, resolviendo el conflicto de comunicación con Ignition.

### 🔧 Lanzamiento de los modelos estándar y lite

El archivo de lanzamiento `turtlebot4_ignition.launch.py` permite personalizar la simulación mediante diversos parámetros, como el modelo del robot o el entorno simulado.  
De forma predeterminada, se lanza el mundo `warehouse` con el modelo estándar de TurtleBot 4.

#### Modelo estándar:
```bash
cd ~/turtlebot4_ws    
ros2 launch turtlebot4_ignition_bringup turtlebot4_ignition.launch.py
```

[![Modelo estándar](https://github.com/user-attachments/assets/d3723def-668a-4372-8f40-dbae6c36676f)](https://github.com/user-attachments/assets/d3723def-668a-4372-8f40-dbae6c36676f)

#### Modelo lite:
```bash
ros2 launch turtlebot4_ignition_bringup turtlebot4_ignition.launch.py model:=lite
```

[![Modelo lite](https://github.com/user-attachments/assets/5066d70b-aebf-41c0-b9ce-5f216755e077)](https://github.com/user-attachments/assets/5066d70b-aebf-41c0-b9ce-5f216755e077)


## 2.1 Lanzamiento en el Mundo `maze`

En esta sección se utiliza el mismo archivo de lanzamiento, pero configurando el mundo `maze` como entorno de simulación.

#### Modelo estándar:
```bash
cd ~/turtlebot4_ws
ros2 launch turtlebot4_ignition_bringup turtlebot4_ignition.launch.py world:=maze
```

[![Mundo maze estándar](https://github.com/user-attachments/assets/69b63aad-3a5a-4771-b30f-da923870c6f3)](https://github.com/user-attachments/assets/69b63aad-3a5a-4771-b30f-da923870c6f3)

#### Modelo lite:
```bash
cd ~/turtlebot4_ws
ros2 launch turtlebot4_ignition_bringup turtlebot4_ignition.launch.py world:=maze model:=lite
```

[![Mundo maze lite](https://github.com/user-attachments/assets/873dbeef-d0e2-459d-ae7b-804b97005c9d)](https://github.com/user-attachments/assets/873dbeef-d0e2-459d-ae7b-804b97005c9d)


## 2.2 Lanzamiento en el Mundo `depot`

Ahora lanzamos la simulación en el entorno `depot` utilizando el mismo archivo de lanzamiento.

#### Modelo estándar:
```bash
cd ~/turtlebot4_ws
ros2 launch turtlebot4_ignition_bringup turtlebot4_ignition.launch.py world:=depot
```

[![Mundo depot estándar](https://github.com/user-attachments/assets/1e03f358-30c3-4de3-ac50-3953ad98efcb)](https://github.com/user-attachments/assets/1e03f358-30c3-4de3-ac50-3953ad98efcb)

#### Modelo lite:
```bash
cd ~/turtlebot4_ws
ros2 launch turtlebot4_ignition_bringup turtlebot4_ignition.launch.py world:=depot model:=lite
```

[![Mundo depot lite](https://github.com/user-attachments/assets/c43e7a5d-0677-429c-9043-9d3ceb49544c)](https://github.com/user-attachments/assets/c43e7a5d-0677-429c-9043-9d3ceb49544c)


## 💡 Nota sobre el control del TurtleBot 4

Existen múltiples formas de controlar el TurtleBot 4. En esta sección explicaremos dos de ellas:  
- Control mediante terminal  
- Control mediante interfaz gráfica (GUI) *(por ahora no incluida en este tutorial)*


### 🖥️ Control desde Terminal

Antes de enviar comandos de movimiento, es recomendable conocer los tópicos activos en ROS 2:

```bash
ros2 topic list
```

[![Listado de tópicos](https://github.com/user-attachments/assets/1edfeeef-fa6b-410c-90c8-4a9b3401773f)](https://github.com/user-attachments/assets/1edfeeef-fa6b-410c-90c8-4a9b3401773f)

Como se observa en la imagen anterior, utilizaremos el tópico `/cmd_vel` para enviar comandos de velocidad al TurtleBot 4.

Asegúrate de que la simulación esté corriendo (por ejemplo, con `turtlebot4_ignition.launch.py`) y luego ejecuta en otra terminal el siguiente comando para mover el robot:

```bash
ros2 topic pub /cmd_vel geometry_msgs/Twist '{linear: {x: 0.7 , y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 0.0}}'
```

> 🔁 Puedes modificar los valores `linear.x` y `angular.z` para controlar la dirección y rotación del robot.

[![Movimiento en línea recta](https://github.com/user-attachments/assets/1223d0d9-85ed-4184-848f-e594f7e05a5b)](https://github.com/user-attachments/assets/1223d0d9-85ed-4184-848f-e594f7e05a5b)  
[![Rotación angular](https://github.com/user-attachments/assets/adaeda72-a43d-4559-bb4e-c3af56896915)](https://github.com/user-attachments/assets/adaeda72-a43d-4559-bb4e-c3af56896915)


## Ignition GUI Plugins
En este caso, el control mediante la interfaz gráfica (GUI) es más sencillo que el uso de terminal.  
Simplemente debes establecer una velocidad lineal y angular desde la interfaz, presionar "play" y luego utilizar las flechas para mover el robot en la dirección deseada.

[![Control GUI](https://github.com/user-attachments/assets/1add8455-0e4c-4968-aadc-be69617a078d)](https://github.com/user-attachments/assets/1add8455-0e4c-4968-aadc-be69617a078d)


## 3. Visualización del TurtleBot 4 en RViz2

Para visualizar el TurtleBot 4 en RViz2, es necesario instalar el paquete `turtlebot4_desktop`.

### Instalar paquete Debian:
```bash
sudo apt update
sudo apt install ros-humble-turtlebot4-desktop
```

### Instalación desde código fuente:
```bash
cd ~/turtlebot4_ws/src
git clone https://github.com/turtlebot/turtlebot4_desktop.git -b humble
```

### Instalar dependencias:
```bash
cd ~/turtlebot4_ws
rosdep install --from-path src -yi
```

### Compilar paquetes:
```bash
source /opt/ros/humble/setup.bash
colcon build --symlink-install
```

### Ver el robot en RViz:
```bash
ros2 launch turtlebot4_viz view_robot.launch.py
```

[![RViz Robot](https://github.com/user-attachments/assets/14ae4efe-cfcc-40e6-a25e-d10d03946519)](https://github.com/user-attachments/assets/14ae4efe-cfcc-40e6-a25e-d10d03946519)

### Ver el modelo del robot:
```bash
ros2 launch turtlebot4_viz view_model.launch.py
```

**Modelo estándar**  
[![Modelo estándar](https://github.com/user-attachments/assets/ab98acdf-067a-4309-bf24-064a5d08898d)](https://github.com/user-attachments/assets/ab98acdf-067a-4309-bf24-064a5d08898d)

**Modelo Lite**  
[![Modelo lite](https://github.com/user-attachments/assets/86816512-ed3b-426a-abdf-60be49017039)](https://github.com/user-attachments/assets/86816512-ed3b-426a-abdf-60be49017039)

### ⚠️ Nota importante

Como se observa en las imágenes, existe un error en la visualización del modelo del robot en RViz.  
Este error se resolvió utilizando ROS 2 Humble, ya que en ROS Galactic había paquetes obsoletos que no serían actualizados. ROS Galactic ha llegado al final de su ciclo de vida.

## Visualización del Grafo de Nodos con `rqt_graph`

`rqt_graph` permite visualizar los nodos activos del sistema y sus conexiones.

```bash
ros2 run rqt_graph rqt_graph
```

Por ejemplo, al ejecutar la simulación en Ignition Gazebo, se muestran los siguientes nodos activos:

[![Rqt graph](https://github.com/user-attachments/assets/5d35b069-e4d6-4f5e-a0cc-d15487970735)](https://github.com/user-attachments/assets/5d35b069-e4d6-4f5e-a0cc-d15487970735)


## 4. ¿Cómo visualizar el LiDAR en Gazebo?

### ⚠️ Nota importante

Si tu equipo no dispone de una GPU, es necesario forzar el uso del renderizado por CPU. Esto se debe a que el sensor LiDAR en Ignition utiliza renderizado con GPU (`ign-rendering`) para detectar elementos en la escena. Ejecuta lo siguiente en la terminal **antes de iniciar la simulación**:

```bash
export LIBGL_ALWAYS_SOFTWARE=true
```

Esto permitirá al sensor operar correctamente sin necesidad de una tarjeta gráfica dedicada.

Más información:  
https://github.com/gazebosim/gz-sensors/issues/26  
https://robotics.stackexchange.com/questions/24883/turtlebot-4-simulation-rplidar-not-working

Primero, inicia la simulación:
```bash
ros2 launch turtlebot4_ignition_bringup ignition.launch.py 
```

Luego, haz clic en los tres puntos (parte superior derecha):
[![Botón opciones](https://github.com/user-attachments/assets/eff8883d-6335-456b-a504-d1508261bc9e)](https://github.com/user-attachments/assets/eff8883d-6335-456b-a504-d1508261bc9e)

Busca y selecciona la opción `Visualize Lidar`:
[![Visualizar lidar](https://github.com/user-attachments/assets/36a81193-b1a7-4bfa-a9c3-6878b78d0b25)](https://github.com/user-attachments/assets/36a81193-b1a7-4bfa-a9c3-6878b78d0b25)

Desplázate hasta la sección correspondiente y haz clic en "Refresh". Luego selecciona el LiDAR adecuado del desplegable:

[![Selección lidar](https://github.com/user-attachments/assets/d0cd6f7c-5eeb-4344-bd08-9a0dbef23a03)](https://github.com/user-attachments/assets/d0cd6f7c-5eeb-4344-bd08-9a0dbef23a03)

Es importante presionar el botón "Refresh":

[![Botón refresh](https://github.com/user-attachments/assets/46140f70-8192-433b-b693-107f8fe42254)](https://github.com/user-attachments/assets/46140f70-8192-433b-b693-107f8fe42254)

Si no ves el sensor LiDAR, asegúrate de que la simulación esté activa (el botón en la esquina inferior izquierda debe mostrar el símbolo de pausa).

## ¿Cómo cambiar el rango del RPLiDAR?

Debes modificar el parámetro `r_max` en el archivo URDF del sensor, ubicado en `rplidar.urdf.xacro` dentro del paquete `turtlebot4_description`.

Ejemplo con `r_max` en 1.0 m:

[![r_max = 1.0 m](https://github.com/user-attachments/assets/ccea6278-722a-4e86-b5a5-5c5f2ac237f1)](https://github.com/user-attachments/assets/ccea6278-722a-4e86-b5a5-5c5f2ac237f1)  
[![r_max config](https://github.com/user-attachments/assets/7024d932-0fb5-4854-afca-dfc6b6fe0f34)](https://github.com/user-attachments/assets/7024d932-0fb5-4854-afca-dfc6b6fe0f34)

Ejemplo con `r_max` en 12.0 m:

[![r_max = 12.0 m](https://github.com/user-attachments/assets/f2f441c1-2e47-4882-9e6a-7a3bb4926853)](https://github.com/user-attachments/assets/f2f441c1-2e47-4882-9e6a-7a3bb4926853)  
[![r_max config 12m](https://github.com/user-attachments/assets/b6c6040a-f6f4-434b-abeb-fd4a0319ff12)](https://github.com/user-attachments/assets/b6c6040a-f6f4-434b-abeb-fd4a0319ff12)


## 5. Generación de un Mapa usando el Paquete `slam_toolbox`

En esta sección, se realizará el mapeo de un área mientras se conduce el TurtleBot 4 utilizando técnicas de SLAM (Simultaneous Localization and Mapping).  
La creación del mapa en tiempo real se lleva a cabo con el paquete `slam_toolbox`, que viene preconfigurado por defecto en el TurtleBot 4.

### 📦 Instalación de `turtlebot4_navigation`

Primero, instala el paquete de navegación:
```bash
sudo apt install ros-humble-turtlebot4-navigation
```

### 🚀 Lanzamiento de SLAM

Se recomienda utilizar SLAM síncrono desde una PC remota para obtener un mapa de mayor resolución:
```bash
ros2 launch turtlebot4_navigation slam.launch.py
```

#### Opción alternativa (SLAM Asíncrono):
```bash
ros2 launch turtlebot4_navigation slam.launch.py sync:=false
```

### 🖼️ Lanzamiento de RViz2

Para visualizar el mapa generado en tiempo real, lanza RViz2 con el archivo `view_robot.launch.py`:

```bash
ros2 launch turtlebot4_viz view_robot.launch.py
```

**Nota:** Asegúrate de tener corriendo Ignition Gazebo mientras RViz2 está activo, ya que esta simulación requiere ambos entornos:

```bash
ros2 launch turtlebot4_ignition_bringup turtlebot4_ignition.launch.py
```

### 💾 Guardar el Mapa

Una vez que estés satisfecho con el resultado del mapeo, puedes guardar el mapa con el siguiente comando:

```bash
ros2 service call /slam_toolbox/save_map slam_toolbox/srv/SaveMap "name:
  data: 'map_name'"
```

### 🎥 Video de Referencia

https://github.com/user-attachments/assets/bb17deda-5221-4067-817d-1ce7f774255c


## 6. Referencias

- Documentación oficial: https://turtlebot.github.io/turtlebot4-user-manual/overview/  
- Reporte de errores y comunidad: https://github.com/turtlebot/turtlebot4/issues  
- Referencia general de ROS: https://docs.ros.org/en/galactic/index.html
