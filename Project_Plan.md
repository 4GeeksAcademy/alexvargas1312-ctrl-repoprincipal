# TrackFlow Tech: Plan de Proyecto
## API de Inventario Unificada + Dashboard Ejecutivo

---

## 📋 Visión General del Proyecto

**Objetivo:** Construir una infraestructura de datos que unifique la visibilidad del inventario en tiempo real a nivel global y alimente un dashboard ejecutivo con KPIs estratégicos para TrackFlow.

**Impacto:** 
- Ana Whitfield y su equipo de operaciones tendrán visibilidad del inventario global en tiempo real
- Thomas Espinoza recibirá un dashboard ejecutivo con KPIs actualizados automáticamente, eliminando 3-4 horas de trabajo manual cada semana

**Duración Estimada:** Proyecto de ingeniería de IA

---

## 🎯 Objetivos Principales

### Objetivo 1: API de Inventario Unificada
**Usuario Final:** Ana Whitfield (Operaciones de Almacén)

- Crear una API que agregue datos de inventario de Los Ángeles y Zaragoza en tiempo real
- Soportar consultas por SKU, por almacén, por producto
- Devolver: stock actual, stock reservado, stock disponible, última actualización
- Alertas automáticas de stock bajo por cliente y por almacén
- Datos históricos para análisis de tendencias

### Objetivo 2: Dashboard Ejecutivo Global
**Usuario Final:** Thomas Espinoza (CEO)

- Dashboard con KPIs en tiempo real de las dos operaciones (por país y consolidado)
- Comparativas país a país (Los Ángeles vs. Zaragoza)
- Alertas automáticas por umbral
- Informe ejecutivo generado automáticamente
- Acceso a datos con latencia < 5 minutos

---

## 📊 KPIs a Visualizar en el Dashboard

### KPIs de Inventario (Operaciones de Almacén)
| KPI | Definición | Frecuencia |
|-----|-----------|-----------|
| **Stock Total** | Unidades en stock (ambos almacenes) | Tiempo real |
| **Stock por Almacén** | Inventario de Los Ángeles + Zaragoza | Tiempo real |
| **Fill Rate** | % de pedidos completados sin backorder | Diario |
| **Rotación de Inventario** | Veces que el stock se renueva | Semanal |
| **Discrepancias de Inventario** | Diferencias entre sistema y físico | Diario |
| **Stock en Riesgo** | SKUs por debajo del mínimo | Tiempo real |

### KPIs de Última Milla (Transporte)
| KPI | Definición | Frecuencia |
|-----|-----------|-----------|
| **Volumen Diario** | Paquetes procesados (ambos almacenes) | Tiempo real |
| **Costo Medio por Envío** | Costo total / número de envíos | Diario |
| **On-Time Delivery Rate** | % de entregas a tiempo | Diario |
| **Transportista Top** | Mejor desempeño por destino | Semanal |

### KPIs de Devoluciones (Logística Inversa)
| KPI | Definición | Frecuencia |
|-----|-----------|-----------|
| **Tasa de Devolución** | % de devoluciones sobre envíos | Diario |
| **Tiempo de Retorno** | Días desde recepción hasta aprobación | Diario |
| **Aprob. vs. Rechazo** | % de devoluciones aprobadas | Semanal |

### KPIs de Negocio (Dirección Ejecutiva)
| KPI | Definición | Frecuencia |
|-----|-----------|-----------|
| **Revenue (Estimado)** | Ingresos por envíos procesados | Diario |
| **Rentabilidad** | Ingresos - costo de envíos | Diario |
| **Volumen Acumulado YTD** | Paquetes procesados año a la fecha | Diario |

---

## 🏗️ Arquitectura Técnica (Alto Nivel)

```
┌─────────────────────────────────────────────────────────┐
│         FUENTES DE DATOS (SISTEMAS EXISTENTES)          │
├──────────────────┬──────────────────┬──────────────────┤
│  Los Ángeles     │   Zaragoza       │    ERP Global    │
│  (Software WMS)  │   (Google Sheets)│ (2010 legacy)    │
└──────────────────┴──────────────────┴──────────────────┘
                            ↓
         ┌──────────────────────────────────┐
         │    ETL / DATA PIPELINE           │
         │  (Ingesta, transformación,       │
         │   limpieza, unificación)         │
         └──────────────────────────────────┘
                            ↓
         ┌──────────────────────────────────┐
         │     DATA WAREHOUSE / LAKE        │
         │  (Base de datos centralizada     │
         │   con esquema unificado)         │
         └──────────────────────────────────┘
                    ↓                 ↓
         ┌──────────────┐    ┌──────────────────┐
         │  API REST    │    │  BI / Analytics  │
         │  Inventario  │    │  (Data Studio)   │
         └──────────────┘    └──────────────────┘
                    ↓                 ↓
         ┌──────────────┐    ┌──────────────────┐
         │   Backend    │    │   Dashboard      │
         │  (Ana)       │    │   Ejecutivo      │
         └──────────────┘    │   (Thomas)       │
                             └──────────────────┘
```

### Componentes Clave

1. **ETL Pipeline**
   - Conectores a los dos sistemas de almacén
   - Transformación de datos a esquema unificado
   - Validación y limpieza de datos
   - Actualización en tiempo real (o con latencia < 5 min)

2. **Data Warehouse**
   - Esquema unificado de inventario (SKU, almacén, cantidad, etc.)
   - Tablas de hechos y dimensiones
   - Histórico de movimientos

3. **API de Inventario**
   - Endpoints para consultar stock actual
   - Filtros por SKU, almacén, cliente
   - Respuestas en JSON
   - Autenticación y rate limiting

4. **Dashboard Ejecutivo**
   - Visualizaciones de KPIs (números, gráficos, mapas)
   - Comparativas país a país
   - Alertas automáticas
   - Exportación a PDF/Excel

---

## 👥 Usuarios y Casos de Uso

### Usuario 1: Ana Whitfield (Operaciones de Almacén)
**Caso de Uso:** "Necesito saber el stock global de un SKU específico ahora mismo"
- Accede a un portal o API
- Consulta: "¿Cuánto tengo de SKU #12345 en Los Ángeles y Zaragoza?"
- Respuesta: Stock total, por almacén, stock reservado, disponible

### Usuario 2: Thomas Espinoza (CEO)
**Caso de Uso:** "Quiero ver cómo está el negocio esta semana vs. la anterior"
- Abre el dashboard ejecutivo
- Ve KPIs de inventario, transporte, devoluciones, financiero
- Compara Los Ángeles vs. Zaragoza
- Recibe alertas si algo cae por debajo de umbral

### Usuario 3: Miguel Torres (Comercial)
**Caso de Uso:** "Necesito decirle a un cliente cómo va su operación"
- Genera un informe automático con KPIs de ese cliente
- El dashboard muestra fill rate, on-time delivery, devoluciones

---

## 📈 Métrica de Éxito

| Métrica | Línea Base | Objetivo | Evidencia |
|---------|-----------|----------|-----------|
| **Tiempo de Decisión sobre Stock** | Manual, 2-4 horas | Automático, < 5 min | Prueba de API |
| **Frecuencia de Informes Ejecutivos** | Semanal manual (domingo) | Diaria automática | Dashboard en vivo |
| **Exactitud de Inventario** | 85% | 98% | Auditoría de datos |
| **Tiempo para Detectar Stock Bajo** | Semanal (tardío) | Tiempo real | Alertas automáticas |
| **Tiempo de Generación de Informes** | 3-4 horas/director | < 5 minutos | Automatización |

---

## 📅 Fases del Proyecto

### Fase 1: Descubrimiento y Diseño
- Mapear estructura de datos en ambos sistemas
- Diseñar esquema unificado
- Documentar requerimientos de API
- Definir KPIs finales

### Fase 2: Construcción de la Capa de Datos
- Crear conectores ETL a ambos sistemas
- Implementar transformación y validación
- Construir data warehouse
- Población inicial de datos

### Fase 3: API de Inventario
- Implementar endpoints REST
- Autenticación y autorización
- Rate limiting y caching
- Documentación (OpenAPI/Swagger)

### Fase 4: Dashboard Ejecutivo
- Diseñar UI/UX del dashboard
- Implementar visualizaciones
- Alertas automáticas
- Integración con API

### Fase 5: Testing y Optimización
- Testing de integridad de datos
- Performance testing
- UAT con Ana y Thomas
- Optimizaciones

### Fase 6: Go-Live y Operación
- Deployment a producción
- Capacitación de usuarios
- Monitoreo
- Soporte

---

## 🛠️ Stack Tecnológico (Propuesto)

| Componente | Opciones | Recomendación |
|-----------|----------|---------------|
| **ETL** | Python (Airflow, dbt) | Python + Airflow |
| **Data Warehouse** | PostgreSQL, BigQuery, Snowflake | PostgreSQL (on-premise) o BigQuery (cloud) |
| **API** | FastAPI, Django REST, Node.js | FastAPI (Python) |
| **Dashboard** | Metabase, Looker, Streamlit, Tableau | Streamlit (Python) o Google Data Studio (simple) |
| **Hosting** | AWS, GCP, Azure, On-Premise | AWS o GCP (Cloud) |
| **Monitoring** | DataDog, New Relic, CloudWatch | CloudWatch (si AWS) o similar |

---

## 🎓 Aprendizajes Esperados

Como profesional con experiencia en operaciones y maestría en Supply Chain, este proyecto te permitirá:

1. **Arquitectura de Datos en Logística**
   - Diseñar un data warehouse para una operación multinacional
   - Implementar ETL en sistemas heredados
   - Optimizar consultas para datos operacionales en tiempo real

2. **Ingeniería de IA Aplicada**
   - Usar Python para pipelines de datos
   - Integrar APIs en aplicaciones reales
   - Implementar sistemas de alertas inteligentes

3. **Transformación Digital en Operaciones**
   - Eliminar procesos manuales
   - Mejorar la toma de decisiones estratégicas con datos
   - Gestionar la adopción de nuevas plataformas (como hiciste en tu empresa)

4. **Operaciones Internacionales**
   - Manejar complejidad de dos países, dos idiomas, dos sistemas
   - Normalizar datos de fuentes dispares
   - Escalar procesos en geografías múltiples

---

## 📝 Próximos Pasos

1. **Semana 1:** Alineación con el equipo técnico (Andrés Kim) sobre arquitectura existente
2. **Semana 2:** Mapeo detallado de sistemas de Los Ángeles y Zaragoza
3. **Semana 3-4:** Diseño de esquema unificado y componentes de solución
4. **Semana 5+:** Implementación iterativa

---

*Plan de Proyecto — TrackFlow Tech · 4Geeks Academy AI Engineering Track*
