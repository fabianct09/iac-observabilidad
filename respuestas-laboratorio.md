# Laboratorio de Observabilidad - IAC

## Requisitos previos
- Docker Desktop instalado y corriendo
- Git

## Cómo levantar el stack

```bash
git clone https://github.com/fabianct09/iac-observabilidad.git
cd iac-observabilidad
docker compose up -d --build
```

## Verificar que todo esté corriendo

```bash
docker compose ps
```

## URLs de acceso
| Servicio | URL |
|----------|-----|
| Grafana | http://localhost:3000 (admin/admin) |
| Prometheus | http://localhost:9090 |
| Alloy | http://localhost:12345 |
| Frontend | http://localhost:8080 |
| Backend | http://localhost:3001 |

## Nota importante (Windows)
En Docker Desktop para Windows se debe usar `/:/host:ro` en lugar de `/:/host:ro,rslave` en el volumen de node-exporter.

# Respuestas - Laboratorio de Observabilidad

**Alumno:** fabianct09  
**Fecha:** 2026-06-12  
**Repositorio:** https://github.com/fabianct09/iac-observabilidad

---

## 1. ¿Por qué necesitamos Loki además de Prometheus si ya tenemos `/metrics`?

Prometheus solo almacena métricas numéricas (CPU, memoria, contadores). Loki almacena logs de texto, que explican el *por qué* de un problema. Son complementarios: Prometheus detecta que algo falla, Loki ayuda a encontrar la causa.

---

## 2. ¿Qué ventaja aporta que las fuentes de datos de Grafana estén aprovisionadas como código y no creadas a mano?

Garantiza que cualquiera que levante el stack obtenga la misma configuración automáticamente, sin pasos manuales. Además, los cambios quedan versionados en Git y son reproducibles en cualquier entorno.

---

## 3. El panel "CPU contenedor" y el panel "CPU host" pueden mostrar valores muy distintos. ¿Por qué? ¿Cuál usarías para alertar sobre una aplicación concreta?

El CPU host mide el uso total de la máquina (todos los procesos). El CPU contenedor mide solo el uso de un contenedor específico. Para alertar sobre una aplicación concreta se usa el **CPU por contenedor**, ya que refleja el consumo real de esa app sin ruido del resto del sistema.

---

## 4. ¿Qué diferencia hay entre el evaluation interval y el pending period de una alarma?

- **Evaluation interval**: cada cuánto tiempo Grafana evalúa la condición (ej. cada 1 minuto).
- **Pending period**: cuánto tiempo debe sostenerse la condición antes de disparar la alerta. Con `None` dispara inmediatamente; con `5m` evita falsos positivos por picos momentáneos.
