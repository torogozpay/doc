# Instalación y Configuración de Nexus Repository Manager 3 en Ubuntu 22.04

**Instalación y configuración de Nexus (repositorio de imagens para doockr)**

1. **Instalación de Nexus:**

```bash
wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
tar -xvzf latest-unix.tar.gz
sudo mv nexus-3.x.x-xx /data/nexus
```

2. **Configuración de Nexus:**

```bash
sudo nano /data/nexus/etc/nexus-default.properties
```

Modificar la propiedad `nexus-context-path`:

```properties
nexus-context-path=/nexus
```

Guardar y cerrar el archivo.

```bash
sudo /data/nexus/bin/nexus start
```

3. **Acceder a Nexus:**

   - URL: `http://tu_ip:8081/nexus`
   - Credenciales por defecto: `admin/admin123`

4. **Configuración de Nexus como un Servicio del Sistema (Opcional):**

```bash
sudo /data/nexus/bin/nexus stop
sudo nano /etc/systemd/system/nexus.service
```

Agregar el siguiente contenido:

```ini
[Unit]
Description=Nexus Repository Manager
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/data/nexus/bin/nexus start
ExecStop=/data/nexus/bin/nexus stop
User=nexus
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

Guardar y cerrar el archivo.

```bash
sudo systemctl start nexus
sudo systemctl enable nexus
sudo systemctl status nexus
```

Ahora, Nexus Repository Manager 3 está instalado y configurado en tu máquina virtual. Ajusta los comandos y configuraciones según tus necesidades.


# Configuración de HTTPS para Nexus y Uso de Docker con Nexus

## Configuración de HTTPS para Nexus:

1. **Generar Certificados SSL:**

   - Puedes utilizar herramientas como Let's Encrypt o generar certificados autofirmados para prueba.

2. **Configurar Nexus para HTTPS:**

\\\ash
sudo mkdir /etc/nexus/ssl
sudo cp ruta/al/certificado.crt ruta/a/clave-privada.key /etc/nexus/ssl/
sudo nano /opt/nexus/etc/nexus-default.properties
\\\

Modificar las siguientes propiedades:

\\\properties
application-port-ssl=8443
ssl.keystore=/ssl/keystore.jks
ssl.keystore.password=your_keystore_password
ssl.keystore.keyPassword=your_key_password
\\\

3. **Reiniciar Nexus:**

\\\ash
sudo systemctl restart nexus
\\\

4. **Abrir Puerto 8443 en el Firewall:**

\\\ash
sudo ufw allow 8443
\\\

Ahora, Nexus debería ser accesible a través de HTTPS.

## Uso de Docker con Nexus:

### Subir una Imagen a Nexus:

1. **Hacer Login en Nexus:**

\\\ash
docker login tu_ip:8443 -u tu_usuario -p tu_password
\\\

2. **Tag de la Imagen:**

\\\ash
docker tag nombre_de_la_imagen tu_ip:8443/repository/tu_repositorio/nombre_de_la_imagen
\\\

3. **Subir la Imagen:**

\\\ash
docker push tu_ip:8443/repository/tu_repositorio/nombre_de_la_imagen
\\\

### Descargar una Imagen desde Nexus:

\\\ash
docker pull tu_ip:8443/repository/tu_repositorio/nombre_de_la_imagen
\\\

Ajusta los comandos y configuraciones según tus necesidades.

