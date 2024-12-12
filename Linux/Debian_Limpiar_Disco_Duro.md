## **Guía para Limpiar Completamente Un Disco Duro en Distribuciones Basadas en Debian (se recomienda Live Distros)**


### 1. **Identificar las unidades conectadas**
```bash
lsblk
```
Este comando muestra un listado de los discos y particiones disponibles en el sistema. Es útil para identificar el disco que deseas limpiar (por ejemplo, `/dev/sda`). El listado incluye información como el tamaño, puntos de montaje y estructura jerárquica.

---

### 2. **Desmontar cualquier partición activa del disco**
```bash
sudo umount /dev/sda*
```
- **¿Por qué desmontar?** Antes de trabajar con un disco, especialmente para borrarlo, debes asegurarte de que ninguna partición esté montada ni siendo utilizada por el sistema.
- **`/dev/sda*`**: Este patrón desmonta todas las particiones asociadas al disco `/dev/sda` (por ejemplo, `/dev/sda1`, `/dev/sda2`, etc.).

---

### 3. **Eliminar particiones existentes**
```bash
sudo fdisk /dev/sda
```
`fdisk` es una herramienta interactiva para gestionar particiones. Aquí los pasos:

1. **Listar particiones existentes**: Al abrir `fdisk`, puedes usar `p` para listar las particiones del disco.
2. **Eliminar particiones**: Usa el comando `d` para eliminar una partición. Si hay varias, repite hasta borrar todas.
3. **Guardar cambios**: Escribe `w` para escribir los cambios en el disco y salir.

---

### 4. **Sobrescribir el disco con ceros (limpieza básica)**
```bash
dd if=/dev/zero of=/dev/sda bs=1M status=progress
```
- **`if=/dev/zero`**: Especifica como entrada un flujo de datos lleno de ceros.
- **`of=/dev/sda`**: Indica el disco de destino a sobrescribir.
- **`bs=1M`**: Define el tamaño del bloque como 1 MB, lo cual optimiza la velocidad del proceso.
- **`status=progress`**: Muestra el progreso en tiempo real.

**Propósito**: Sobrescribir todo el disco con ceros es suficiente para borrar datos de manera básica. Sin embargo, los datos podrían recuperarse con herramientas avanzadas.

---

### 5. **Sobrescribir el disco con datos aleatorios (limpieza más segura)**
```bash
dd if=/dev/urandom of=/dev/sda bs=1M status=progress
```
- **`if=/dev/urandom`**: Utiliza un flujo de datos aleatorios como entrada.
- **Ventaja**: Este método dificulta significativamente la recuperación de datos incluso con técnicas avanzadas.
- **Desventaja**: Es más lento que usar ceros debido a la generación de datos aleatorios.

**¿Cuál elegir?**
- Usa **ceros** (`/dev/zero`) si necesitas limpiar el disco para un uso interno o formateo básico.
- Usa **aleatoriedad** (`/dev/urandom`) si planeas desechar el disco o transferirlo a terceros y necesitas mayor seguridad.

---

### 6. **Revisar el estado del disco después de la limpieza**
```bash
lsblk
```
- Asegúrate de que el disco esté limpio y sin particiones activas.

---

### 7. **Crear una nueva tabla de particiones (opcional)**
Si deseas reutilizar el disco después de limpiarlo, puedes crear una nueva tabla de particiones:

```bash
sudo fdisk /dev/sda
```
1. **Crear una nueva tabla de particiones**: Escribe `g` para crear una tabla GPT (moderna) o `o` para una tabla MBR (legado).
2. **Crear una nueva partición**: Usa `n` y sigue las instrucciones.
3. **Guardar cambios**: Escribe `w` para guardar.

---

### 8. **Formatear el disco (opcional)**
Después de crear una partición, puedes formatearla con un sistema de archivos. Por ejemplo:

```bash
sudo mkfs.ext4 /dev/sda1
```
- Esto crea un sistema de archivos EXT4 en la partición `/dev/sda1`.

---

### 9. **Montar el disco (opcional)**
Por último, puedes montar el disco en un punto de tu sistema si necesitas usarlo:

```bash
sudo mount /dev/sda1 /mnt
```