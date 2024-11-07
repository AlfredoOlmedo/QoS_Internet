1. **Definición y planificación de métricas**:
   
Vamos a definir cada una de las métricas y planificar los umbrales críticos y alertas para un sistema de monitoreo de QoS (Quality of Service) en la red. También estableceré un formato de exportación para Prometheus.

### 1. Definición de métricas

#### 1.1 Velocidad de Carga (Upload Speed) y Velocidad de Descarga (Download Speed)
- **Descripción**: Mide la cantidad de datos que pueden enviarse o recibirse en un tiempo específico. Se expresa en Mbps (Megabits por segundo).
- **Frecuencia de medición**: Cada 5 minutos (para detectar variaciones significativas en un intervalo corto).
- **Umbrales críticos**:
  - **Descarga**:
    - Advertencia: < 50 Mbps
    - Crítico: < 20 Mbps
  - **Carga**:
    - Advertencia: < 10 Mbps
    - Crítico: < 5 Mbps
- **Exportación para Prometheus**:
  ```plaintext
  qos_upload_speed_mbps{isp="ISP_name"} <valor>
  qos_download_speed_mbps{isp="ISP_name"} <valor>
  ```

#### 1.2 Ping/Latencia
- **Descripción**: El tiempo que toma para que un paquete de datos viaje desde el origen al destino y regrese. Se mide en milisegundos (ms).
- **Frecuencia de medición**: Cada 1 minuto.
- **Umbrales críticos**:
  - Advertencia: > 100 ms
  - Crítico: > 200 ms
- **Exportación para Prometheus**:
  ```plaintext
  qos_latency_ms{isp="ISP_name"} <valor>
  ```

#### 1.3 Jitter
- **Descripción**: Variabilidad en el tiempo de latencia de los paquetes, que puede afectar la estabilidad de la conexión, especialmente en aplicaciones en tiempo real como videollamadas.
- **Frecuencia de medición**: Cada 1 minuto.
- **Umbrales críticos**:
  - Advertencia: > 30 ms
  - Crítico: > 50 ms
- **Exportación para Prometheus**:
  ```plaintext
  qos_jitter_ms{isp="ISP_name"} <valor>
  ```

#### 1.4 Pérdida de Paquetes (Packet Loss)
- **Descripción**: Porcentaje de paquetes que se pierden en la transmisión, lo que afecta directamente la calidad y fiabilidad de la conexión.
- **Frecuencia de medición**: Cada 1 minuto.
- **Umbrales críticos**:
  - Advertencia: > 1%
  - Crítico: > 3%
- **Exportación para Prometheus**:
  ```plaintext
  qos_packet_loss_percentage{isp="ISP_name"} <valor>
  ```

#### 1.5 Estabilidad de Conexión
- **Descripción**: Mide la frecuencia de desconexiones breves o interrupciones en la conexión. Esta métrica es acumulativa y se reinicia en cada período de monitoreo (por ejemplo, cada 24 horas).
- **Frecuencia de medición**: Cada 5 minutos (registro acumulativo diario).
- **Umbrales críticos**:
  - Advertencia: > 1 desconexión en 24 horas
  - Crítico: > 3 desconexiones en 24 horas
- **Exportación para Prometheus**:
  ```plaintext
  qos_connection_stability_events{isp="ISP_name"} <valor>
  ```

### 2. Planificación de alertas

Para cada métrica, configuraremos alertas en Prometheus/Grafana:

- **Advertencia**: Indicará problemas potenciales o degradación en la calidad de la red. Estas alertas deberían enviarse al equipo de monitoreo o DevOps para revisión temprana.
- **Crítico**: Indicará problemas severos que probablemente impacten el rendimiento general de la red. Estas alertas deben enviarse inmediatamente a los responsables de incidentes.

### Ejemplo de alertas en Prometheus

Para configurar alertas en Prometheus, podemos usar reglas de alerta. A continuación, unos ejemplos:

#### Alerta de Velocidad de Descarga Crítica
```yaml
alert: DownloadSpeedCritical
expr: qos_download_speed_mbps < 20
for: 5m
labels:
  severity: critical
annotations:
  summary: "Velocidad de descarga crítica"
  description: "La velocidad de descarga es inferior a 20 Mbps por más de 5 minutos"
```

#### Alerta de Latencia Crítica
```yaml
alert: LatencyCritical
expr: qos_latency_ms > 200
for: 1m
labels:
  severity: critical
annotations:
  summary: "Latencia crítica"
  description: "La latencia es superior a 200 ms por más de 1 minuto"
```

#### Alerta de Pérdida de Paquetes Crítica
```yaml
alert: PacketLossCritical
expr: qos_packet_loss_percentage > 3
for: 1m
labels:
  severity: critical
annotations:
  summary: "Pérdida de paquetes crítica"
  description: "La pérdida de paquetes es superior al 3% por más de 1 minuto"
```

#### Alerta de Estabilidad de Conexión Crítica
```yaml
alert: ConnectionStabilityCritical
expr: qos_connection_stability_events > 3
for: 24h
labels:
  severity: critical
annotations:
  summary: "Problemas de estabilidad de conexión"
  description: "Más de 3 desconexiones en las últimas 24 horas"
```

### Resumen del formato de exportación para Prometheus

Cada métrica se exportará con el siguiente formato:
- Nombre de la métrica en formato `qos_<nombre_metrica>_<unidad>`.
- Etiqueta `isp` que permite identificar al proveedor de servicio.

Por ejemplo:
```plaintext
qos_upload_speed_mbps{isp="ISP_name"} <valor>
qos_latency_ms{isp="ISP_name"} <valor>
qos_packet_loss_percentage{isp="ISP_name"} <valor>
```

Esta estructura es flexible y permite agregar más etiquetas, como `location`, en caso de que se monitoreen múltiples ubicaciones. ¿Quieres que avancemos en algún aspecto específico, como los scripts de pruebas para obtener estos datos?