# 🗄️ Docker NAS Server (NFS + Samba)

Este proyecto proporciona un **servidor NAS sencillo usando Docker** que
permite compartir carpetas en red mediante:

-   **NFS** → ideal para sistemas **Linux**
-   **Samba (SMB)** → ideal para **Windows, macOS y acceso de
    invitados**

El sistema permite tener un **servidor de archivos doméstico o de
laboratorio** con acceso desde distintos sistemas operativos.

------------------------------------------------------------------------

# 📦 Características

-   Servidor **NFS** para clientes Linux
-   Servidor **Samba / SMB** para clientes Windows
-   Compartición de carpetas privadas y públicas
-   Acceso de invitados
-   Despliegue rápido con **Docker Compose**
-   Uso de carpetas del host como almacenamiento

------------------------------------------------------------------------

# 🧱 Arquitectura

El sistema levanta **dos contenedores Docker**:

  Servicio   Imagen                   Uso
  ---------- ------------------------ ---------------------------
  NFS        `erichough/nfs-server`   Compartición para Linux
  Samba      `dperson/samba`          Compartición para Windows

------------------------------------------------------------------------

# 📁 Estructura de Carpetas

Ejemplo de estructura en el host:

    /home
     ├── carpeta_compartida_red
     │   ├── invitado
     │   └── manuel
     │       └── data
     │
     ├── manuel
     │   ├── Escritorio
     │   └── Descargas

Estas carpetas se comparten en red mediante NFS y Samba.

------------------------------------------------------------------------

# 🚀 Instalación

## 1️⃣ Clonar el repositorio

``` bash
git clone https://github.com/usuario/docker-nas-server.git
cd docker-nas-server
```

------------------------------------------------------------------------

## 2️⃣ Crear las carpetas necesarias

``` bash
mkdir -p /home/carpeta_compartida_red/invitado
mkdir -p /home/carpeta_compartida_red/manuel/data
```

------------------------------------------------------------------------

## 3️⃣ Ejecutar el servidor

``` bash
docker compose up -d
```

------------------------------------------------------------------------

## 4️⃣ Verificar que los contenedores estén funcionando

``` bash
docker ps
```

------------------------------------------------------------------------

# 🔗 Acceso desde clientes

## Linux (NFS)

Montar el recurso compartido:

``` bash
sudo mount -t nfs IP_SERVIDOR:/mnt/nas /mnt/nas
```

Ejemplo:

``` bash
sudo mount -t nfs 192.168.1.100:/mnt/nas /mnt/nas
```

------------------------------------------------------------------------

## Windows (Samba)

Abrir el explorador de archivos y acceder a:

    \\IP_SERVIDOR

Ejemplo:

    \\192.168.1.100

------------------------------------------------------------------------

# 📂 Recursos compartidos

## Privado

Nombre:

    Privado-Manuel

Ruta:

    /mnt/nas/manuel

Usuario:

    manuel

------------------------------------------------------------------------

## Público

Nombre:

    Publico-Invitado

Ruta:

    /mnt/nas/invitado

Acceso:

    sin contraseña

------------------------------------------------------------------------

# ⚙️ Configuración

Toda la configuración se encuentra en:

    docker-compose.yml

### Usuario Samba

    -u "manuel;contrasena"

Formato:

    usuario;contraseña

------------------------------------------------------------------------

### Carpetas exportadas por NFS

Ejemplo:

    NFS_EXPORT_0=/mnt/nas/manuel *(rw,sync,no_subtree_check,all_squash,anonuid=1000,anongid=1000)

Esto permite acceso de lectura y escritura a la red.

------------------------------------------------------------------------

# 🔐 Seguridad (Recomendado)

Para entornos de producción se recomienda:

-   Limitar acceso por IP en NFS
-   Usar firewall
-   No usar `network_mode: host` si no es necesario
-   Usar contraseñas seguras
-   Crear redes Docker dedicadas

------------------------------------------------------------------------

# 🧰 Comandos útiles

Ver logs:

``` bash
docker logs samba_nas
```

``` bash
docker logs nfs_nas
```

Detener los contenedores:

``` bash
docker compose down
```

Reiniciar:

``` bash
docker compose restart
```

------------------------------------------------------------------------

# 📜 Licencia

MIT License
