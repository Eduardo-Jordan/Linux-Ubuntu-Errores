# Linux-Ubuntu-Error- `(initramfs)`
# Solución al Error de Arranque: BusyBox / `(initramfs)` en Ubuntu

Este repositorio documenta el fallo común de arranque en sistemas con Dual Boot (Windows/Ubuntu) donde el sistema operativo Linux no logra iniciar el entorno de escritorio y se detiene en la consola de comandos de `BusyBox` con el prompt `(initramfs)`.

## 📸 Evidencia del Error

Si tu pantalla se veía así al encender el equipo, significa que el sistema de archivos sufrió una corrupción leve:

![Pantalla de Error BusyBox (initramfs)](ruta/a/tu/imagen1.jpg)

---

## 🔍 ¿Por qué ocurre este error?

El mensaje `UNEXPECTED INCONSISTENCY; RUN fsck MANUALLY` en la partición `/dev/sda6` ocurre principalmente por las siguientes razones:
1. **Apagado forzado:** Apagar la computadora manteniendo presionado el botón de encendido mientras Ubuntu aún realizaba tareas en segundo plano.
2. **Corte de energía:** Una pérdida repentina de luz eléctrica que interrumpió la escritura de datos en el disco.
3. **Fallas o desgaste en el disco duro:** Sectores defectuosos en el disco (especialmente común en discos mecánicos HDD antiguos).

Al detectar esta inconsistencia, por seguridad, Linux bloquea el montaje automático del sistema de archivos para evitar la pérdida o sobreescritura de tus datos, enviándote a la consola de emergencia.

---

## 🛠️ Solución Paso a Paso

Para reparar la estructura de archivos interna y volver a entrar al escritorio sin perder información, se deben ejecutar los siguientes comandos directamente en el prompt `(initramfs)`:

### Paso 1: Reparar la partición afectada
Ejecuta el comando de verificación apuntando específicamente a la partición de Ubuntu dañada (en este caso, `/dev/sda6`):

```bash
fsck /dev/sda6 -y

¿Qué hace este comando?

fsck (File System Consistency Check) escanea y repara los errores del disco.

El parámetro -y responde automáticamente "Sí" (yes) a todas las confirmaciones para corregir los bloques de memoria corruptos ("garbage").

### Paso 2: Salir de la consola de emergencia y arrancar el sistema
Una vez que el comando `fsck` termine de reparar todos los sectores y te devuelva el control de la pantalla, debes indicarle a la consola que reanude el inicio normal de la computadora. Para ello, escribe:

```bash
exit

¿Por qué usamos exit en lugar de reiniciar a la fuerza?
Al escribir exit, le estás diciendo formalmente al entorno de emergencia (BusyBox) que tu trabajo de reparación ha terminado.

En ese instante, el sistema realiza dos acciones automáticas:

Cierra de forma segura la sesión temporal en la memoria RAM.

Intenta montar de nuevo la partición /dev/sda6 (que ahora ya está limpia y sin errores). Al ver que todo está en orden, el sistema continúa con el proceso de arranque normal y te lleva directo a la pantalla de inicio de tu escritorio Ubuntu.
Con este ajuste, cualquiera que lea tu documentación entenderá perfectamente que `exit` no es solo "apagar", sino la orden para que Linux vuelva a comprobar que el disco ya funciona y te deje entrar al escritorio.
