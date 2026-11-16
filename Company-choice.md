# Mi Elección de Empresa: TrackFlow

## Mi idea de Agente de IA

### Agente de Atención al Cliente de Primera Línea

**¿Qué haría el agente?**

Actuaría como primera línea de atención para los 15 agentes de Valentina Cruz, automatizando el 80% de las consultas repetitivas que hoy se atienden manualmente en correo, WhatsApp y teléfono sin un sistema de tickets unificado. El agente convertiría un proceso manual y fragmentado en un flujo estructurado que resuelve consultas de forma autónoma y escala solo los casos que requieren decisión humana.

**Información que necesitaría:**

1. **Base de conocimiento semántica indexada para RAG:**
   - Políticas de devoluciones (que varían según cliente, según el BRIEFING menciona que las devoluciones representan 18-25% del volumen)
   - Términos y condiciones específicos por marca cliente (TrackFlow atiende marcas B2B distintas, cada una con contratos anuales)
   - Procedimientos de reclamación, cobertura geográfica (dos países: Estados Unidos y España), horarios de atención
   - Información de los 8 transportistas (UPS, FedEx, DHL en EE.UU.; MRW, SEUR, DHL en España, más dos locales) — sus ventanas de entrega típicas, tasas históricas de puntualidad

2. **Datos en tiempo real del ecosistema de TrackFlow:**
   - Estado de cualquier envío consultando el endpoint unificado de seguimiento (que debe agregarse de los 8 transportistas)
   - Estado de devoluciones: si fue aprobada, en qué fase del flujo está (recogida, inspección, reacondicionamiento)
   - Información de clientes B2B desde el CRM (una integración que hoy no existe pero está identificada en el BRIEFING)
   - Datos de incidencias conocidas (ej: "DHL tiene retrasos en rutas de Los Ángeles esta semana")

3. **Contexto histórico del cliente específico:**
   - Consultas previas de esa marca o consumidor final
   - Patrones de devoluciones de ese cliente (tasa de rechazo, productos problemáticos)
   - Historial de incidencias y cómo se resolvieron

**Qué produciría o desencadenaría:**

- **Respuestas autónomas en español e inglés** (soporte multilingüe que el BRIEFING marca como "muy recomendable"):
  - Consultas de seguimiento: "¿Dónde está mi paquete? → información en tiempo real del transportista
  - Estado de devoluciones: "¿Cuándo me reembolsan?" → fase actual en el flujo de logística inversa
  - Consultas de política: "¿Puedo devolver este producto?" → basado en términos del cliente y política de 18-25 días típica

- **Escalado inteligente a agentes humanos:**
  - Solo cuando el agente detecte frustración (análisis de sentimiento)
  - Cuando la pregunta requiera decisión discrecional (excepciones, casos especiales)
  - Cuando la confianza de la respuesta sea baja
  - Genera ticket estructurado para el agente con contexto completo (historial del cliente, consultas previas, datos de envío)

- **Generación de inteligencia operacional:**
  - Reporte agregado de consultas frecuentes por tema/transportista/ruta (ej: "78 consultas sobre retrasos con MRW en Zaragoza esta semana → alerta a Carlos Vega)
  - Identificación de puntos de fricción: productos con tasa alta de devoluciones, transportistas con mayor número de quejas, rutas problemáticas
  - Dashboard de Experiencia del Cliente en tiempo real (un componente que hoy no existe)
  - Alertas proactivas de clientes frustrados antes de que decidan no renovar (impacto directo en el equipo de Miguel Torres de Comercial)

**Por qué es el reto correcto para TrackFlow:**

Hoy TrackFlow tiene un problema claro en el BRIEFING: 15 agentes sin sistema de tickets unificado, 80% de consultas repetitivas, cobertura nula fuera de horario, dos países y dos idiomas sin automatización. Este agente resuelve tres problemas críticos simultáneamente:

1. **Libera capacidad:** 15 agentes pueden enfocarse en casos que realmente requieren negociación (clientes en riesgo de cancelar, excepciones complejas) en vez de responder "¿Dónde está mi paquete?" 500 veces al mes

2. **Cierra un hueco de datos:** El BRIEFING identifica que "el 80% de las consultas podría responderse automáticamente" pero no hay base de conocimiento ni sistema unificado. Este agente es el primer componente que convierte datos fragmentados en información accesible

3. **Genera visibilidad ejecutiva:** Thomas (CEO) toma decisiones hoy con datos que tienen 1-2 días de antigüedad. Las alertas de fricción operacional del agente (ej: "transportista X falla sistemáticamente en región Y") alimentan decisiones inmediatas sobre selección de transportistas, que es responsabilidad de Carlos Vega

4. **Escala con TrackFlow:** No requiere decisiones arquitectónicas que bloqueen futuras integraciones. El agente usa APIs estándar (transportistas, CRM, devoluciones) que están en el roadmap del equipo de Andrés Kim

5. **Riesgo controlado:** Las consultas de seguimiento son estructuradas y verificables. El agente no aprueba devoluciones (esa lógica corresponde al motor de aprobación automática que Sofía necesita), solo responde el estado
