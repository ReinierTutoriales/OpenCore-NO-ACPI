# OpenCore sin inyección ACPI 🚀
Guía para evitar inyección ACPI (SSDTs/parches) y ciertos quirks fuera de macOS.

---

## ℹ️ Acerca de
**OpenCore sin inyección ACPI** es una bifurcación *no oficial* de OpenCore (no respaldada por **Acidanthera**). El proyecto original es de **btwise** y los builds más usados se publican en **OpenCore_NO_ACPI_Build**.

Su propósito es simple: **evitar la inyección de ACPI en sistemas distintos a macOS**. Esto ayuda cuando tu EFI (pensada para macOS) provoca problemas en otros sistemas, especialmente **Windows** (incluyendo BSOD en algunos casos).

> Si tu EFI está correctamente diseñada (inyección selectiva por SO o configuración correcta), normalmente **no necesitas** esta bifurcación.

---

## ✅ Requisitos previos
1. Una EFI funcional y un `config.plist` ya configurado.
2. **Obligatorio:** la versión del fork debe **coincidir** con la versión de tu OpenCore.
   - Ejemplo: OpenCore **0.9.7** → fork **0.9.7**
   - Si no coincide, suelen aparecer errores de validación y fallos.

---

## 🧠 Cómo funciona
Este fork añade el quirk **`EnableForAll`** en dos rutas:

- `ACPI/Quirks/EnableForAll`
- `Booter/Quirks/EnableForAll`

Cuando `EnableForAll = false`:
- **No se inyectan** parches ACPI/SSDTs fuera de macOS.
- **No se aplican** ciertos comportamientos del bloque Booter fuera de macOS (según el fork).

---

## 🛠️ Instalación paso a paso
1. **Haz backup** de tu EFI actual (recomendado en un USB FAT32).
2. Descarga la release que **coincida con tu versión de OpenCore**:
   - https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases
3. Extrae el ZIP y reemplaza en tu EFI estos archivos:
   - `EFI/Boot/BootX64.efi`
   - `EFI/OC/OpenCore.efi`
4. Reemplaza también (solo si corresponde a tu setup):
   - `EFI/OC/Drivers` (los drivers que realmente uses)
   - `EFI/OC/Tools` (tus herramientas)
5. Edita tu `config.plist` y añade/ajusta estas claves:
   - `ACPI -> Quirks -> EnableForAll` = `False`
   - `Booter -> Quirks -> EnableForAll` = `False`
6. Guarda cambios y reinicia.

---

## 🔍 Verificación en Windows
1. Arranca **Windows** desde el picker de OpenCore.
2. Ejecuta **HWiNFO**:
   - https://sourceforge.net/projects/hwinfo/
3. Revisa “System Manufacturer / Model” (o equivalente):
   - Debe mostrar el fabricante/modelo real de tu placa/portátil.
   - Si ves **“Acidanthera”** o el modelo del Mac de tu SMBIOS, entonces la inyección/identidad **sigue activa** (config incorrecta o no aplicado el fork).

---

## 💡 TIP: Solo quieres evitar SMBIOS en Windows (sin usar el fork)
Si tu objetivo es únicamente evitar que Windows vea identidad “Mac”, en OpenCore oficial normalmente basta con:

- `Kernel/Quirks/CustomSMBIOSGuid` = `True`
- `PlatformInfo/SMBIOS/UpdateSMBIOSMode` = `Custom`

Guarda y reinicia.

---

## 🌐 Recursos
- Builds (releases): https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases  
- Proyecto original (referencia): https://gitee.com/btwise/OpenCore_NO_ACPI
