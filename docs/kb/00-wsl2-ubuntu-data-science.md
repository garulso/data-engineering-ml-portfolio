# Qué es WSL2 y por qué Ubuntu para Data Science

## ¿Qué problema resuelve?
Permite ejecutar un entorno Linux nativo (Ubuntu) dentro de Windows sin la sobrecarga de una máquina virtual tradicional ni los problemas de compatibilidad de dual-boot, proporcionando acceso a herramientas de Data Science (Docker, Spark, PyTorch) que solo están optimizadas para Linux.

## ¿Cuándo falla?
Cuando se instalan herramientas de Data Science directamente en Windows o cuando se trabaja con archivos alojados en el disco de Windows (`/mnt/c/`) en lugar del sistema de archivos nativo de Linux (`~/`), lo que causa lentitud extrema y errores de permisos.

## Código mínimo clave
```bash
# Verificar versión de WSL y kernel (desde PowerShell y Ubuntu)
wsl --list --verbose
uname -r
# Salida esperada: Ubuntu en VERSION 2, kernel terminando en microsoft-standard-WSL2
