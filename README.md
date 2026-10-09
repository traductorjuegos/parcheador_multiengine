# 📖 Manual de Uso - Parcheador Multiengine (Unreal Engine / RPG Maker)

El **Parcheador Autónomo (`Parcheador_multiengine.exe`)** (versión **v0.2**) es una herramienta portátil diseñada para aplicar traducciones en juegos desarrollados tanto con **Unreal Engine (4 y 5)** como con **RPG Maker (MZ y MV)** de forma **100% independiente** (no requiere tener instalado Python, LM Studio ni herramientas externas en el equipo).

Toma como entrada exclusivamente un paquete **`.zip`** que contiene la traducción y cualquier recurso gráfico o archivo complementario que requiera el juego.

---

## 📋 Índice
1. [Requisitos Previos](#1-requisitos-previos)
2. [Estructura del Paquete ZIP de Traducción](#2-estructura-del-paquete-zip-de-traducción)
   - [A. Paquete ZIP para RPG Maker (MZ / MV)](#a-paquete-zip-para-rpg-maker-mz--mv)
   - [B. Paquete ZIP para Unreal Engine (4 / 5)](#b-paquete-zip-para-unreal-engine-4--5)
3. [Soporte Multi-Motor y Detección Automática](#3-soporte-multi-motor-y-detección-automática)
4. [Interfaz y Opciones Dinámicas según Motor](#4-interfaz-y-opciones-dinámicas-según-motor)
5. [Explicación de Opciones de Parcheo](#5-explicación-de-opciones-de-parcheo)
6. [Paso a Paso: Aplicar el Parche](#6-paso-a-paso-aplicar-el-parche)
7. [Cómo Iniciar el Juego en Español](#7-cómo-iniciar-el-juego-en-español)
8. [Cómo Desinstalar o Restaurar el Juego Original](#8-cómo-desinstalar-o-restaurar-el-juego-original)

---

## 1. Requisitos Previos

> [!IMPORTANT]
> **El juego debe estar CERRADO por completo antes de aplicar el parche.**  
> Si el juego se encuentra en ejecución en segundo plano (`.exe`), el sistema operativo Windows bloqueará los archivos contra escritura impidiendo que el parcheador actualice los textos o imágenes.

* **Paquete `.zip` de traducción:** Archivo comprimido con la traducción y recursos.
* **Juego instalado:** Cualquier juego compatible de Unreal Engine o RPG Maker en el disco local.

---

## 2. Estructura del Paquete ZIP de Traducción

El instalador recibe un único archivo comprimido `.zip` que contendrá todo lo necesario según el motor:

### A. Paquete ZIP para RPG Maker (MZ / MV)
Un archivo `.zip` que incluye el JSON de traducción y las imágenes o recursos en la estructura de carpetas del juego:
```text
MiTraduccion_RPGMaker.zip
├── traduccion.json                  <-- Archivo JSON con metadatos y textos
└── img/                             <-- Imágenes traducidas en su estructura relativa
    └── pictures/
        └── UI/
            ├── Status_ResetButton.png
            ├── Status_OKButton.png
            └── Status_Skill_Fishing.png
```
* **Textos:** Inyecta automáticamente los textos traducidos en `data/*.json` y en `js/plugins.js`.
* **Imágenes y recursos:** Despliega las imágenes traducidas en sus rutas correspondientes dentro del juego y realiza una copia de seguridad en `data/backup_original/assets/` de cualquier imagen sobrescrita para poder restaurarla en cualquier momento.

### B. Paquete ZIP para Unreal Engine 4/5 (IoStore / Zen / Mods)
Un archivo `.zip` que contiene:
1. `metadata.json` (o archivo `.json` de metadatos del proyecto).
2. Archivos precompilados del mod:
   - Contenedor mod: `z_Spanish_P.pak` (o similar).
   - Datos IoStore / Zen (UE5): `z_Spanish_P.ucas` y `z_Spanish_P.utoc`.
   - (Opcional) Firmas `.sig`.

> [!TIP]
> **Despliegue rápido y seguro:**  
> El parcheador no necesita desempaquetar archivos `.pak` gigantes originales. Despliega los mods en `Content/Paks`, configura los `.ini` y crea el acceso directo en segundos.

---

## 3. Soporte Multi-Motor y Detección Automática

El parcheador analiza los metadatos y la estructura de la carpeta del juego:

| Motor | Contenido del ZIP | Método de Inyección | Desinstalación / Restore |
| :--- | :--- | :--- | :--- |
| **Unreal Engine** | `metadata.json` + `.pak`, `.ucas`, `.utoc` | Despliegue en `Paks/`, configuración de `Engine.ini` y lanzador `.bat`. | Elimina los archivos `.pak`, `.ucas`, `.utoc` instalados y revierte `.ini`. |
| **RPG Maker MZ / MV** | `traduccion.json` + carpeta `img/` u otros assets | Inyección en `data/*.json`, `js/plugins.js` y copia de imágenes en `img/`. | Restaura datos e imágenes originales desde `data/backup_original/`. |

---

## 4. Interfaz y Opciones Dinámicas según Motor

Al ejecutar `Parcheador_multiengine.exe`, la interfaz se muestra limpia y compacta. Ni el cuadro de la clave AES ni los checkboxes de configuración se muestran inicialmente hasta que se carga y analiza el archivo `.zip`:

```
+-------------------------------------------------------------------------------+
|  ⚡ Parcheador Multiengine (Unreal Engine / RPG Maker)                   v0.2 |
|  Aplica traducciones y recursos a Unreal Engine y RPG Maker desde paquetes ZIP|
+-------------------------------------------------------------------------------+
|  📦 Paquete de Traducción (*.zip):                                            |
|  [ C:/Ruta/A/MiTraduccion.zip                                ] [Examinar...]  |
|                                                                               |
|  🎮 Carpeta del Juego (donde está el .exe o Content/data):                   |
|  [ D:/Juegos/MiJuego/                                        ] [Examinar Carpeta.] |
|                                                                               |
|  (La clave AES y las opciones aparecen dinámicamente al leer el paquete ZIP)  |
|                                                                               |
|  [ ⚡ PARCHEAR ]            [ ▶ JUGAR ]            [ ↺ Restaurar Original ]   |
|                                                                               |
|  Consola de Registro:                                                         |
|  ============================================================================ |
|  [1/5] Leyendo paquete de traducciones y metadatos...                         |
+-------------------------------------------------------------------------------+
```

* **Si se detecta RPG Maker:** Solo se muestra la opción aplicable de *Crear copia de seguridad (backup_original) de los archivos e imágenes originales* (sin cuadro de clave AES ni opciones innecesarias de `.ini`).
* **Si se detecta Unreal Engine:** Se despliega el campo opcional de *Clave AES de Cifrado*, junto a las opciones de *Normalizar caracteres especiales*, *Configurar inicio automático en Español* y *Crear copia de seguridad (.orig)*.

---

## 5. Explicación de Opciones de Parcheo

| Opción | Aplica en | Descripción |
| :--- | :--- | :--- |
| **Crear copia de seguridad** | RPG Maker / Unreal Engine | Genera respaldos antes de modificar archivos (`data/backup_original/` con datos e imágenes en RPG Maker o `.orig` en Unreal Engine). |
| **Normalizar caracteres especiales** | Solo Unreal Engine | Sustituye acentos para motores de Unreal Engine con fuentes limitadas. En RPG Maker se preservan todos los caracteres UTF-8 nativos automáticamente. |
| **Configurar inicio automático en Español** | Solo Unreal Engine | Configura `Engine.ini`, `GameUserSettings.ini`, emuladores de Steam y genera accesos directos `.bat`. |

---

## 6. Paso a Paso: Aplicar el Parche

1. Abre **`Parcheador_multiengine.exe`**.
2. Selecciona el archivo **`.zip`** de traducción y la **Carpeta del Juego**.
3. Revisa las opciones específicas que hayan aparecido para el motor de tu juego.
4. Haz clic en **`⚡ PARCHEAR`**.
5. El registro mostrará el avance en tiempo real hasta confirmar la inyección exitosa de textos y recursos.

---

## 7. Cómo Iniciar el Juego en Español

* **Desde la propia interfaz:** Haz clic en el botón **`▶ JUGAR`**. El parcheador iniciará automáticamente el ejecutable del juego (`Game.exe`, `nw.exe` o el binario de Unreal Engine).
* **Desde el explorador de archivos:**
  * **RPG Maker:** Ejecuta `Game.exe`.
  * **Unreal Engine:** Ejecuta **`Jugar_en_Espanol.bat`** o el ejecutable principal.

---

## 8. Cómo Desinstalar o Restaurar el Juego Original

1. Abre **`Parcheador_multiengine.exe`**.
2. Selecciona la carpeta del juego (y opcionalmente el paquete ZIP).
3. Haz clic en el botón **`↺ Restaurar Original`**.
4. El programa:
   - **RPG Maker:** Restaura los archivos `data/*.json`, `js/plugins.js` y las imágenes originales desde `data/backup_original/`, eliminando los archivos nuevos que se hubieran añadido.
   - **Unreal Engine:** Elimina los archivos mod (`.pak`, `.ucas`, `.utoc`) de `Paks/`, revierte los `.ini` y elimina los accesos directos `.bat`.
