# whatsapp-lite-builder

Este repositorio aloja un **Orquestador de Compilación Automatizado** diseñado para portar y empaquetar [whatsapp-lite](https://github.com/Bittu-the-coder/whatsapp-lite) (un cliente de WhatsApp Web ultra ligero, enfocado en privacidad y que consume hasta 10 veces menos RAM que la versión oficial basada en Electron) a entornos de **Linux** y **macOS**, manteniendo viva la distribución multiplataforma.

A través de flujos de trabajo automatizados en GitHub Actions, el orquestador toma el código base original de Windows, inyecta firmas de compatibilidad modernas (User Agent) y compila paquetes nativos listos para usar.

---

## 🚀 Pre-Release / Estado del Proyecto (Fase Constructiva)

¡Estamos en fase inicial de pruebas y los resultados son muy prometedores! Este instalador genera una versión funcional considerada como **Pre-release / Beta**, aportando un cliente de bajo consumo sumamente útil para el ecosistema fuera de Windows.

### 📈 Funcionalidades Listas y Probadas
* **Mensajería:** Envío y recepción de mensajes de texto en tiempo real de forma fluida.
* **Mensajes de voz:** Reproducción y envío de audios nativos 100% operativos.
* **Instaladores Multi-arquitectura:** Generación exitosa de paquetes nativos para múltiples entornos.

### ⚠️ Problemas Conocidos (Known Issues - Motor Gráfico & Aceleración)
Al migrar el software original diseñado para la capa gráfica de Windows (DirectX) hacia entornos Unix, nos topamos con los clásicos desafíos del motor de renderizado bajo **OpenGL/Vulkan** (Linux) y **Metal** (macOS):
* **Videos e Historias (Estados):** La reproducción de streams de video en las historias y chats presenta fallos de renderizado y parpadeos debido a la gestión de aceleración por hardware en el motor web.
* **Consumo de Recursos en Multimedia:** Aunque la mensajería base consume una fracción de RAM en comparación con Electron, la decodificación multimedia en estas plataformas aún no está completamente optimizada en el port y puede elevar temporalmente el uso de CPU/RAM.
* **Todos los demás problemas del repositorio fuente**

---

## 🛠️ Arquitectura de Compilación Integrada

El pipeline de compilación (`.github/workflows/`) ejecuta de forma transparente las siguientes optimizaciones sobre el código fuente antes de empaquetar:

1. **Inyección de User Agent Seguro:** Modifica dinámicamente la ventana de Tauri v2 con una firma moderna de Chrome aprobada por las reglas de Meta para garantizar la conexión.
2. **Parche de Retrocompatibilidad Apple:** Fuerza la propiedad `minimumSystemVersion` en el archivo `tauri.conf.json` fijándola a macOS 11.0 (Big Sur) en adelante.
3. **Limpieza de Historial Estructural:** Elimina referencias a submódulos Git para compilar de manera directa.

---

## 📦 Matriz de Paquetes Generados

Cada ejecución del pipeline genera y firma artefactos nativos para las siguientes plataformas y arquitecturas:

| Sistema Operativo | Arquitectura | Tipo de Instalador Producido |
| :--- | :--- | :--- |
| 🐧 **Linux** (Ubuntu 22.04) | Intel / AMD64 | `.deb` y `.AppImage` |
| 🐧 **Linux ARM** (Nativo) | ARM64 | `.deb` y `.AppImage` |
| 🍏 **macOS** (Mac-15 Intel) | Universal (Intel + Apple Silicon) | `.dmg` y `.app` |

---

## 💻 Entornos Probados con Éxito

Esta versión pre-release ya ha sido ejecutada y validada en condiciones reales:
* **Linux (Intel/AMD64):** Probado en **Linux Mint 22.3** funcionando de forma fluida aunque con algunos glitches en los videos de las historias de usuario, funciona del todo el .deb, aun en pruebas el .appImage, da algunos problemas.
* **macOS (Intel):** Probado en **macOS Ventura** mediante **OpenCore Legacy Patcher (OCLP).**, corriendo de forma nativa en una **MacBook Pro A1278 (Mid 2012)**. N o funcionan los videos de las historias, no con OCLP al menos, lo demás funciona, salvo por lo demás fallos que pueda tener el proyecto original. Requiere permitir la ejecución como programa de desarrollador desconocido de momento por politicas de seguridad de mac os despues de instalarlo.

### 🧪 Estado en Arquitectura ARM
* **Aún no probado:** Los instaladores para arquitecturas ARM (Linux ARM64 / Apple Silicon) se generan automáticamente en el pipeline, pero **aún no han sido validados en hardware real**.
* **¡Ayúdanos a probar!** Dejamos estos paquetes a total disposición de la comunidad. Si tienes un dispositivo ARM y deseas probarlo, tus comentarios y reportes nos serán de enorme utilidad.
* **Próximos pasos:** Se prevén pruebas oficiales en entornos Linux ARM en el futuro cercano.

## 🏗️ Cómo Ejecutar el Constructor

Al estar configurado mediante `workflow_dispatch`, puedes disparar nuevas compilaciones manualmente:

1. Ve a la pestaña **Actions** en tu repositorio de GitHub.
2. Selecciona el flujo de trabajo **"Orquestador de Compilación con Firma"**.
3. Haz clic en el menú desplegable **Run workflow** y selecciona tu rama.
4. Una vez finalizado el trabajo, descarga los instaladores empaquetados desde la sección de **Artifacts**.

---

## 📝 Descargo de Responsabilidad / Disclaimer

Este proyecto es un constructor de empaquetado no oficial. No está afiliado, asociado, autorizado, respaldado ni conectado oficialmente de ninguna manera con WhatsApp, WhatsApp Inc., Meta Platforms, Inc., ni con el creador del código base original. WhatsApp y sus logotipos son marcas registradas de Meta Platforms, Inc.
