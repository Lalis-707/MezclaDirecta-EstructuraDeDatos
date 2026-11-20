# Práctica de Mezcla Directa (Ordenamiento Externo)

**Materia:** AED-1026 Estructura de Datos

**Unidad 5:** Ordenamiento Externo

**Docente:** Víctor Hugo Vásquez Herrera

**Instituto Tecnológico Superior de Xalapa** · Ingeniería en Sistemas Computacionales

**Alumna:** García Saavedra Laura Irais

---

## ¿Qué hace el método `intercalarBloques`?

El ordenamiento externo se usa cuando los datos son tan grandes que no caben en memoria y hay que trabajarlos desde archivos. La **Mezcla Directa (Straight Merge)** es uno de sus algoritmos: toma dos archivos auxiliares que ya contienen bloques ordenados y los combina en uno solo, ordenado.

El método `intercalarBloques(archivoF1, archivoF2, archivoSalida, L)` hace justamente esa combinación:

1. **Abre tres archivos**: dos de lectura (`f1.txt` y `f2.txt`) y uno de escritura (`f3.txt`). Usa `try-with-resources`, así que Java los cierra solo al terminar.
2. **Lee el primer número de cada archivo**, que es el "frente" de cada fila.
3. **Compara los dos números del frente** y escribe el menor en `f3.txt`. Luego avanza solo en el archivo del que salió ese número. Para llevar la cuenta usa dos contadores (`elementosEscritos1` y `elementosEscritos2`), que miden cuántos elementos se han sacado de cada bloque de tamaño `L`.
4. **Cuando uno de los bloques llega a `L` elementos**, el otro ya no se compara: se vacía lo que le queda en la salida.
5. **Cuando uno de los archivos se acaba**, el ciclo principal termina. Dos ciclos `while` finales vacían lo que haya sobrado en cualquiera de los dos archivos para no perder elementos.
6. Al terminar imprime `Bloque intercalado con exito!`. Si falta algún archivo, atrapa el `IOException` e imprime el error.

### Código completado (los 3 TODO)

| TODO | Qué se completó |
|------|-----------------|
| **TODO 1** | La condición `val1 <= val2` para saber si el número de F1 es menor o igual al de F2. |
| **TODO 2** | Cuando el menor es el de F2: `escritor.println(val2);`, `linea2 = lector2.readLine();` y `elementosEscritos2++;`. |
| **TODO 3** | Para vaciar lo que sobra de F2: `escritor.println(linea2);`, `linea2 = lector2.readLine();` y `elementosEscritos2++;`. |

Además, se agregaron **dos ciclos `while` después del ciclo principal** que no estaban en el código base. Sin ellos, cuando un archivo se termina antes, el último número del otro archivo se perdía (por ejemplo, con `45` en F1 y `12` en F2 solo se escribía el `12`).

### Ejemplo de ejecución (`L = 3`)

| Paso | Frente de F1 | Frente de F2 | Se escribe en F3 |
|------|--------------|--------------|------------------|
| 1 | 10 | 20 | 10 |
| 2 | 30 | 20 | 20 |
| 3 | 30 | 40 | 30 |
| 4 | 50 | 40 | 40 |
| 5 | 50 | 60 | 50 |
| 6 | (vacío) | 60 | 60 |

---

## Estructura del proyecto

```
EstructuraDeDatos
├── src
│   └── Estructuras_Lineales
│       └── Unidad05y06
│           └── MezclaDirectaPractica
│               └── MezclaDirectaPractica.java
├── f1.txt
├── f2.txt
├── f3.txt
└── README.md
```

La clase está en el paquete `Estructuras_Lineales.Unidad05y06.MezclaDirectaPractica`.

## Requisitos

- JDK instalado (versión 17 o superior)
- IntelliJ IDEA (o cualquier IDE de Java)
- Git

## Cómo clonar, configurar y ejecutar

1. **Clona** el repositorio:
   ```bash
   git clone https://github.com/Lalis-707/MezclaDirecta-EstructuraDeDatos.git
   ```
2. **Abre el proyecto** en IntelliJ IDEA: **File → Open** y selecciona la carpeta clonada.
3. **Configura el JDK**: en **File → Project Structure → Project**, elige un JDK instalado en tu equipo.
4. **Revisa los archivos de entrada**: `f1.txt` y `f2.txt` deben estar en la **carpeta raíz** del proyecto (junto a este README), con un número por renglón y ya ordenados. Si no existen, el programa muestra `Error en E/S de archivos`.
5. **Ejecuta**: abre `src/Estructuras_Lineales/Unidad05y06/MezclaDirectaPractica/MezclaDirectaPractica.java` y presiona **Run** (el botón verde junto al método `main`). El `main` usa `L = 3`.
6. **Revisa el resultado**: se genera `f3.txt` en la raíz del proyecto. Si no lo ves en el panel Project, haz clic derecho sobre el proyecto → **Reload from Disk**.

## Archivos de prueba

**f1.txt**
```
10
30
50
```

**f2.txt**
```
20
40
60
```

## Salida en consola

```
Bloque intercalado con exito!

Process finished with exit code 0
```

## Resultado (`f3.txt`)

```
10
20
30
40
50
60
```

## Limitaciones

El método mezcla **un bloque de F1 con un bloque de F2** (de tamaño `L`). Los contadores no se reinician, así que para archivos con varios bloques por archivo habría que repetir el proceso por cada par de bloques. Para esta práctica se probó con un bloque por archivo (`L = 3`).
