# Hobnil — `hobnil.edge`

Gestor del dispositivo **Hobnil** (terminal de control de accesos sobre
Raspberry Pi). Un único binario autocontenido con subcomandos: instala,
actualiza, desinstala y reporta estado, **sin clonar ningún repositorio**.

```
sudo ./hobnil.edge install      # (default) aprovisiona/instala el dispositivo
sudo ./hobnil.edge update       # re-despliega la versión (idempotente)
sudo ./hobnil.edge uninstall    # quita servicios y binarios (conserva datos)
sudo ./hobnil.edge status       # estado de servicios, versión y panel
```

Durante `install` te pregunta lo que necesita: **modelo de PiSugar** y
**usuario + contraseña del panel de red** (netadmin).

## Descargar y ejecutar

```bash
# Raspberry Pi OS de 64 bits (aarch64) — Pi Zero 2 W, Pi 3/4/5:
curl -fsSLO https://github.com/utzilon/hobnil-edge/releases/latest/download/hobnil.edge-arm64
chmod +x hobnil.edge-arm64
sudo ./hobnil.edge-arm64 install
```

```bash
# Raspberry Pi OS de 32 bits (armhf):
curl -fsSLO https://github.com/utzilon/hobnil-edge/releases/latest/download/hobnil.edge-armhf
chmod +x hobnil.edge-armhf
sudo ./hobnil.edge-armhf install
```

¿No sabes tu arquitectura? `uname -m` → `aarch64` = arm64, `armv7l`/`armv6l` = armhf.

Si habilitó SPI por primera vez, al terminar: `sudo reboot`.

## Opciones (variables de entorno)

Útiles para instalación **desatendida** (saltan las preguntas):

| Variable | Efecto |
|----------|--------|
| `NETADMIN_USER` / `NETADMIN_PASS` | Credenciales del panel (si no, las pregunta) |
| `NETADMIN_PORT` | Puerto del panel (default `8080`) |
| `PISUGAR_MODEL` | Modelo exacto de PiSugar (si no, lo pregunta) |
| `WITH_PISUGAR=0` | No instala el gestor PiSugar |
| `WITH_TAILSCALE=0` | No instala Tailscale |
| `PURGE=1` | En `uninstall`, borra también datos y config (incluida la DB de eventos) |

Ejemplo desatendido (banco de pruebas):

```bash
sudo WITH_PISUGAR=0 WITH_TAILSCALE=0 NETADMIN_USER=admin NETADMIN_PASS=secreta ./hobnil.edge-arm64 install
```

## Verificar la descarga

```bash
curl -fsSLO https://github.com/utzilon/hobnil-edge/releases/latest/download/SHA256SUMS
sha256sum --ignore-missing -c SHA256SUMS
```

## Notas

- Ejecutar como `root` (`sudo`) para `install` / `update` / `uninstall`.
- `update` es idempotente: conserva la configuración y la DB de eventos.
- `uninstall` **conserva** los datos por defecto (la DB de accesos es auditable);
  usa `PURGE=1` para borrarlos. No toca PiSugar ni Tailscale.
- Requiere Raspberry Pi OS (Debian) y conexión a internet durante la instalación.

---

© Utzilon — binario del dispositivo Hobnil.
