# Structure QoS Internet

```
project-qos-internet/
│
├── backend/                        # Backend del proyecto
│   ├── api/                        # API para datos de QoS
│   │   ├── __init__.py
│   │   ├── endpoints/              # Endpoints para métricas de red
│   │   │   ├── speed_test.py       # Endpoint para medir velocidad
│   │   │   ├── ping_latency.py     # Endpoint para latencia y ping
│   │   │   ├── jitter.py           # Endpoint para jitter
│   │   │   ├── packet_loss.py      # Endpoint para pérdida de paquetes
│   │   │   └── connection_stability.py  # Endpoint para estabilidad
│   │   ├── models/                 # Modelos de datos
│   │   │   └── metric_data.py      # Modelo para almacenar datos de métricas
│   │   └── utils/                  # Funciones auxiliares
│   │       ├── network_tests.py    # Funciones para pruebas de red
│   │       └── prometheus_metrics.py  # Funciones para exportar a Prometheus
│   ├── config.py                   # Configuración general del backend
│   └── app.py                      # Archivo principal de la API (Flask/FastAPI)
│
├── frontend/                       # Interfaz de usuario
│   ├── static/                     # Archivos estáticos
│   │   ├── css/
│   │   │   └── styles.css          # Estilos CSS personalizados
│   │   ├── js/
│   │   │   └── scripts.js          # JavaScript para interacción frontend
│   ├── templates/                  # Plantillas HTML
│   │   └── index.html              # Página principal
│   └── app.py                      # Controlador para la interfaz (si se necesita separar del backend)
│
├── monitoring/                     # Archivos de configuración para Prometheus y Grafana
│   ├── prometheus/                 # Configuración de Prometheus
│   │   ├── prometheus.yml          # Configuración de scraping y alertas
│   ├── grafana/                    # Configuración de Grafana
│   │   ├── dashboards/             # Dashboards de QoS personalizados
│   │   │   └── qos_dashboard.json  # Configuración de un dashboard de QoS
│   │   └── datasources.yaml        # Fuente de datos de Prometheus para Grafana
│
├── tests/                          # Pruebas unitarias y de integración
│   ├── test_api/                   # Pruebas de endpoints de API
│   │   ├── test_speed_test.py      # Pruebas de velocidad de carga
│   │   ├── test_latency.py         # Pruebas de latencia
│   │   ├── test_jitter.py          # Pruebas de jitter
│   │   └── test_packet_loss.py     # Pruebas de pérdida de paquetes
│   ├── test_integration/           # Pruebas de integración
│   │   └── test_endpoints.py       # Pruebas de integración de endpoints
│   └── test_frontend/              # Pruebas para la interfaz de usuario
│
├── scripts/                        # Scripts auxiliares para DevOps y SRE
│   ├── setup_prometheus.sh         # Script para configurar Prometheus
│   ├── setup_grafana.sh            # Script para configurar Grafana
│   ├── deploy.sh                   # Script de despliegue
│   └── monitor_network.sh          # Script para iniciar monitoreo de red
│
├── .env                            # Variables de entorno (configuración sensible)
├── README.md                       # Descripción y guía del proyecto
└── requirements.txt                # Dependencias de Python

```