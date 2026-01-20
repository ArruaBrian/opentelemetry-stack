# OpenTelemetry Stack Completo

Stack completo de OpenTelemetry con Jaeger, Prometheus y Grafana para monitoreo distribuido.

## 🚀 Arquitectura

```
Tus Aplicaciones → OpenTelemetry SDK → Collector → Jaeger (Traces) + Prometheus (Métricas) → Grafana (Dashboards)
```

## 📋 Componentes

- **OpenTelemetry Collector**: Recibe y procesa datos de telemetría
- **Jaeger**: Almacenamiento y visualización de traces distribuidos
- **Prometheus**: Recolección y almacenamiento de métricas
- **Grafana**: Dashboards y visualización (puerto 3200)

## 🔧 Requisitos

- Docker y Docker Compose
- 4GB+ RAM recomendado
- 2GB+ espacio en disco

## 🚀 Instalación Rápida

### 1. Clonar y ejecutar

```bash
git clone <URL-del-repo>
cd opentelemetry-stack
docker-compose up -d
```

### 2. Verificar que todo está funcionando

```bash
# Verificar estado de los contenedores
docker-compose ps

# Ver logs del collector
docker-compose logs -f otel-collector
```

## 🌐 Acceso a los Servicios

| Servicio | URL | Usuario/Contraseña |
|----------|-----|-------------------|
| Grafana | http://localhost:3200 | admin/admin123 |
| Jaeger UI | http://localhost:16686 | - |
| Prometheus | http://localhost:9090 | - |
| Collector Health | http://localhost:13133 | - |

## 🔗 Configuración de Aplicaciones

### Node.js
```bash
npm install @opentelemetry/api @opentelemetry/sdk-node @opentelemetry/auto-instrumentations-node
```

```javascript
const { NodeSDK } = require('@opentelemetry/sdk-node');
const { getNodeAutoInstrumentations } = require('@opentelemetry/auto-instrumentations-node');

const sdk = new NodeSDK({
  instrumentations: [getNodeAutoInstrumentations()],
  serviceName: 'mi-app',
  endpoint: 'http://localhost:4318',
});

sdk.start();
```

### Python
```bash
pip install opentelemetry-api opentelemetry-sdk opentelemetry-auto-instrumentation
```

```bash
export OTEL_SERVICE_NAME=mi-app
export OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4318
opentelemetry-instrument python tu_app.py
```

### Java
```bash
java -javaagent:opentelemetry-javaagent.jar \
     -Dotel.service.name=mi-app \
     -Dotel.exporter.otlp.endpoint=http://localhost:4318 \
     -jar tu-app.jar
```

## 📊 Configuración de Grafana

1. **Importar Dashboards**:
   - Ve a Dashboards → Import
   - Importa dashboards de OpenTelemetry desde Grafana.com

2. **Datasources configuradas automáticamente**:
   - Prometheus (métricas)
   - Jaeger (traces)

## 🔍 Verificación

### Enviar datos de prueba

```bash
# Ejecutar una app de ejemplo
docker run --rm --network opentelemetry-stack_otel-network \
  -e OTEL_SERVICE_NAME=test-app \
  -e OTEL_EXPORTER_OTLP_ENDPOINT=http://otel-collector:4318 \
  alpine/sidecar-injector:latest
```

### Verificar en Jaeger

1. Ve a http://localhost:16686
2. Busca el servicio "test-app"
3. Deberías ver los traces

### Verificar en Grafana

1. Ve a http://localhost:3200
2. Explora los dashboards de métricas
3. Crea tus propios dashboards

## 🛠️ Configuración Avanzada

### Personalizar Collector

Edita `otel-collector-config.yaml` para:
- Agregar más procesadores
- Configurar exportadores adicionales
- Ajustar límites de memoria

### Configurar Alertas

1. En Grafana: Alerting → New alert rule
2. Configura reglas basadas en métricas de Prometheus

## 🔧 Troubleshooting

### Problemas Comunes

1. **No recibes datos**:
   - Revisa logs del collector: `docker-compose logs otel-collector`
   - Verifica conectividad de red

2. **Grafana no se conecta a Prometheus**:
   - Verifica que Prometheus esté accesible
   - Revisa configuración de datasource

3. **Alto uso de memoria**:
   - Ajusta `memory_limiter` en collector config
   - Reduce intervalos de scrape

### Logs Útiles

```bash
# Logs de cada servicio
docker-compose logs -f otel-collector
docker-compose logs -f jaeger
docker-compose logs -f prometheus
docker-compose logs -f grafana
```

## 📈 Métricas Importantes

- **otelcol_processor_***: Métricas del collector
- **prometheus_target_interval_length_seconds**: Tiempo de scrape
- **jaeger_***: Métricas de Jaeger

## 🔄 Actualización

```bash
# Actualizar imágenes
docker-compose pull
docker-compose up -d
```

## 🤝 Contribuir

1. Fork el repositorio
2. Crea una rama feature
3. Commit tus cambios
4. Push a la rama
5. Crea un Pull Request

## 📄 Licencia

MIT License - ver archivo LICENSE

## 🆘 Soporte

Crea un issue en el repositorio para problemas o preguntas.
