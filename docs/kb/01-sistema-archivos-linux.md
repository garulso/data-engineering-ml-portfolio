# Sistema de archivos Linux (WSL2 Ubuntu)

## ¿Qué problema resuelve?
Permite organizar, localizar y manipular archivos sin interfaz gráfica dentro de Linux. En WSL2, además, distingue entre el filesystem nativo (ext4 virtual, rápido) y los discos de Windows montados (`/mnt/c`, DrvFs, lento). Saber esto evita pérdidas de rendimiento y errores de permisos.

## ¿Cuándo falla?
- Trabajar en `/mnt/c/...` en lugar de `~/...`: los permisos POSIX se emulan y el rendimiento cae drásticamente en operaciones con muchos archivos (git status, instalación de paquetes, tests).
- Usar `rm -rf` sin verificar `pwd`: borrado irreversible.
- Confundir rutas relativas (`docs/`) con absolutas (`~/data-engineering-ml-portfolio/docs/`).
- Permisos incorrectos en llaves SSH (`600` para privada, `644` para pública); `777` es un agujero de seguridad.

## Código mínimo clave
```bash
pwd                              # /home/rsilva
cd ~/data-engineering-ml-portfolio
mkdir -p docs/kb modules/M0/tests
touch docs/kb/.gitkeep
chmod 600 ~/.ssh/id_ed25519      # privada: solo dueño lee/escribe
chmod 644 ~/.ssh/id_ed25519.pub  # pública: todos leen, dueño escribe
ls -la docs/kb
# Salida esperada:
# drwxr-xr-x ... .
# drwxr-xr-x ... ..
# -rw-r--r-- ... .gitkeep
