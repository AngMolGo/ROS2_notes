cmake_minimum_required(VERSION 3.8)
project(pkg_pub)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

# 1. Agregamos las dependencias necesarias que necesitan nuestros programas.
find_package(rclcpp REQUIRED)     # "Busca el paquete 'rclcpp' que utiliza uno de mis programas para funcionar"
find_package(rclpy REQUIRED)     # "Busca el paquete 'rclcpp' que utiliza uno de mis programas para funcionar"
find_package(std_msgs REQUIRED)   # "Busca el paquete 'std_msgs' que utiliza uno de mis programas para funcionar"

# 2. Agregamos los ejecutables para que "$ ros2 run" pueda correrlos
# OJO: SOLO LOS ARCHIVOS CPP GENERAN EJECUTABLES (los archivos .py ejecutan el sript directamente, no necesitan ejecutable)
add_executable(talker src/minimal_publisher.cpp)      # "Agrega un ejecutable llamado 'talker' que ejecuta el código 'src/minimal_publisher.cpp'"
ament_target_dependencies(talker rclcpp std_msgs)     # "El ejecutable 'talker' utiliza los paquetes antes mencionados 'rclcpp' y 'std_msgs'"

# 3. Instalamos los ejecutables (cpp.) creados y los almacenamos en lib/${PROJECT_NAME} de la instalación del paquete
install(
  TARGETS talker                      # "Estos son los ejecutables que vas a instalar en la instalación del paquete."
  DESTINATION lib/${PROJECT_NAME}     # "Los vas a instalar en este directorio de la instalación del paquete."
  )

# 3.1 Instalamos el script de Python como ejecutable directamente
install(
  PROGRAMS src/minimal_suscriber.py
  DESTINATION lib/${PROJECT_NAME}
)

# ###### --> OPCIONAL <-- ######
# if(BUILD_TESTING)
#   find_package(ament_lint_auto REQUIRED)
#   # the following line skips the linter which checks for copyrights
#   # comment the line when a copyright and license is added to all source files
#   set(ament_cmake_copyright_FOUND TRUE)
#   # the following line skips cpplint (only works in a git repo)
#   # comment the line when this package is in a git repo and when
#   # a copyright and license is added to all source files
#   set(ament_cmake_cpplint_FOUND TRUE)
#   ament_lint_auto_find_test_dependencies()
# endif()

ament_package()
