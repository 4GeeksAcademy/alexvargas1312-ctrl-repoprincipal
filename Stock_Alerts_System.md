# Sistema Automatizado de Alertas de Stock Bajo
## TrackFlow Tech — Componente de API de Inventario Unificada

---

## 📋 Visión del Componente

**Objetivo:** Crear un sistema inteligente que dispare alertas automáticas cuando el inventario de un SKU caiga por debajo de umbrales predefinidos, notificando simultáneamente a **clientes** (B2B) y al **equipo de compras** (interno) en tiempo real.

**Impacto:**
- ✅ Prevención proactiva de stockouts
- ✅ Clientes notificados automáticamente → mejor experience y evita sorpresas
- ✅ Equipo de compras actúa rápidamente → menos backorders, menos pérdida de ventas
- ✅ Cero trabajo manual de monitoreo

**Usuario Final Principal:** Ana Whitfield (Operaciones) + Miguel Torres (Comercial) + Rafael Sanz (Compras)

---

## 🎯 Requisitos Funcionales

### R1: Definición de Umbrales de Stock
- Cada SKU tiene un **umbral mínimo de alerta** configurable
- Umbrales pueden variar por **almacén** (Los Ángeles vs. Zaragoza)
- Umbrales pueden variar por **cliente** (algunos clientes ordenan 500 unidades/mes, otros 50)
- Umbrales pueden variar por **temporada** (temporada alta vs. baja)

**Ejemplo:**
```
SKU #12345 (Producto X)
├── Los Ángeles: umbral = 100 unidades
├── Zaragoza: umbral = 50 unidades
└── Cliente Premium ABC: umbral = 200 unidades (necesita más cobertura)
```

### R2: Disparador de Alertas
- Alert se dispara cuando: `Stock Actual ≤ Umbral Mínimo`
- Se ejecuta **en tiempo real** (máximo 5 minutos de latencia)
- **Evita alertas duplicadas:** si ya hay una alerta activa para ese SKU, no envía otra hasta 24h después (configurable)
- Registra cada alerta: quién, cuándo, qué SKU, por qué motivo

### R3: Routing Inteligente de Notificaciones
Las alertas se envían a **diferentes destinatarios** según el contexto:

| Tipo de Alerta | Quién la Recibe | Canales | Urgencia |
|----------------|-----------------|---------|----------|
| **Stock Bajo General** | Equipo de Compras + Gerente de Almacén (Ana) | Email + SMS | Normal |
| **Stock Crítico** (≤10% del umbral) | Equipo de Compras + CEO (Thomas) | Email + SMS + Slack | URGENTE |
| **Stock Bajo de Cliente Premium** | Account Manager del cliente (Miguel) + Cliente | Email + API notification | Alta |
| **Stock Bajo de Producto de Alto Costo** | CFO + Compras | Email | Media |

### R4: Canales de Notificación
- **Email:** Notificación formal con contexto
- **SMS:** Para alertas críticas a personas clave
- **Slack:** Notificación interna instantánea al equipo
- **API Webhook:** Notificación a sistemas de clientes (opcional)
- **Dashboard:** Visible en tiempo real en la pantalla de operaciones

### R5: Contenido de la Alerta
Cada alerta debe incluir:
```
🔴 ALERTA: STOCK BAJO DETECTADO

Producto: Producto X (SKU #12345)
Almacén: Los Ángeles
Stock Actual: 85 unidades
Umbral Mínimo: 100 unidades
Déficit: 15 unidades
Último Movimiento: Salida de 50 unidades hace 2 horas
Tasa de Consumo: 25 unidades/día (estimado)
Días hasta Stockout: ~3.4 días

ACCIÓN RECOMENDADA:
- Orden de compra urgente: 500 unidades
- Tiempo de entrega típico: 7 días
- Riesgo de Backorder: ALTO

[Ver más detalles] [Crear OC] [Descartar Alerta]
```

### R6: Acciones Desde la Alerta
- **Crear Orden de Compra (OC):** Click directo para generar OC con cantidad recomendada
- **Ignorar/Descartar:** Si es falsa alarma, marca como resuelta
- **Cambiar Umbral:** Ajusta el umbral si es necesario
- **Ver Histórico:** Gráfico de consumo histórico del SKU
- **Notificar Cliente:** Checkbox para avisar al cliente de posible delay

---

## 🏗️ Arquitectura Técnica

### Flujo End-to-End

```
┌──────────────────────────────────────────────────────────────┐
│              FUENTES DE INVENTARIO (EN VIVO)                 │
│  Los Ángeles WMS + Zaragoza Google Sheets + ERP Legacy       │
└────────────────────┬─────────────────────────────────────────┘
                     │
                     ↓ (Cada 5 minutos)
         ┌─────────────────────────────┐
         │   INGESTA DE DATOS          │
         │  (ETL Pipeline actualizando │
         │   Data Warehouse)           │
         └────────────┬────────────────┘
                      │
                      ↓
         ┌─────────────────────────────┐
         │   DATA WAREHOUSE            │
         │  (PostgreSQL / BigQuery)    │
         │  Tabla: inventory_levels    │
         └────────────┬────────────────┘
                      │
                      ↓ (Trigger cada 5 min)
    ┌────────────────────────────────────────────┐
    │   MOTOR DE ALERTAS (Alert Engine)          │
    │  (Python + APScheduler)                    │
    │                                             │
    │  1. Lee Data Warehouse                     │
    │  2. Compara Stock vs. Umbrales             │
    │  3. Calcula Tasa de Consumo               │
    │  4. Detecta Stockouts Potenciales          │
    │  5. Genera Alertas                         │
    └────────────┬─────────────────────────────┘
                 │
        ┌────────┴────────┐
        │                 │
        ↓                 ↓
  ┌──────────────┐  ┌──────────────────────┐
  │ALERT LOG DB  │  │ NOTIFICATION QUEUE   │
  │(Histórico)   │  │(Redis/RabbitMQ)      │
  └──────────────┘  └──────────┬───────────┘
                                │
                   ┌────────────┼────────────┐
                   │            │            │
                   ↓            ↓            ↓
         ┌──────────────┐ ┌──────────┐ ┌─────────┐
         │ EMAIL SES    │ │SMS SNS   │ │ SLACK   │
         │(AWS SES)     │ │ (AWS)    │ │ (API)   │
         └──────────────┘ └──────────┘ └─────────┘
                   │            │            │
                   └────────────┼────────────┘
                                │
                                ↓
                    ┌──────────────────────┐
                    │  USUARIOS FINALES    │
                    │  (Compras, Ana,      │
                    │   Gerentes, Clientes)│
                    └──────────────────────┘
```

### Componentes Clave

#### 1. **Alert Configuration Service**
Administra configuración de umbrales y reglas.

```python
# Ejemplo de configuración
alert_config = {
    "sku_id": "12345",
    "product_name": "Producto X",
    "thresholds": {
        "los_angeles": {
            "minimum": 100,
            "critical": 20,
            "reorder_point": 150
        },
        "zaragoza": {
            "minimum": 50,
            "critical": 10,
            "reorder_point": 75
        }
    },
    "client_overrides": {
        "client_abc_premium": {
            "los_angeles_minimum": 200,  # Override para cliente
            "alert_channels": ["email", "sms", "api_webhook"]
        }
    },
    "consumption_rate": "25 units/day",  # Calculado automáticamente
    "lead_time_days": 7,
    "notify_to": ["compras_team", "ana@trackflow.com"],
    "client_notify": True
}
```

#### 2. **Alert Engine (Motor de Alertas)**
Corre cada 5 minutos, evalúa todas las SKUs.

```python
# Pseudocódigo
def check_all_skus():
    for sku in all_skus:
        current_stock = get_stock(sku)
        threshold = get_threshold(sku)
        
        if current_stock <= threshold:
            # Generar alerta
            alert = Alert(
                sku_id=sku,
                current_stock=current_stock,
                threshold=threshold,
                deficit=threshold - current_stock,
                estimated_days_to_stockout=calculate_days(current_stock, consumption_rate),
                severity=get_severity(current_stock, threshold)
            )
            
            # Evitar duplicados
            if not has_recent_alert_for_sku(sku, hours=24):
                enqueue_notification(alert)
                log_alert(alert)
```

#### 3. **Notification Service**
Enruta notificaciones a canales correctos.

```python
def send_notification(alert):
    # Determinar severidad
    if alert.severity == "CRITICAL":
        channels = ["email", "sms", "slack"]
        recipients = ["compras", "ana", "thomas"]  # CEO
    elif alert.severity == "HIGH":
        channels = ["email", "slack"]
        recipients = ["compras", "ana"]
    else:
        channels = ["email"]
        recipients = ["compras"]
    
    # Enviar por cada canal
    for channel in channels:
        for recipient in recipients:
            send_to_channel(alert, channel, recipient)
    
    # Notificar cliente si aplica
    if alert.sku.is_customer_critical:
        notify_account_manager(alert)
```

#### 4. **Alert History & Analytics**
Log de todas las alertas para análisis.

```sql
-- Tabla de alertas
CREATE TABLE alerts (
    id SERIAL PRIMARY KEY,
    sku_id INT,
    alert_timestamp TIMESTAMP,
    stock_level INT,
    threshold INT,
    severity VARCHAR(20),  -- CRITICAL, HIGH, NORMAL
    status VARCHAR(20),     -- ACTIVE, RESOLVED, FALSE_ALARM
    resolved_by VARCHAR(100),
    resolved_timestamp TIMESTAMP,
    action_taken VARCHAR(500),
    created_at TIMESTAMP
);
```

---

## 🚨 Umbrales Inteligentes (Recomendados)

### Nivel 1: Mínimo (Green → Yellow)
```
Stock = Umbral Mínimo
Acción: Notificación a compras (no urgente)
Mensaje: "Stock bajo. Considerar reorden en los próximos 2-3 días"
Canales: Email
```

### Nivel 2: Crítico (Yellow → Red)
```
Stock ≤ 20% del Umbral Mínimo
Acción: Notificación urgente + SMS
Mensaje: "CRÍTICO. Stock podría agotarse en 2-3 días. Crear OC AHORA"
Canales: Email + SMS + Slack
```

### Nivel 3: Stockout (Red → Black)
```
Stock = 0
Acción: Escalación máxima + Notificar CEO + Clientes
Mensaje: "STOCKOUT. Producto agotado. Riesgo de backorders."
Canales: Email + SMS + Slack + API
```

---

## 👥 Flujos de Usuario

### Flujo 1: Compras Recibe Alerta de Stock Bajo

```
1. Email llega a compras@trackflow.com
2. Rafael (Compras) abre email
3. Ve: SKU #12345, Stock: 85, Umbral: 100
4. Click en [Crear OC Automática]
5. Sistema pre-rellena:
   - Producto: Producto X
   - Cantidad: 500 unidades (cantidad de reorden recomendada)
   - Proveedor: Supplier ABC (el más rápido)
   - Costo estimado: $2,500
6. Rafael revisa y aprueba
7. OC generada automáticamente en ERP
8. Alert marcada como RESOLVED
```

### Flujo 2: Cliente Premium Notificado de Posible Delay

```
1. Motor de alertas detecta que stock de SKU #12345 va a stockout
2. Verifica: "¿Es este producto crítico para algún cliente?"
3. Sí: Cliente ABC Premium ha ordenado 100 unidades/mes
4. Stock actual: 85, Consumo: 25/día = 3.4 días
5. Notifica a Miguel Torres (Account Manager): 
   "Stock de Producto X para Cliente ABC caerá en 3 días"
6. Miguel envía email al cliente:
   "Hola, queremos notificarte que el Producto X podría tener 
    disponibilidad limitada la próxima semana. 
    ¿Quieres colocar orden adicional ahora?"
7. Cliente ordena 50 unidades adicionales → Stock sube → Alert resuelta
```

### Flujo 3: CEO Recibe Alerta de Stockout Crítico

```
1. Stock crítico detectado (múltiples SKUs bajos simultáneamente)
2. Sistema envía SMS a Thomas:
   "ALERTA: 3 productos en riesgo de stockout. Ver dashboard."
3. Thomas abre dashboard ejecutivo
4. Ve tabla de alertas activas con impacto financiero
5. Toma decisión: ¿Comprar más inventario? ¿Notificar clientes?
```

---

## 📊 Métricas de Éxito del Sistema

| Métrica | Baseline | Objetivo | Cómo Medir |
|---------|----------|----------|-----------|
| **Detección de Stock Bajo** | Manual (días) | Automático (5 min) | Timestamp de alerta |
| **Tiempo de Respuesta (Compras)** | 24-48 horas | < 2 horas | Timestamp OC creada |
| **Stockouts Prevenidos** | N/A | 80%+ | Alertas que evitaron stockout |
| **Tasa de Falsa Alarma** | 20% | < 5% | Alertas marcadas como "false alarm" |
| **Satisfacción de Cliente** | Manual | 95% | Clientes notificados a tiempo |
| **Costo de Inventory** | Actual | -15% | Menos overstock + menos backorders |

---

## 🔌 Integraciones Necesarias

### 1. **Data Warehouse → Alert Engine**
- Query: Obtener stock actual de todos los SKUs cada 5 minutos
- Query: Obtener umbrales configurados para cada SKU
- Query: Obtener histórico de consumo (últimos 30 días)

### 2. **Alert Engine → Notification Service**
- Enviar payloads de alerta a queue (Redis/RabbitMQ)
- Marcar alertas como enviadas

### 3. **Notification Service → Email (SES/SendGrid)**
```
POST /send-email
{
  "to": "compras@trackflow.com",
  "template": "stock_alert_critical",
  "data": {
    "sku_id": "12345",
    "current_stock": 85,
    "threshold": 100
  }
}
```

### 4. **Notification Service → SMS (Twilio/AWS SNS)**
```
POST /send-sms
{
  "phone": "+34666123456",
  "message": "ALERTA: Stock bajo SKU #12345 (85 unidades)"
}
```

### 5. **Notification Service → Slack**
```
POST https://hooks.slack.com/services/YOUR/WEBHOOK
{
  "text": "🔴 ALERTA: Stock bajo SKU #12345",
  "attachments": [...]
}
```

### 6. **Alert Engine → Dashboard**
- Actualización en tiempo real de alertas activas
- Filtros por severidad, almacén, SKU

---

## 🛠️ Stack Tecnológico

| Componente | Opción Recomendada | Razón |
|-----------|-------------------|-------|
| **Alert Engine** | Python + APScheduler | Simple, confiable, fácil mantener |
| **Queue** | Redis | Rápido, en-memoria, ideal para notificaciones |
| **Email** | AWS SES o SendGrid | Escalable, entregable, cheap |
| **SMS** | AWS SNS o Twilio | Confiable, global |
| **Chat** | Slack API | Integración nativa con team |
| **Storage** | PostgreSQL | Alertas históricas, queryable |
| **Monitoring** | CloudWatch / DataDog | Alertas sobre alertas (meta) |

---

## 📝 Configuración de Ejemplo (JSON)

```json
{
  "alert_rules": [
    {
      "sku_id": "12345",
      "product_name": "Producto X",
      "alert_type": "stock_low",
      "thresholds": {
        "default": {
          "minimum": 100,
          "critical": 20
        },
        "by_warehouse": {
          "los_angeles": {
            "minimum": 100,
            "critical": 20
          },
          "zaragoza": {
            "minimum": 50,
            "critical": 10
          }
        },
        "by_customer": {
          "client_abc_premium": {
            "minimum": 200,
            "critical": 50
          }
        }
      },
      "notifications": {
        "level_1": {
          "severity": "normal",
          "recipients": ["compras_team@trackflow.com"],
          "channels": ["email"],
          "message_template": "stock_low_notification"
        },
        "level_2": {
          "severity": "critical",
          "recipients": ["compras_team@trackflow.com", "ana@trackflow.com", "thomas@trackflow.com"],
          "channels": ["email", "sms", "slack"],
          "message_template": "stock_critical_alert"
        }
      },
      "auto_actions": {
        "suggest_reorder": true,
        "reorder_quantity": 500,
        "preferred_supplier": "supplier_abc"
      },
      "customer_notification": {
        "enabled": true,
        "notify_via_account_manager": true,
        "template": "stock_low_customer_message"
      }
    }
  ]
}
```

---

## 🎯 Roadmap de Implementación

### Phase 1: MVP (Semanas 1-2)
- [x] Alert Engine básico (Python)
- [x] Detección de stock bajo
- [x] Notificaciones por Email
- [x] Alert Log en BD

### Phase 2: Expansión de Canales (Semanas 3-4)
- [ ] Integración SMS (Twilio/SNS)
- [ ] Integración Slack
- [ ] Dashboard de alertas activas
- [ ] UI para configurar umbrales

### Phase 3: Inteligencia (Semanas 5-6)
- [ ] Cálculo automático de tasa de consumo
- [ ] Predicción de stockout (ML)
- [ ] Reorden automático sugerido
- [ ] Análisis de patrones de devolución

### Phase 4: Integración con Clientes (Semanas 7+)
- [ ] API Webhook para notificar a clientes
- [ ] Portal de cliente para ver alertas
- [ ] Integración con sistemas CRM

---

## ✅ Checklist de Implementación

- [ ] Conectar a Data Warehouse (schemas de inventory_levels)
- [ ] Crear tabla `alerts` en BD
- [ ] Implementar Alert Engine (Python)
- [ ] Configurar umbrales por SKU
- [ ] Integrar SES/SendGrid para Email
- [ ] Integrar Twilio/SNS para SMS
- [ ] Integrar Slack Webhook
- [ ] Crear dashboard de alertas
- [ ] Testing: Verificar emails, SMS, Slack
- [ ] Testing: Falsa alarmas (5% objetivo)
- [ ] UAT con Ana + Compras + Thomas
- [ ] Deploy a producción
- [ ] Capacitación de usuarios
- [ ] Monitoreo 24/7

---

*Sistema de Alertas — TrackFlow Tech · Componente del API de Inventario*
