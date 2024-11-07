# Docker Dependencias

- Nginx
- Prometheus
- Grafana
- Jenkins
- Blackbox
- Node-Exporter
- Pyhton-Env

# Docker Compose

docker-compose.yml
```
version: '3'

services:
  nginx:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./nginx:/etc/nginx/conf.d
    networks:
      - monitoring

  prometheus:
    image: prom/prometheus:latest
    volumes:
      - ./prometheus:/etc/prometheus
    ports:
      - "9090:9090"
    networks:
      - monitoring
    depends_on:
      - blackbox-exporter
      - node-exporter

  grafana:
    image: grafana/grafana:latest
    volumes:
      - ./grafana:/var/lib/grafana
    ports:
      - "3000:3000"
    networks:
      - monitoring

  jenkins:
    image: jenkins/jenkins:lts
    ports:
      - "8080:8080"
    volumes:
      - ./jenkins:/var/jenkins_home
    networks:
      - monitoring

  blackbox-exporter:
    image: prom/blackbox-exporter:latest
    ports:
      - "9115:9115"
    networks:
      - monitoring

  node-exporter:
    image: prom/node-exporter:latest
    ports:
      - "9100:9100"
    networks:
      - monitoring

  app:
    build: ./app
    ports:
      - "5000:5000"
    networks:
      - monitoring
    depends_on:
      - prometheus
      - grafana
      - nginx

  python-env:
    image: python:3.9
    volumes:
      - ./app:/app
    working_dir: /app
    command: bash -c "python -m venv venv && source venv/bin/activate && pip install -r requirements.txt"
    networks:
      - monitoring

networks:
  monitoring:
    driver: bridge
```


















# Despliegue en DigitalOcean
Una vez que hayas configurado y probado tu entorno localmente, haz el deploy con DigitalOcean

1. Crea un Droplet en DigitalOcean con Ubuntu instalado.
2. Configura Docker en tu Droplet siguiendo los mismos pasos que hiciste en tu entorno local.
3. Transfiere tu proyecto al Droplet utilizando scp o Git.
4. Ejecuta Docker Compose en tu Droplet:
```
Copy code
docker-compose up -d
```

# Integración con Jenkins
- Configura Jenkins para ejecutar tus pipelines de CI/CD. - 
- Configuracion de Jenkins para construir y desplegar tu aplicación en Docker automáticamente.




### 1. **Instalación y Configuración de Jenkins**

Ya que has incluido Jenkins en tu archivo `docker-compose.yml`, asegúrate de que Jenkins esté corriendo.

Accede a Jenkins en `http://<tu-ip>:8080` y sigue los pasos iniciales para configurarlo:

1. **Desbloquea Jenkins:**
   - El primer inicio de Jenkins te pedirá un token que puedes obtener ejecutando:
     ```bash
     docker exec -it <jenkins_container_name> cat /var/jenkins_home/secrets/initialAdminPassword
     ```
   - Usa este token para desbloquear Jenkins en la interfaz web.

2. **Instala los Plugins Necesarios:**
   - Una vez que hayas desbloqueado Jenkins, te sugerirá instalar un conjunto de plugins. Asegúrate de que los siguientes estén instalados:
     - **Docker**: Para gestionar contenedores Docker.
     - **Pipeline**: Para definir pipelines como código.
     - **Git**: Para clonar y manejar repositorios Git.
     - **GitHub**: Si estás utilizando GitHub.

### 2. **Configurar Jenkins para Docker**

1. **Configurar Docker en Jenkins:**

   - En la página de configuración de Jenkins, ve a **Manage Jenkins > Manage Plugins > Available Plugins** y busca el plugin **Docker** e instálalo.

   - Luego, ve a **Manage Jenkins > Configure System** y busca la sección de **Cloud**.

   - Añade una nueva nube de tipo **Docker** y configura la URL del Docker Host, que normalmente es `unix:///var/run/docker.sock` si Docker está corriendo en el mismo servidor que Jenkins.

2. **Configuración de Credenciales:**

   - Si necesitas autenticarte en Docker Hub o algún registro privado, ve a **Manage Jenkins > Manage Credentials** y añade las credenciales necesarias (Docker Hub, GitHub, etc.).

### 3. **Crear un Pipeline en Jenkins**

Para automatizar la construcción y el despliegue, puedes crear un pipeline. Aquí te muestro un ejemplo básico de un `Jenkinsfile` que puedes agregar en la raíz de tu repositorio Git:

```groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'tu-imagen:latest'
    }

    stages {
        stage('Clonar Repositorio') {
            steps {
                git 'https://github.com/tu-usuario/tu-repo.git'
            }
        }
        stage('Construir Imagen Docker') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Publicar Imagen Docker') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }
        stage('Desplegar en Producción') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image(DOCKER_IMAGE).run('-d --name tu-contenedor -p 80:80')
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
```

### 4. **Explicación del `Jenkinsfile`:**

- **pipeline**: Define el pipeline como código.
- **agent any**: Indica que el pipeline puede ejecutarse en cualquier agente disponible.
- **environment**: Define variables de entorno, como el nombre de la imagen Docker.
- **stages**: Secciones donde se definen las etapas del pipeline:
  - **Clonar Repositorio**: Clona tu repositorio desde GitHub o cualquier otro origen Git.
  - **Construir Imagen Docker**: Construye la imagen Docker desde tu `Dockerfile`.
  - **Publicar Imagen Docker**: Publica la imagen en Docker Hub (requiere credenciales configuradas).
  - **Desplegar en Producción**: Despliega el contenedor en el entorno de producción.

### 5. **Crear el Pipeline en Jenkins:**

1. **Ve a Jenkins** y selecciona **Nuevo Item**.
2. **Elige Pipeline** y dale un nombre.
3. En la configuración del Pipeline, selecciona **Pipeline script from SCM** si quieres usar un `Jenkinsfile` desde tu repositorio Git. Configura la URL de tu repositorio y la rama que quieres construir.

4. **Guarda** la configuración y ejecuta el Pipeline para probar.

### 6. **Automatización con Webhooks (Opcional)**

Si estás usando GitHub, puedes configurar un webhook para que cada vez que hagas un push al repositorio, Jenkins dispare automáticamente el pipeline. Para hacerlo:

1. Ve a la configuración de tu repositorio en GitHub.
2. Añade un webhook con la URL de tu Jenkins: `http://<tu-jenkins-url>/github-webhook/`.
3. Configura Jenkins para que escuche los webhooks en la sección **Build Triggers** del proyecto.

### 7. **Monitoreo y Logs**

Una vez que tu pipeline esté en funcionamiento, puedes monitorear el progreso y revisar los logs directamente desde la interfaz de Jenkins. Cada etapa del pipeline tendrá sus propios logs, lo que facilita la depuración.

Con esto, tienes una configuración básica para construir y desplegar automáticamente tu aplicación en Docker utilizando Jenkins.






















Accede a Jenkins en http://<tu-ip>:8080
Instala los plugins necesarios como Docker, Git, y Python.
Configura tus pipelines en Jenkins para ejecutar scripts de despliegue y pruebas.