# CreaApps Studio — starter multiplataforma

Este es un proyecto inicial para desarrollar CreaApps como aplicación web y contenedor nativo Android/iOS con Capacitor. Incluye editor de HTML/CSS/JS, vista previa aislada, almacenamiento local, exportación y botones de prueba para GPS, cámara, notificaciones y escritura de un archivo de proyecto.

## Requisitos
- Node.js LTS y npm.
- Android Studio + Android SDK para compilar Android.
- macOS + Xcode para compilar iOS (Apple requiere macOS/Xcode para firmar y compilar la app iOS).
- Dispositivo físico o emulador para probar permisos y funciones.

## Instalar dependencias y ejecutar en web

```bash
npm install
npm run dev
```

Abre la URL local que Vite muestre. Para crear los archivos de producción:

```bash
npm run build
```

## Crear el proyecto Android

```bash
npm install
npx cap add android
npm run build
npx cap sync android
npx cap open android
```

En Android Studio espera la sincronización de Gradle y pulsa Run para probar en un dispositivo/emulador. Para crear APK de prueba: Build > Build Bundle(s) / APK(s) > Build APK(s). Para distribuir por Google Play, normalmente genera un Android App Bundle (AAB), firma la compilación y sigue los requisitos vigentes de Play Console.

## Crear el proyecto iOS

En un Mac con Xcode:

```bash
npm install
npx cap add ios
npm run build
npx cap sync ios
npx cap open ios
```

En Xcode configura el Bundle Identifier, el equipo de firma y las descripciones de permisos. Ejecuta en simulador o dispositivo. Para instalar en dispositivos de otras personas o publicar, necesitarás la firma y distribución apropiadas de Apple.

## Funciones nativas incluidas
- GPS puntual: `@capacitor/geolocation`.
- Cámara: `@capacitor/camera`.
- Notificaciones locales: `@capacitor/local-notifications`.
- Escritura del archivo del proyecto: `@capacitor/filesystem`.

Los plugins piden permisos cuando corresponde. En producción revisa los textos de privacidad y los permisos nativos requeridos por cada plataforma.

## Segundo plano: limitación importante
El botón GPS de este starter obtiene una posición puntual. No es un servicio de rastreo continuo en segundo plano. Para ello hay que integrar una solución específica de geolocalización en segundo plano, configurar Android (servicio en primer plano, notificación persistente y permisos de ubicación en segundo plano según la versión de Android) y iOS (capacidad Background Modes y textos de uso de ubicación). Los sistemas pueden suspender o limitar tareas; no se debe prometer funcionamiento ininterrumpido. Pide consentimiento claro al usuario y muestra cuándo se está compartiendo ubicación.

## IA: conectar de forma segura
Este starter todavía no llama a un modelo de IA. Para añadir generación/reparación de código, crea un backend propio y guarda allí la clave del proveedor como variable de entorno. Nunca incrustes una API key en `src/main.js`, HTML, APK o código distribuido. El cliente debería enviar la descripción/código al backend autenticado; el backend valida, limita el uso y llama al proveedor. Consulta la documentación oficial del proveedor elegido.

## Seguridad de código pegado
El código pegado se ejecuta en una vista previa `iframe` aislada. No lo ejecutes dentro del proceso principal de CreaApps ni le des acceso a tokens, archivos privados o credenciales. Para proyectos no confiables usa además límites de tamaño, revisión de dependencias, un entorno de compilación aislado y cuotas. No existe capacidad ni almacenamiento literalmente ilimitados.

## Qué no hace todavía
- No produce APK/AAB/IPA directamente desde el navegador.
- No tiene backend de cuentas, sincronización en nube ni generación IA conectada.
- No implementa GPS continuo en segundo plano.
- La vista previa es para aplicaciones web; código Kotlin/Swift necesita proyectos y compiladores nativos.

## Compilar el APK en la nube con GitHub Actions

Este proyecto incluye el flujo `.github/workflows/android-apk.yml` para compilar un APK de prueba en GitHub Actions, sin instalar Android Studio en tu teléfono.

1. Sube el contenido de esta carpeta a un repositorio de GitHub.
2. En GitHub, abre la pestaña **Actions** y habilita los workflows si te lo pide.
3. Entra en **Compilar APK Android** y pulsa **Run workflow** (si aparece la opción).
4. Cuando termine correctamente, abre la ejecución y descarga el artefacto **CreaApps-Android-APK**.
5. Descomprime el artefacto y encontrarás `app-debug.apk`. Pásalo a tu Android e instálalo; quizá tengas que permitir instalar aplicaciones desde esa fuente.

Es un APK de depuración para pruebas, no una versión firmada de publicación. La compilación en la nube no se ha ejecutado desde este entorno; el APK solo existirá después de que el workflow termine correctamente.
