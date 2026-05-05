## wxGlade Light Theme Wrapper

A shell script wrapper to force wxGlade to render with a light GTK theme and dark icons on dark desktop environments (like KDE Plasma). This resolves the issue of invisible/white icons blending into light backgrounds when the desktop environment attempts to inject global dark-theme assets into GTK applications.

## Installation

1. Deploy the script to your local binaries path:

```bash
sudo tee /usr/local/bin/wxglade-light >/dev/null <<'EOF'
#!/bin/sh
set -eu

export XDG_CURRENT_DESKTOP=GNOME
export GTK_THEME=Adwaita:light

ISOLATED_CONF="$HOME/.wxglade_light_config"
mkdir -p "$ISOLATED_CONF/gtk-3.0"

cat <<EOCONF> "$ISOLATED_CONF/gtk-3.0/settings.ini"
[Settings]
gtk-icon-theme-name=breeze
gtk-application-prefer-dark-theme=0
EOCONF

export XDG_CONFIG_HOME="$ISOLATED_CONF"

exec /usr/bin/python3.14 /apps/wxGlade-1.1.1/wxglade.py "$@"
EOF
```

2. Make the script executable:

```bash
sudo chmod +x /usr/local/bin/wxglade-light
```

*(Note: Adjust the Python executable and wxGlade installation paths in the `exec` line according to your specific environment).*

## Usage

Run the wrapper directly from the terminal:
```bash
wxglade-light
```

### PyCharm External Tool Integration

To use this wrapper as an External Tool in PyCharm, use the following configuration:

* **Program:** `/usr/local/bin/wxglade-light`
* **Arguments:** *(leave blank)*
* **Working directory:** `$ProjectFileDir$`
