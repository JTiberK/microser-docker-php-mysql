## 2. 🔹 Conectar SSH con EC2 Amazon Linux 

```powershell
ssh -i "mi-clave.pem" ec2-user@IP_PUBLICA
# Esta es la plantilla por ChatGPT
```
<br>

> [!TIP]
> ESTA ES EL PROMPT CORRECTO LANZADO EN POWERSHELL VS CODE

```powershell
ssh -i "labsuser.pem" ec2-user@44.201.122.34
# 
```
<br>

```powershell
[ec2-user@ip-172-31-90-231 ~]$ 
# Este es el prompt del servidor
```
## 3 🔹 Instalar y configurar el servidor web Apache dentro de DOCKER
    Una vez conectado a la instancia EC2, ejecuta los sigueintes comandos:

- #Actualizar librerias

```bash
sudo yum update –y
# A funcionado
```
- #Instalar Docker

```bash
sudo yum install -y docker
# A funcionado
```

- #Arrancar Docker

```bash
sudo service docker start
# A funcionado
```

- #Añadir nuestro usuario al grupo docker

```bash
sudo usermod -a -G docker $(whoami)
# A funcionado
```

- #Descargarse el binario para utilizar docker-compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# A funcionado
```

- #Dar permisos de ejecucion al archivo binario

```bash
sudo chmod +x /usr/local/bin/docker-compose
# A funcionado
```

- #Comprobar que docker compose esta instalado

```bash
docker-compose --version
# ERROR #~~~~~~~~
```

### Respuesta ChatGPT

- #Sí, ese error es común en sistemas como Amazon Linux 2 o algunas distribuciones minimalistas. El mensaje:
significa que docker-compose fue instalado como binario precompilado y depende de una librería del sistema `(libcrypt.so.1)` que no está instalada.

### Solución

- #Instala el paquete que contiene esa biblioteca:
Para Amazon Linux 2 o distribuciones similares a CentOS/RHEL:

```bash
sudo yum install -y libxcrypt-compat
# A funcionado
```

- #Comprobar que docker compose esta instalado

```bash
docker-compose --version
# A Funcionado []$ docker-compose version 1.29.2, build 5becea4c
```

### Se me olvidó

- #Instalar pip3
```bash
sudo yum install -y python3-pip
# A funcionado []$ pip3 --version
```

## 4. 🔹 OPCIÓN A POR CHATGPT: Usar Apache dentro de un contenedor Docker (recomendado si ya usas Docker)

- #✅ Paso 1: Crea la estructura de tu proyecto

```bash
mkdir -p ~/proyecto/html
cd ~/proyecto/html
echo "<h1>Hola desde Apache en Docker</h1>" > index.html
```

- #✅ Paso 2: Crea un archivo `docker-compose.yml`

```yaml
# ~/proyecto/docker-compose.yml
version: '3'
services:
  apache:
    image: httpd:2.4
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/local/apache2/htdocs/
```

- #✅ Paso 2.1: 📂 Sitúate en el directorio del proyecto (si no lo has hecho):

```bash
mkdir -p ~/proyecto/html
cd ~/proyecto
```

- #✅ Paso 2.2: 📝 Crea el archivo con nano:

```bash
nano docker-compose.yml
```

- #✅ Paso 2.3: 🧪 Luego, prueba levantando el contenedor: ⚠️ Si tu comando es docker-compose (con guion), usa:

```bash
sudo docker-compose up -d
# sin sudo no funciona
```

- #✅ Paso 2.4: 🧾 Verifica que está funcionando:

```bash
sudo docker ps
# sin sudo no funciona
```

- #✅ Paso 2.5: 🧭 Abre tu Navegador

```cpp
http://<IP_PUBLICA_DE_EC2>:8080

En mi caso >>> http://44.201.122.34:8080/
```