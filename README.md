## Documento 1 – Soluciones para "wuolah‐free‐PruebashellL21920.pdf" (Grupo L2)

_Este documento recoge las soluciones para cada ejercicio de la prueba de Administración del Sistema – Grupo L2. Se asume que el directorio base es, por ejemplo, `/home/usuario` y que se crea la estructura en `~/pruebaL2`._

### 1. Cambiar el passwd de la cuenta
```bash
passwd
```

### 2. Crear la estructura de directorios (usando rutas absolutas)
```bash
mkdir -p /home/usuario/pruebaL2/{basura,guion,varios,temp}
```

### 3. Modificar permisos del directorio _pruebaL2_ y quitar permisos en _varios_ y _guion_
```bash
chmod 700 /home/usuario/pruebaL2
chmod go-rwx /home/usuario/pruebaL2/varios /home/usuario/pruebaL2/guion
```

### 4. Copiar archivos específicos a _varios_ (usando rutas relativas)
```bash
cd /home/so/ficheros
cp [aeiouAEIOU]*[0-9]* /home/usuario/pruebaL2/varios
```

### 5. Mover archivos con "bak" de _varios_ a _basura_
```bash
mv /home/usuario/pruebaL2/varios/*bak /home/usuario/pruebaL2/basura
```

### 6. Borrar el directorio _basura_
```bash
rm -r /home/usuario/pruebaL2/basura
```

### 7. Añadir _guion_ y el directorio actual al PATH
```bash
echo 'export PATH=$PATH:/home/usuario/pruebaL2/guion:$(pwd)' >> ~/.bashrc
source ~/.bashrc
```

### 8. Crear variable _GUION_ y alias _cdvarios_
```bash
echo 'export GUION=/home/usuario/pruebaL2/guion' >> ~/.bashrc
echo "alias cdvarios='cd /home/usuario/pruebaL2/varios'" >> ~/.bashrc
source ~/.bashrc
```

### 9. Crear fichero con nombres de archivos específicos en `/etc`
```bash
find /etc -maxdepth 1 -type f -printf '%f\n' 2> /home/usuario/pruebaL2/temp/errores9 | grep -E '^[^a-zA-Z][ae].*[0-9]' | sort > /home/usuario/pruebaL2/varios/ficheros
```

### 10. Añadir la fecha y hora al fichero _ficheros_
```bash
echo "Buenos dias. Esta usted en $(hostname). Es $(date '+%A, %d de %B de %Y'). Son las $(date '+%T')" >> /home/usuario/pruebaL2/varios/ficheros
```

### 11. Crear un enlace simbólico en _guion_
```bash
ln -s ../temp/errores9 /home/usuario/pruebaL2/guion/enlace
```

### 12. Contar archivos con "muchos" en `/home/so/ficheros`
```bash
find /home/so/ficheros -maxdepth 1 -type f \( -name 'f*' -o -name 'a*' \) | grep -i 'muchos' | wc -l > /home/usuario/pruebaL2/varios/listadoL2.txt
```

### 13. Visualizar archivos específicos en `/usr`
```bash
find /usr -maxdepth 1 -type f -name 'm?.??' ! -newer ~/.profile -exec cat {} \; | less
```

### 14. Crear grupo `admin20` con GID 500
```bash
groupadd -g 500 admin20
```

### 15. Configurar grupo predeterminado para nuevos usuarios
```bash
useradd -g admin20 nuevo_usuario
```

### 16. Buscar paquete `jedit`
```bash
apt-cache search jedit
```

### 17. Visualizar `/etc/passwd` y `/etc/shadow`
```bash
less /etc/passwd /etc/shadow
```

### 18. Crear script `guardanota`
```bash
#!/bin/bash
if [ "$1" -lt 1 ]; then
  echo "Error: el parámetro debe ser mayor o igual que 1"
  exit 1
fi
for ((i=1; i<=$1; i++)); do
  read -p "Ingrese una nota (0-10): " nota
  if [ "$nota" -lt 0 ] || [ "$nota" -gt 10 ]; then
    echo "Error: nota fuera de rango"
    exit 1
  elif [ "$nota" -ge 5 ]; then
    echo "aprobado" >> /home/usuario/pruebaL2/varios/notas
  else
    echo "suspenso" >> /home/usuario/pruebaL2/varios/notas
  fi
  done
```

```bash
chmod +x /home/usuario/pruebaL2/guion/guardanota
```

### 19. Ejecutar `guardanota` con parámetro 3
```bash
/home/usuario/pruebaL2/guion/guardanota 3
```

### 20. Crear script `ejecut`
```bash
#!/bin/bash
if [ "$#" -ne 2 ]; then
  echo "Error: se requieren 2 parámetros"
  exit 1
fi
if [ ! -d "$1" ]; then
  echo "Error: el primer parámetro no es un directorio válido"
  exit 1
fi
output="$1/$2"
> "$output"
for file in $(find ~ -type f -atime -3); do
  echo "$file" >> "$output"
done
sort -r "$output" -o "$output"
```

```bash
chmod +x /home/usuario/pruebaL2/guion/ejecut
```

---
