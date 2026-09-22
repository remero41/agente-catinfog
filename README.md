# Agente de impresión Catinfog

Conecta el TPV de Catinfog con la impresora de tickets.

Se instala una vez, arranca solo al encender el ordenador y no hay que volver a tocarlo.

**[⬇️ Descargar la última versión](https://github.com/remero41/agente-catinfog/releases/latest)**

---

## Instalar

### Windows

Descarga `CatinfogAgenteImpresion-x.y.z.exe` y haz doble clic.

Windows mostrará un aviso de «editor desconocido» la primera vez: pulsa **Más información** y
**Ejecutar de todas formas**. El instalador pide permiso de administrador, instala el agente
como servicio y queda listo.

Para desinstalarlo, entra en **Configuración → Aplicaciones** y busca *Agente de impresión
Catinfog*.

### Debian, Ubuntu, Linux Mint

```bash
sudo dpkg -i agente-catinfog_x.y.z_amd64.deb
systemctl --user enable --now agente-catinfog
```

### Fedora, RHEL, openSUSE

```bash
sudo rpm -i agente-catinfog-x.y.z-1.x86_64.rpm
systemctl --user enable --now agente-catinfog
```

### Arch, Manjaro

```bash
sudo pacman -U agente-catinfog-x.y.z-1-x86_64.pkg.tar.zst
systemctl --user enable --now agente-catinfog
```

> **En Linux, un detalle que importa en una caja registradora.** El agente corre con la sesión
> del usuario, así que se para al cerrar sesión. Para que siga funcionando:
>
> ```bash
> sudo loginctl enable-linger $USER
> ```

---

## Comprobar que funciona

```bash
curl http://127.0.0.1:8909/estado
```

Debe responder algo como:

```json
{"ok":true,"version":"0.5.1","pendientes":0,"dudosos":0}
```

En Windows, si `curl` no está disponible, abre `http://127.0.0.1:8909/estado` en el navegador.

---

## Si algo no va

**El TPV dice que no encuentra el agente.**
La primera vez, el navegador pide permiso para acceder a la impresora de este equipo. Mira si
hay un aviso arriba en el navegador y pulsa **Permitir**. Si ya lo rechazaste, entra por el
candado de la barra de direcciones y permítelo ahí.

**El ticket no sale.**
Mira el papel y la tapa de la impresora. El agente guarda el ticket y lo saca en cuanto la
impresora vuelva. La venta siempre queda cobrada, aunque el ticket no salga.

**Ver qué ha pasado.**

```bash
curl http://127.0.0.1:8909/log
```

---

## Qué hace, por dentro

- **Escucha solo en tu propio ordenador** (`127.0.0.1`). No es accesible desde la red.
- **Habla con la impresora por su identidad**, no por el nombre de la cola de Windows: cambiar
  la impresora de puerto USB no deja de imprimir.
- **Guarda los tickets en una cola en disco.** Si la impresora está apagada o sin papel, el
  ticket no se pierde: sale cuando vuelve.
- **No imprime dos veces.** Cada ticket lleva un identificador; un reintento devuelve el
  resultado anterior en vez de sacar otra copia.
- **Cuando no está seguro, pregunta.** Si la impresora aceptó parte de un ticket y se paró, el
  agente no lo reimprime por su cuenta: lo deja pendiente para que una persona decida, porque
  duplicar un ticket es peor que no imprimirlo.

---

## Soporte

[catinfog.com](https://catinfog.com)
