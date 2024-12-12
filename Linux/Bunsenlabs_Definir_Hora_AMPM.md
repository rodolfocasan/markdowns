### **Guía para Configurar el Reloj en Formato AM/PM en BunsenLabs con Tint2**

Esta guía te llevará paso a paso para configurar el reloj del panel **Tint2** en formato de 12 horas (AM/PM) y reiniciar Tint2 para aplicar los cambios.


### **1. Editar la configuración de Tint2**

1. **Abrir el archivo de configuración de Tint2**:
   - Abre un terminal y escribe:
     ```bash
     nano ~/.config/tint2/tint2rc
     ```
   - Si no encuentras el archivo, búscalo con:
     ```bash
     find ~/.config -name "tint2rc"
     ```

2. **Modificar el formato del reloj**:
   - Busca la línea donde se define el formato del reloj. Generalmente es algo como:
     ```bash
     time1_format = %H:%M
     ```
   - Cambia el formato al de 12 horas con AM/PM:
     - **Para minúsculas (am/pm)**:
       ```bash
       time1_format = %l:%M %P
       ```
     - **Para mayúsculas (AM/PM)**:
       ```bash
       time1_format = %l:%M %p
       ```

3. **Guardar los cambios**:
   - Si usas `nano`, presiona `Ctrl + O` para guardar y `Ctrl + X` para salir.

---

### **2. Reiniciar Tint2**

Tint2 no reconoce la opción `-R` para recargar la configuración en algunas versiones. Debes reiniciarlo manualmente:

1. **Detener el proceso actual de Tint2**:
   En el terminal, escribe:
   ```bash
   pkill tint2
   ```

2. **Iniciar Tint2 nuevamente**:
   Para cargar el archivo de configuración actualizado:
   ```bash
   tint2
   ```

   Si estás usando un archivo de configuración en una ubicación personalizada, utiliza:
   ```bash
   tint2 -c /ruta/a/tu/configuración/tint2rc
   ```

---

### **3. Verificar el Formato del Reloj**

- **Códigos importantes del formato del reloj**:
  - `%l`: Hora en formato 12 horas (sin ceros a la izquierda).
  - `%M`: Minutos con dos dígitos.
  - `%P`: AM/PM en minúsculas.
  - `%p`: AM/PM en mayúsculas.

- **Ejemplo final**:
  Si deseas que el reloj muestre algo como **11:06 PM**, tu configuración debería ser:
  ```bash
  time1_format = %l:%M %p
  ```

---

### **4. Solución de Problemas Comunes**

- **Error al cargar el archivo de configuración**:
  Si Tint2 muestra un mensaje como:
  ```
  tint2: Could not read config file.
  ```
  Asegúrate de que:
  - El archivo `tint2rc` existe y es legible.
  - Estás usando la ruta correcta al reiniciar Tint2.

- **El reloj no cambia al formato esperado**:
  Revisa que hayas guardado correctamente el archivo de configuración y hayas reiniciado Tint2.