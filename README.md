<div align="center">
  <img src="assets/banner.svg" alt="Miguel - SysAdmin, Homelabber y DAW Developer" width="100%" />
</div>

<div align="center">

[![SMR](https://img.shields.io/badge/Titulado-SMR-3b82f6?style=flat-square&labelColor=0d1117)](#)
[![DAW](https://img.shields.io/badge/Cursando-DAW-8b5cf6?style=flat-square&labelColor=0d1117)](#)
[![Homelab](https://img.shields.io/badge/Homelab-activo-22c55e?style=flat-square&labelColor=0d1117&logo=docker&logoColor=white)](#)
[![Linux](https://img.shields.io/badge/Linux-Ubuntu_%7C_Debian-E95420?style=flat-square&labelColor=0d1117&logo=linux&logoColor=white)](#)
[![Minecraft](https://img.shields.io/badge/Minecraft-modded-388E3C?style=flat-square&labelColor=0d1117&logo=minecraft&logoColor=white)](#)

</div>

<br>

<div align="center">
  <img src="assets/terminal.svg" alt="Terminal con mi información" width="100%" />
</div>

<br>

<img src="assets/divider.svg" width="100%" alt="" />

## `~/sobre-mi`

```json
{
  "nombre": "Miguel",
  "roles": ["SysAdmin", "Homelabber", "DAW Developer"],
  "formacion": {
    "titulado": "SMR - Sistemas Microinformáticos y Redes",
    "cursando": "DAW - Desarrollo de Aplicaciones Web"
  },
  "obsesiones": ["Docker", "Linux", "redes", "organización personal"],
  "disfruta": [
    "montar hardware",
    "configurar switches UniFi",
    "optimizar sistemas Linux",
    "administrar servidores de Minecraft con mods"
  ],
  "lema": "romper cosas, arreglarlas y apuntar cómo lo hice"
}
```

> [!NOTE]
> Todo lo que aprendo en clase lo pruebo después en mi homelab, y todo lo que rompo en el homelab acaba documentado en mis apuntes.

<img src="assets/divider.svg" width="100%" alt="" />

## `~/skills`

**💻 Desarrollo**

<p>
  <img src="https://skillicons.dev/icons?i=java,js,html,css,mysql&theme=dark" alt="Java, JavaScript, HTML, CSS y MySQL" />
</p>

**🐧 Sistemas, contenedores y redes**

<p>
  <img src="https://skillicons.dev/icons?i=linux,ubuntu,debian,bash,docker,nginx,elasticsearch&theme=dark" alt="Linux, Ubuntu, Debian, Bash, Docker, Nginx y Elasticsearch" />
</p>

![WireGuard](https://img.shields.io/badge/WireGuard-881798?style=flat-square&logo=wireguard&logoColor=white)
![Tailscale](https://img.shields.io/badge/Tailscale-4B5563?style=flat-square&logo=tailscale&logoColor=white)
![UniFi](https://img.shields.io/badge/UniFi-0559C9?style=flat-square&logo=ubiquiti&logoColor=white)
![Portainer](https://img.shields.io/badge/Portainer-13BEF9?style=flat-square&logo=portainer&logoColor=white)
![ELK](https://img.shields.io/badge/ELK_Stack-005571?style=flat-square&logo=elastic&logoColor=white)

**🛠 Herramientas y productividad**

<p>
  <img src="https://skillicons.dev/icons?i=vscode,git,github,obsidian,notion&theme=dark" alt="VS Code, Git, GitHub, Obsidian y Notion" />
</p>

<img src="assets/divider.svg" width="100%" alt="" />

## `~/homelab`

Un servidor casero sobre **Ubuntu Server** que funciona como mi campo de pruebas.

```mermaid
flowchart LR
    subgraph EXT[Fuera de casa]
        ME([Yo])
    end
    subgraph NET[Red privada cifrada]
        TS{{Tailscale / WireGuard}}
    end
    subgraph SRV[Ubuntu Server + Docker]
        DNS[AdGuard Home]
        NPM[Nginx Proxy Manager]
        subgraph UI[Paneles]
            HU[Homepage User]
            HA[Homepage Admin]
        end
        subgraph APPS[Servicios]
            NC[Nextcloud]
            VW[Vaultwarden]
            NO[NocoDB]
            MC[Minecraft]
        end
        PT[Portainer]
        KO[(Kopia)]
    end
    ME --> TS --> NPM
    DNS -.-> NPM
    NPM --> UI
    NPM --> APPS
    HA --> PT
    APPS -.->|snapshots| KO

    style TS fill:#8b5cf6,stroke:#c4b5fd,color:#fff
    style NPM fill:#3b82f6,stroke:#93c5fd,color:#fff
    style KO fill:#22c55e,stroke:#86efac,color:#fff
```

```text
$ docker ps --format "table {{.Names}}\t{{.Image}}"
NAMES                  ROL
nginx-proxy-manager    Proxy inverso
adguard-home           DNS local y filtrado
portainer              Gestión de contenedores
homepage-user          Panel de usuario
homepage-admin         Panel de administración
nextcloud              Nube personal
vaultwarden            Gestor de contraseñas
nocodb                 Bases de datos con interfaz visual
kopia                  Copias de seguridad
minecraft              Servidor con mods
```

<details>
<summary><b>🎨 Detalle: los dos Homepage con estilo glassmorphism</b></summary>

<br>

Monté **dos paneles** con [Homepage](https://gethomepage.dev/) para separar lo cotidiano de lo crítico:

| Panel | Para qué sirve |
|---|---|
| 🙋 **User** | Accesos a servicios del día a día, sin exponer nada de administración |
| 🛡 **Admin** | Portainer, Kopia, Nginx Proxy Manager, métricas del host y estado de contenedores |

El diseño usa `custom.css` con `backdrop-filter: blur`, bordes translúcidos y tarjetas flotantes. Curiosidad: Homepage está hecha con Next.js, así que no hay ningún `.html` que editar; todo se controla con `.yaml` y CSS.

</details>

<img src="assets/divider.svg" width="100%" alt="" />

## `~/camino`

```mermaid
timeline
    title Mi recorrido
    SMR : Sistemas, redes y hardware
        : Linux y administración
    Homelab : Docker y proxy inverso
            : VPN mesh y copias de seguridad
            : Servidores de Minecraft con mods
    DAW : Java, HTML, CSS y JavaScript
        : Bases de datos con MySQL
    Siguiente : Unir sistemas y desarrollo
```

<img src="assets/divider.svg" width="100%" alt="" />

## `~/retos-resueltos`

Los errores que más me han enseñado, y cómo salí de ellos:

<details>
<summary><b>🔥 Nginx no arrancaba por un upstream huérfano</b></summary>

<br>

Tras reorganizar contenedores, quedó un proxy host apuntando a un servicio de autenticación que ya no existía y Nginx abortaba el arranque.

```text
nginx: [emerg] host not found in upstream "authelia"
```

**Solución:** probar la configuración con `nginx -t`, renombrar el archivo afectado a `.disabled` y recargar en caliente con `nginx -s reload`.

</details>

<details>
<summary><b>🌐 Conectar a mi red privada desde una red restringida</b></summary>

<br>

Desde una red con proxy y cortafuegos fallaron, uno tras otro, la autenticación (403), un registro de nodo duplicado y el DNS.

- **403 en el login:** hacerlo desde otra red o saltarse el navegador con una Auth Key.
- **`nodekey already exists`:** `tailscale logout` y reautenticar con `--force-reauth`.
- **Sin internet al conectar:** evitar que la VPN sustituya el DNS con `--accept-dns=false`.

**Lección:** probar por IP antes que por dominio separa los problemas de red de los de DNS.

</details>

<details>
<summary><b>💾 Que las copias de seguridad no mientan</b></summary>

<br>

Al mover y borrar carpetas, las políticas de Kopia apuntaban a rutas que ya no existían. Limpié los snapshots huérfanos y lancé uno manual para dejar guardado el estado bueno.

**Regla:** cada reorganización termina con revisión de rutas y un snapshot manual.

</details>

<details>
<summary><b>🧱 Permisos rotos y base de datos en solo lectura</b></summary>

<br>

Borrar carpetas que alojaban volúmenes de Docker dejó a Nginx Proxy Manager con la base de datos en modo lectura (errores 502 y 403).

**Solución:** parar el stack, reajustar propietarios y permisos, limpiar redes huérfanas y volver a levantarlo.

</details>

<img src="assets/divider.svg" width="100%" alt="" />

## `~/mapa-mental`

```mermaid
mindmap
  root((Miguel))
    Sistemas
      Ubuntu Server
      Debian
      Docker
      Nginx Proxy Manager
    Redes
      UniFi
      Tailscale
      WireGuard
      DNS con AdGuard
    Desarrollo
      Java
      HTML y CSS
      JavaScript
      MySQL
    Productividad
      Obsidian
      Notion
      Git y GitHub
    Ocio
      Minecraft con mods
      Montar hardware
```

<img src="assets/divider.svg" width="100%" alt="" />

## `~/ahora`

| 📚 Estudiando | 🔬 Practicando | 🧠 Organizando |
|:---:|:---:|:---:|
| Grado Superior en **DAW** | Docker, redes, VPN y backups en mi homelab | Apuntes en **Obsidian** y bases de datos en **Notion** |

<br>

<div align="center">
  <sub>💜 Gracias por pasarte por mi perfil · Siempre aprendiendo, siempre rompiendo y arreglando cosas</sub>
</div>

<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:8b5cf6,100:3b82f6&height=120&section=footer" alt="" width="100%" />
</div>
