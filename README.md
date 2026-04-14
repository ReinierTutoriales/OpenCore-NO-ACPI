# OpenCore sin inyección ACPI

> Fork no oficial de OpenCore que evita la inyección de SSDTs, parches ACPI y ciertos quirks fuera de macOS.

---

## ¿Qué es y para qué sirve?

Este fork añade el quirk `EnableForAll` que, cuando se establece en `false`, desactiva la inyección ACPI y ciertos comportamientos del bloque `Booter` en cualquier sistema operativo que no sea macOS.

**Caso de uso principal:** tienes una EFI funcional para macOS y experimentas problemas en Windows — incluyendo BSOD — causados por SSDTs o parches ACPI que no deberían aplicarse fuera de macOS.

> ℹ️ **Nota:** Si tu EFI ya usa inyección selectiva por sistema operativo (`_OSI ("Darwin")`), normalmente **no necesitas este fork**. Esta solución está pensada para quienes no quieren o no pueden reescribir sus SSDTs.

El proyecto original es de **btwise**. Los builds publicados y mantenidos están en **[wjz304/OpenCore_NO_ACPI_Build](https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases)**.

---

## Requisitos

- EFI funcional con `config.plist` ya configurado
- La versión del fork **debe coincidir exactamente** con tu versión de OpenCore

| Tu OpenCore | Fork a descargar |
|:---|:---|
| 0.9.7 | OpenCore_NO_ACPI 0.9.7 |
| 1.0.x | OpenCore_NO_ACPI 1.0.x |

> ⚠️ Una versión incorrecta genera errores de validación en el arranque.

---

## Cómo funciona

El fork expone dos nuevas claves en `config.plist`:

```
ACPI → Quirks → EnableForAll
Booter → Quirks → EnableForAll
```

| Valor | Comportamiento |
|:---|:---|
| `true` | OpenCore inyecta ACPI y aplica quirks en **todos** los sistemas (comportamiento estándar) |
| `false` | Inyección ACPI y quirks de Booter **solo activos en macOS** |

---

## Instalación

**1. Backup obligatorio**

Copia tu EFI actual a un USB FAT32 antes de continuar.

**2. Descarga el fork**

Descarga la release que coincida con tu versión de OpenCore:
[github.com/wjz304/OpenCore_NO_ACPI_Build/releases](https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases)

**3. Reemplaza los binarios principales**

```
EFI/Boot/BootX64.efi
EFI/OC/OpenCore.efi
```

**4. Reemplaza drivers y herramientas** *(solo los que uses)*

```
EFI/OC/Drivers/
EFI/OC/Tools/
```

**5. Edita `config.plist`**

Añade o ajusta las siguientes claves:

```xml
<!-- ACPI → Quirks -->
<key>EnableForAll</key>
<false/>

<!-- Booter → Quirks -->
<key>EnableForAll</key>
<false/>
```

**6. Guarda y reinicia.**

---

## Verificación en Windows

Arranca Windows desde el picker de OpenCore y abre **[HWiNFO](https://sourceforge.net/projects/hwinfo/)**.

En la sección **System Summary**, revisa el campo *System / Motherboard Manufacturer*:

| Lo que ves | Diagnóstico |
|:---|:---|
| Fabricante real de tu placa (ej. `ASUS`, `Gigabyte`) | ✅ Fork aplicado correctamente |
| `Acidanthera` o modelo de Mac (ej. `MacPro7,1`) | ❌ SMBIOS/ACPI siguen inyectándose — revisar configuración |

---

## Alternativa sin fork: solo evitar SMBIOS en Windows

Si el único objetivo es que Windows no vea la identidad Mac, OpenCore oficial lo resuelve sin necesidad del fork:

```xml
<!-- Kernel → Quirks -->
<key>CustomSMBIOSGuid</key>
<true/>

<!-- PlatformInfo → Generic -->
<key>UpdateSMBIOSMode</key>
<string>Custom</string>
```

Esto no afecta la inyección ACPI, solo el SMBIOS.

---

## Recursos

| Recurso | Enlace |
|:---|:---|
| Releases del fork | [wjz304/OpenCore_NO_ACPI_Build](https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases) |
| Proyecto original (btwise) | [gitee.com/btwise/OpenCore_NO_ACPI](https://gitee.com/btwise/OpenCore_NO_ACPI) |

---

<div align="center">
<sub>Guía por <a href="https://www.reiniertutoriales.com/">ReinierTutoriales</a></sub>
</div>
