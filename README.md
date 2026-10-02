# 📖 Manual de Uso - Parcheador Autónomo de Traducciones para Unreal Engine 5

El **Parcheador Autónomo (`Parcheador_JSON_UE5.exe`)** es una herramienta portátil diseñada para aplicar traducciones en juegos desarrollados con Unreal Engine 5 a partir de un archivo `.json` de forma **100% independiente** (no requiere tener instalado Python, LM Studio ni herramientas externas en el equipo).

---

## 📋 Índice
1. [Requisitos Previos](#1-requisitos-previos)
2. [Interfaz y Selección de Archivos](#2-interfaz-y-selección-de-archivos)
   - [A. Archivo JSON de Traducción](#a-archivo-json-de-traducción)
   - [B. Carpeta del Juego](#b-carpeta-del-juego)
   - [C. Clave AES de Cifrado (Opcional)](#c-clave-aes-de-cifrado-opcional)
3. [Explicación de Opciones de Parcheo](#3-explicación-de-opciones-de-parcheo)
4. [Paso a Paso: Aplicar el Parche](#4-paso-a-paso-aplicar-el-parche)
5. [Cómo Iniciar el Juego en Español](#5-cómo-iniciar-el-juego-en-español)
6. [Resolución de Problemas Frecuentes](#6-resolución-de-problemas-frecuentes)
7. [Cómo Desinstalar o Restaurar el Juego Original](#7-cómo-desinstalar-o-restaurar-el-juego-original)

---

## 1. Requisitos Previos

> [!IMPORTANT]
> **El juego debe estar CERRADO por completo antes de aplicar el parche.**  
> Si el juego se encuentra en ejecución en segundo plano (`.exe`), el sistema operativo Windows bloqueará los archivos `.pak` contra escritura (`os error 32: Archivo en uso`), impidiendo que el parcheador actualice los textos.

* **Archivo `.json` de traducción:** El archivo exportado desde el Editor Web de Traducciones (o generado por el traductor).
* **Juego instalado:** Cualquier versión del juego instalada en el disco local.

---

## 2. Interfaz y Selección de Archivos

Al ejecutar `Parcheador_JSON_UE5.exe` se abrirá la ventana gráfica del instalador:

```
+-----------------------------------------------------------------------+
|  ⚡ Parcheador Universal UE (JSON ➔ Español Aditivo)                 |
|  Añade el idioma Español a Unreal Engine 5 sin alterar originales.    |
+-----------------------------------------------------------------------+
|  📄 Archivo JSON de Traducción:                                       |
|  [ C:/Ruta/A/traduccion_export.json                          ] [Examinar JSON...] |
|                                                                       |
|  🎮 Carpeta del Juego:                                                |
|  [ D:/Juegos/MiJuego/game                                    ] [Examinar Carpeta.] |
|                                                                       |
|  🔑 Clave AES de Cifrado (Opcional - Solo si el juego está cifrado):  |
|  [ 0x1A2B3C4D...                                                     ] |
|                                                                       |
|  [✓] Normalizar caracteres especiales (acentos / ñ)                  |
|  [✓] Configurar inicio automático en Español (Engine.ini y .bat)      |
|  [✓] Crear copia de seguridad (.orig) de paquetes originales          |
|                                                                       |
|  [                          🚀 Parchear Juego                        ] |
|                                                                       |
|  Consola de progreso:                                                 |
|  ==================================================================== |
|  [1/5] Leyendo archivo de traducciones...                             |
+-----------------------------------------------------------------------+
```

### A. Archivo JSON de Traducción
1. Haz clic en el botón **`Examinar JSON...`**.
2. Selecciona el archivo `.json` que contiene las traducciones.
3. El parcheador admite tanto el formato por categorías/namespaces (`{ "Namespace": { "Clave": "Texto" } }`) como listas planas de frases.

### B. Carpeta del Juego
1. Haz clic en el botón **`Examinar Carpeta...`**.
2. Selecciona la carpeta raíz de instalación del juego (donde se ubica el ejecutable del juego o las carpetas `Content/` / `Paks/`).
3. El parcheador detectará y localizará automáticamente el contenedor `.pak` principal del proyecto (por ejemplo, `JUEGO-Windows.pak`).

### C. Clave AES de Cifrado (Opcional)
* **Juegos sin cifrar (la mayoría de indies y juegos de Unreal Engine):** Deja este campo completamente en blanco.
* **Juegos comerciales con cifrado AES-256:** Introduce aquí la clave hexadecimal de 64 caracteres (ejemplo: `0x4A1E8F2C...`).
* **Detección inteligente:** Si el juego está cifrado y dejas el campo vacío, el parcheador detectará automáticamente el bloqueo y te mostrará una ventana emergente solicitándote la clave AES para reintentar el parcheo sin tener que reiniciar el programa.

---

## 3. Explicación de Opciones de Parcheo

El parcheador incluye tres casillas de verificación configuradas por defecto con los valores recomendados:

| Opción | Estado Recomendado | ¿Para qué sirve? |
| :--- | :---: | :--- |
| **Normalizar caracteres especiales** | **Activado (✓)** | Sustituye tildes (`á, é, í...`), diéresis (`ü`), eñes y comillas tipográficas por caracteres estándar limpios. Esto previene que los textos aparezcan rotos o con símbolos extraños (`?`, espacios vacíos) si la fuente del juego no incluye caracteres en español. |
| **Configurar inicio automático en Español** | **Activado (✓)** | Inyecta la cultura `es-ES` en los archivos de configuración (`DefaultEngine.ini`, `Engine.ini` y `GameUserSettings.ini`) y genera el acceso directo **`Jugar_en_Espanol.bat`** en la carpeta del juego. |
| **Crear copia de seguridad (.orig)** | **Activado (✓)** | Crea un respaldo exacto e intacto del paquete original del juego (ejemplo: `JUEGO-Windows.pak.orig`). Permite restaurar el juego a su estado de fábrica en cualquier momento. |

---

## 4. Paso a Paso: Aplicar el Parche

1. Asegúrate de que el **juego esté cerrado**.
2. Abre **`Parcheador_JSON_UE5.exe`**.
3. Selecciona el archivo **JSON** y la **Carpeta del Juego**.
4. (Opcional) Introduce la **Clave AES** si el juego requiere desencriptado.
5. Deja marcadas las 3 opciones recomendadas.
6. Haz clic en el botón verde **`🚀 Parchear Juego`**.
7. La consola inferior mostrará el progreso en 5 etapas automáticas:
   - **`[1/5]`** Lectura y validación de las frases del archivo JSON.
   - **`[2/5]`** Normalización de acentos y caracteres especiales.
   - **`[3/5]`** Desempaquetado temporal y extracción de las plantillas de hashes binarios UE.
   - **`[4/5]`** Compilación binaria `.locres` e inyección de la cultura `es-ES` dentro del archivo `.pak`.
   - **`[5/5]`** Limpieza de archivos temporales y creación del lanzador directo.
8. Al finalizar, aparecerá una ventana emergente notificando:  
   **`🎉 ¡Juego parcheado con éxito! Se añadió el idioma Español sin modificar los idiomas originales.`**

---

## 5. Cómo Iniciar el Juego en Español

Para jugar con la traducción activa tienes dos métodos:

### Método A: Mediante el lanzador directo (Recomendado)
* Ve a la carpeta raíz del juego y haz doble clic sobre el archivo **`Jugar_en_Espanol.bat`**.
* Este lanzador inicia el juego forzando el parámetro oficial de Unreal Engine: `-culture=es-ES`.

### Método B: Lanzador habitual o cliente Steam
* Si abres el juego directamente desde su `.exe` habitual o desde la biblioteca de Steam, el juego leerá automáticamente la configuración inyectada en `Engine.ini` / `DefaultEngine.ini` y cargará en español.
* Si el juego incluye menú de selección de idioma en opciones, el español estará disponible como opción seleccionable.

---

## 6. Resolución de Problemas Frecuentes

### ❌ Error: *"El archivo .pak está bloqueado por el juego abierto"*
* **Causa:** El ejecutable del juego (`JUEGO-Win64-Shipping.exe` o `JUEGO.exe`) sigue abierto en segundo plano.
* **Solución:** Cierra el juego (o finaliza su proceso desde el Administrador de Tareas de Windows) y pulsa de nuevo en **`🚀 Parchear Juego`**.

### ❌ Error: *"El archivo .pak está cifrado con AES. Se requiere la clave AES"*
* **Causa:** El desarrollador comercial del juego protegió sus archivos `.pak` con una clave AES de 256 bits.
* **Solución:** Introduce la clave AES en el campo de texto `🔑 Clave AES` (o en la ventana emergente que aparecerá automáticamente) y pulsa Aceptar.

### ❌ El juego arranca en inglés o en otro idioma
* **Causa:** El juego tiene configurada una preferencia regional persistente en su archivo de guardado (`SaveGames`).
* **Solución:** Inicia el juego usando el archivo **`Jugar_en_Espanol.bat`**, o entra en el menú de opciones del juego y selecciona el idioma Español.

### ❌ Los textos tienen signos extraños (`?` o símbolos rotos)
* **Causa:** La tipografía interna del juego no soporta caracteres UTF-8 extendidos (tildes / diéresis).
* **Solución:** Vuelve a aplicar el parche asegurándote de tener activada la casilla **`Normalizar caracteres especiales`**.

---

## 7. Cómo Desinstalar o Restaurar el Juego Original

El parcheador utiliza un método **completamente reversible**:

1. Ve a la carpeta `Content/Paks/` del juego (por ejemplo: `JUEGO/Content/Paks/`).
2. Elimina el archivo `JUEGO-Windows.pak` parcheado.
3. Renombra la copia de seguridad `JUEGO-Windows.pak.orig` a `JUEGO-Windows.pak`.
4. (Opcional) Borra el archivo `Jugar_en_Espanol.bat` de la raíz del juego.

El juego volverá a su estado 100% original sin necesidad de reinstalarlo ni verificar archivos.
