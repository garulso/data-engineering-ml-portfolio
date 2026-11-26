# Qué es WSL2 y por qué Ubuntu para Data Science

## ¿Qué problema resuelve?
Permite ejecutar un entorno Linux nativo (Ubuntu) dentro de Windows sin la sobrecarga de una máquina virtual tradicional ni los problemas de compatibilidad de dual-boot. Proporciona acceso real a herramientas de Data Science (Docker Engine, Spark, PyTorch/CUDA) que están optimizadas para Linux, permitiendo usar Windows solo para navegar o editar con VS Code Remote-WSL.

## ¿Cuándo falla?
- Cuando se trabaja con archivos alojados en el disco de Windows (`/mnt/c/`) en lugar del sistema de archivos nativo de Linux (`~/`), lo que causa lentitud extrema y errores de permisos POSIX.
- Cuando WSL2 consume toda la memoria RAM del host porque no se ha configurado un límite en el archivo `.wslconfig` de Windows.
- Cuando se intenta instalar el driver de NVIDIA dentro de Ubuntu: el driver va en Windows y WSL2 lo expone a través del kernel.

## Código mínimo clave
Desde PowerShell (Windows):
wsl --status
# Salida esperada: Default Version: 2 / Default Distribution: Ubuntu

Desde Ubuntu (WSL2):
uname -r
# Salida esperada: 6.18.58.1-microsoft-standard-WSL2 (o similar)

cat /etc/os-release | head -2
# Salida esperada: PRETTY_NAME="Ubuntu 26.04.1 LTS"

free -h
# Salida esperada: Muestra la RAM (ej. total 7.7Gi si está limitada por .wslconfig)

## Explicación a stakeholder no técnico
WSL2 es como tener un ordenador Linux virtual dentro de tu ordenador Windows, pero sin la lentitud de una máquina virtual tradicional. Comparte el disco y la memoria de forma muy eficiente. Sirve para que las herramientas pesadas de análisis de datos funcionen a máxima velocidad sin que tengas que cambiar de sistema operativo.

## Cómo se lo enseñaría a un estudiante
Imagina que Windows es tu oficina con escritorio, silla y café. Linux es el taller donde están las herramientas pesadas. WSL2 es una puerta corredera que conecta ambos: entras al taller desde la oficina sin salir a la calle. Trabajar en `/mnt/c` sería llevar las herramientas del taller a tu escritorio: funciona, pero estorba y va lento. Lo correcto es trabajar dentro del taller (`~`).

## Definiciones y conceptos clave
- **WSL2:** Windows Subsystem for Linux versión 2. Usa un kernel Linux real gestionado por Microsoft, con virtualización ligera.
- **DrvFs:** Sistema de archivos que monta discos de Windows (`/mnt/c`, `/mnt/d`) dentro de WSL. Es más lento que ext4 nativo.
- **`.wslconfig`:** Archivo de configuración global de WSL2 en Windows (`C:\Users\<usuario>\.wslconfig`). Limita RAM, swap, procesadores.
- **Kernel Linux:** Núcleo del sistema operativo. En WSL2 no se actualiza con `apt upgrade`; se actualiza con `wsl --update` desde PowerShell.

## Temas [PROFUNDIZAR]
1. Filesystem de Linux y WSL2. Fuente: Microsoft Learn (learn.microsoft.com/windows/wsl). Criterio: explicar diferencia de rendimiento entre `~/` y `/mnt/c/` y demostrarlo con `time`.
2. Configuración de `.wslconfig`. Fuente: Microsoft Learn WSL config. Criterio: limitar RAM a 8 GB y swap a 8 GB, verificar con `free -h`.
