## **Guía para Mantener la Pantalla Activa en Distribuciones Basadas en Debian**

### **Requisitos Previos**
1. Tener instalado el paquete `x11-xserver-utils`, que incluye la herramienta `xset`. Si no está instalado, puedes hacerlo con:
   ```bash
   sudo apt update
   sudo apt install x11-xserver-utils
   ```

### **Pasos para Configurar xset**
1. **Desactivar el Apagado de Pantalla y el Salvapantallas:**
   Ejecuta los siguientes comandos en una terminal para desactivar la suspensión y el salvapantallas:
   ```bash
   xset s off           # Desactiva el salvapantallas
   xset -dpms           # Desactiva el modo de gestión de energía del monitor (DPMS)
   xset s noblank       # Evita que la pantalla se quede en blanco
   ```

2. **Confirmar que la Configuración es Correcta:**
   Verifica el estado de las configuraciones con:
   ```bash
   xset q
   ```
   En la salida, asegúrate de que las líneas relevantes indiquen:
   - **"Screen Saver: off"**
   - **"DPMS is Disabled"**

### **Hacer la Configuración Permanente**
Los comandos anteriores funcionan solo durante la sesión actual. Para que se apliquen automáticamente en cada inicio de sesión, sigue estos pasos:

1. **Editar el Archivo de Autoinicio del Usuario:**
   - Para entornos de escritorio como XFCE, LXDE o similares, agrega los comandos a tu archivo de autoinicio:
     ```bash
     nano ~/.xsessionrc
     ```
     Luego, agrega estas líneas:
     ```bash
     xset s off
     xset -dpms
     xset s noblank
     ```

   - En caso de que el escritorio use otro sistema de inicio, puedes usar el archivo `~/.bashrc`:
     ```bash
     nano ~/.bashrc
     ```
     Y agrega al final:
     ```bash
     if [[ $DISPLAY ]]; then
         xset s off
         xset -dpms
         xset s noblank
     fi
     ```

2. **Para Sistemas que Usan `lightdm`:**
   Si tu sistema utiliza `lightdm` (muy común en derivados de Debian), edita el archivo de configuración global:
   ```bash
   sudo nano /etc/lightdm/lightdm.conf
   ```
   Asegúrate de agregar o modificar estas líneas en la sección `[SeatDefaults]`:
   ```ini
   [SeatDefaults]
   display-setup-script=/etc/lightdm/xset.sh
   ```

   Luego, crea el archivo `/etc/lightdm/xset.sh`:
   ```bash
   sudo nano /etc/lightdm/xset.sh
   ```
   Contenido:
   ```bash
   #!/bin/bash
   xset s off
   xset -dpms
   xset s noblank
   ```
   Asegúrate de hacerlo ejecutable:
   ```bash
   sudo chmod +x /etc/lightdm/xset.sh
   ```

### **Comprobación Final**
1. **Reinicia tu Sesión o el Sistema:**
   Reinicia tu sesión gráfica o el equipo completo.

2. **Verifica el Estado de `xset`:**
   Una vez reiniciado, ejecuta:
   ```bash
   xset q
   ```
   Confirma que las configuraciones siguen mostrando:
   - **"Screen Saver: off"**
   - **"DPMS is Disabled"**

---

### **Notas Adicionales**
- Si estás utilizando un entorno minimalista como i3 o Openbox, puedes agregar los comandos de `xset` al archivo de configuración de inicio del gestor de ventanas, como `~/.config/i3/config` o `~/.config/openbox/autostart`.

- Si estás trabajando con un servidor sin interfaz gráfica, puedes desactivar el apagado de pantalla a través del kernel (opción `consoleblank=0` en el archivo de configuración de GRUB).