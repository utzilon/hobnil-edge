# Hobnil — Instalador

Instalador del dispositivo **Hobnil** (terminal de control de accesos sobre
Raspberry Pi). Es un único binario autocontenido: aprovisiona una Pi nueva sin
clonar ningún repositorio.

## Uso

Descarga el binario para la arquitectura de tu Pi, dale permisos y ejecútalo con
`sudo`:

```bash
# Raspberry Pi OS de 64 bits (aarch64) — Pi Zero 2 W, Pi 3/4/5:
curl -fsSLO https://github.com/utzilon/hobnil-installer/releases/latest/download/install-arm64.bin
chmod +x install-arm64.bin
sudo ./install-arm64.bin

# Raspberry Pi OS de 32 bits (armhf):
#   usa install-armhf.bin en su lugar
```

¿No sabes qué arquitectura tienes?

```bash
uname -m   # aarch64 -> arm64   |   armv7l/armv6l -> armhf
```

Tras la instalación, si habilitó SPI por primera vez:

```bash
sudo reboot
```

## Opciones

Se pasan como variables de entorno antes del binario:

| Variable | Default | Efecto |
|----------|---------|--------|
| `WITH_PISUGAR`   | `1` | Instala el gestor de energía PiSugar |
| `WITH_TAILSCALE` | `1` | Instala Tailscale (luego `sudo tailscale up`) |
| `PISUGAR_MODEL`  | `PiSugar 2 (2-LEDs)` | Modelo de PiSugar |
| `NETADMIN_USER` / `NETADMIN_PASS` / `NETADMIN_PORT` | `admin` / aleatoria / `8080` | Panel de red |

Ejemplo (banco de pruebas, sin batería ni VPN):

```bash
sudo WITH_PISUGAR=0 WITH_TAILSCALE=0 ./install-arm64.bin
```

Es **idempotente**: re-ejecutarlo actualiza el dispositivo y conserva su
configuración.

## Verificar la descarga

```bash
sha256sum -c SHA256SUMS
```

## Requisitos

- Raspberry Pi con Raspberry Pi OS (basado en Debian).
- Conexión a internet durante la instalación.
- Ejecutar como `root` (`sudo`).

---

© Utzilon. Distribución del binario del dispositivo Hobnil.
