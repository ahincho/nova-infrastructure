# Nova Platform — Infraestructura de Observabilidad Local

Este repositorio contiene la infraestructura Docker necesaria para ejecutar un stack de observabilidad completo en tu máquina local. Replica la arquitectura de Grafana Cloud:

```
App (host) → OpenTelemetry Collector → Backends (Mimir, Loki, Tempo, Pyroscope) → Grafana
```

## Requisitos previos

- Docker y Docker Compose instalados
- Puertos disponibles: 3000, 3100, 3200, 4040, 4317, 4318, 9009

## Inicio rápido

```bash
cd observability
docker compose up -d
```

Espera unos segundos a que todos los servicios inicien correctamente.

## Servicios y puertos

| Servicio           | Puerto | URL                          | Descripción                     |
|--------------------|--------|------------------------------|---------------------------------|
| Grafana            | 3000   | http://localhost:3000        | Dashboards y visualización      |
| OTel Collector     | 4317   | grpc://localhost:4317        | Receptor OTLP gRPC             |
| OTel Collector     | 4318   | http://localhost:4318        | Receptor OTLP HTTP             |
| Mimir              | 9009   | http://localhost:9009        | Métricas (compatible Prometheus)|
| Tempo              | 3200   | http://localhost:3200        | Traces                          |
| Loki               | 3100   | http://localhost:3100        | Logs                            |
| Pyroscope          | 4040   | http://localhost:4040        | Profiles                        |

## Credenciales de Grafana

- **Usuario:** admin
- **Contraseña:** admin

Los datasources (Mimir, Tempo, Loki, Pyroscope) se provisionan automáticamente. Solo abre http://localhost:3000 y empieza a explorar.

## Configurar la aplicación

La aplicación se ejecuta **fuera de Docker** en tu máquina host y envía telemetría al Collector.

En tu `application.yml`:

```yaml
nova:
  observability:
    otlp:
      endpoint: http://localhost:4318
```

## Detener los servicios

```bash
cd observability
docker compose down
```

Para eliminar también los volúmenes de datos:

```bash
docker compose down -v
```

## Stack de observabilidad

| Pilar | Backend | Descripción |
|---|---|---|
| **Metrics** | Grafana Mimir | Compatible con Prometheus, recibe vía remote write |
| **Traces** | Grafana Tempo | Almacena trazas distribuidas, recibe vía OTLP |
| **Logs** | Grafana Loki | Almacena logs estructurados, recibe vía OTLP |
| **Profiles** | Grafana Pyroscope | Almacena perfiles de rendimiento |
