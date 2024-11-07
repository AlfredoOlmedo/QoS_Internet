# QoS Internet

## Proyecto
Objetivo:
- Monitorizar y asegurar la calidad del servicio de internet, usando procesos de SRE y DevOps 

Repositorio:
- El repositorio contiene la documentacioin del proceso de DevOps, con el codigo del proyecto, es open source asi que pueden descargalor, modificarlo etc.

## ¿Qué es QoS?
QoS se refiere a la capacidad de una red para proporcionar diferentes niveles de servicio a distintos tipos de tráfico de datos. Esto es crucial para garantizar que las aplicaciones críticas, como videoconferencias, VoIP (Voz sobre IP) y transmisión de video, funcionen de manera óptima incluso en condiciones de alta demanda de red.

## ¿Cómo funciona QoS?
QoS funciona mediante la clasificación y priorización del tráfico de red. Aquí están los pasos básicos:

1. Clasificación del Tráfico:
   - Los paquetes de datos se clasifican según su tipo y prioridad. Por ejemplo, el tráfico de voz y video puede recibir una prioridad más alta que el tráfico de correo electrónico o navegación web.

2. Asignación de Ancho de Banda:
   - Se asigna una cantidad específica de ancho de banda a diferentes tipos de tráfico para asegurar que las aplicaciones críticas tengan los recursos necesarios.

3. Control de Congestión:
   - Durante periodos de alta demanda, QoS gestiona la congestión de la red priorizando el tráfico más importante y asegurando que no haya demoras significativas.

4. Minimización de Latencia y Jitter:
   - QoS ayuda a reducir la latencia (tiempo de retraso) y el jitter (variabilidad en el tiempo de latencia), lo cual es crucial para aplicaciones en tiempo real.

## ¿Para qué sirve QoS?
QoS es esencial para:

- Videoconferencias y VoIP: Asegura que las llamadas de voz y video sean claras y sin interrupciones.
- Transmisión de Video: Garantiza una experiencia de visualización fluida sin buffering.
- Juegos en Línea: Reduce la latencia y el jitter, mejorando la experiencia de juego.
- Aplicaciones Empresariales: Optimiza el rendimiento de aplicaciones críticas para los negocios, mejorando la productividad y la eficiencia.

En resumen, QoS es una herramienta vital para gestionar el tráfico de red y asegurar que las aplicaciones y servicios más importantes funcionen de manera eficiente y sin interrupciones.




## Proceso DevOps

1. Plan
2. Desarrollo continuo
3. Integración Continua
4. Pruebas Continuas
5. Entrega Continua
6. Monitoreo Continuo
7. Retroalimentación Continua

# Plan
##  Definición de Objetivos
Los objetivos principales que se pueden alcanzar con la Calidad de Servicio (QoS) en Internet incluyen:

1. Priorizar el tráfico de red:
   - Asignar diferentes niveles de prioridad a distintos tipos de tráfico, asegurando que las aplicaciones críticas (como videoconferencias y VoIP) reciban el ancho de banda necesario para funcionar correctamente.

2. Minimizar la latencia:
   - Reducir el tiempo de retraso en la transmisión de datos, lo cual es crucial para aplicaciones en tiempo real como juegos en línea y videollamadas.

3. Controlar el jitter:
   - Mantener la variabilidad en el tiempo de latencia bajo control, garantizando una transmisión de datos más estable y predecible.

4. Optimizar el ancho de banda:
   - Gestionar eficientemente el uso del ancho de banda disponible, evitando la congestión de la red y asegurando que todas las aplicaciones tengan los recursos necesarios.

5. Reducir la pérdida de paquetes:
   - Minimizar el porcentaje de paquetes de datos que se pierden durante la transmisión, mejorando la calidad de la conexión y la experiencia del usuario.

6. Garantizar la calidad y velocidad de acceso:
   - Asegurar que todos los usuarios tengan una experiencia de navegación fluida y rápida, incluso durante periodos de alta demanda.

Estos objetivos ayudan a mejorar la eficiencia y la fiabilidad de la red, proporcionando una mejor experiencia de usuario y optimizando el rendimiento de las aplicaciones críticas.

## Gestión de Requisitos

La Gestión de Requisitos de Calidad de Servicio (QoS) en redes implica una serie de técnicas y protocolos destinados a garantizar que ciertos tipos de tráfico de datos reciban el tratamiento adecuado para cumplir con los niveles de servicio predefinidos. 

1. Identificación y Clasificación del Tráfico:
   - Marcado y Clasificación: Los paquetes de datos se marcan y clasifican según su tipo y prioridad. Esto permite a los dispositivos de red diferenciar entre tráfico crítico (como VoIP y videoconferencias) y tráfico menos urgente.

2. Asignación de Recursos:
   - Ancho de Banda: Se asigna una cantidad específica de ancho de banda a diferentes tipos de tráfico para asegurar que las aplicaciones críticas reciban los recursos necesarios.
   - Gestión de la Congestión: Durante periodos de alta demanda, los mecanismos de QoS priorizan el tráfico en función de su clasificación, garantizando que las aplicaciones críticas sigan funcionando sin problemas.

3. Control de Latencia y Jitter:
   - Minimización de Latencia: Se implementan técnicas para reducir el tiempo de retraso en la transmisión de datos, crucial para aplicaciones en tiempo real.
   - Control de Jitter: Se asegura que la variabilidad en el tiempo de latencia sea mínima, proporcionando una transmisión de datos más estable y predecible.

4. Garantías de QoS:
   - Acuerdos de Nivel de Servicio (SLA): Se establecen y aplican políticas de QoS que garantizan ciertas métricas de rendimiento para aplicaciones o grupos de usuarios específicos.
   - Monitoreo y Ajuste: Se monitorea continuamente el rendimiento de la red y se ajustan las políticas de QoS según sea necesario para mantener la calidad del servicio.

5. Optimización del Uso de la Red:
   - Uso Eficiente de Recursos: Los recursos de red se utilizan de manera más eficiente, asignando dinámicamente el ancho de banda no utilizado a aplicaciones de alta prioridad.
   - Mejora de la Experiencia del Usuario: Al priorizar el tráfico crítico, se mejora la experiencia del usuario final, asegurando llamadas más claras y transmisiones de video más fluidas.

Estos aspectos son fundamentales para garantizar que las redes puedan manejar eficientemente el tráfico de datos y proporcionar una experiencia de usuario de alta calidad.

### Arguitectura
#### Tecnologias 
- Linux
- Git
- Jenkins
- Docker
- Docker Compose
- Nginx
- Prometheus
- Grafana
- Digital Ocean
- Python


## Desarrollo continuo
- Definicion de Branches (Main, Dev, Prod)
- Codificación
- Revisiones de código

## Integración Continua (CI / CD)
- Automatización de la construcción
- Pruebas Unitarias
- Análisis estático

## Pruebas Continuas 
- Pruebas de Integración
- Pruebas funcionales
- Pruebas de Rendimiento
- Pruebas de Seguridad

## Entrega Continua
- Automatización del despliegue
- Gestión de la configuración

## Monitoreo Continuo
- Recopilación de métricas
- Análisis de Logs
- Alertas

## Retroalimentación Continua
- Revisión retrospectiva
- Mejora continua
