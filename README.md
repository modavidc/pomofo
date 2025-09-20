# ⏱️ Pomodoro Personal (PomoBerserk - nombre tentativo)

Cliente Pomodoro **personalizable**, diseñado para sincronizar tareas con Google Calendar y generar reportes profesionales en Word para facturación.  
Incluye un sistema único de **modos de enfoque** inspirados en referentes culturales, filosóficos y personales.

---

## 📛 Nombre del producto (ideas)

- **PomoBerserk** (modo foco brutal, guiño a *Berserk*)  
- **Vía Rápida** (flash/hyper-speed vibes)  
- **TempoCuida** (guiño a T-Cuida + tempo)  
- **FocusZedis** (emparentado con stack robótico)  
- **CronFocus / FocusQueue**  
- **PomoDrive** (empuje, momentum)  

---

## 🎯 Objetivo y alcance (MVP)

Construir un cliente Pomodoro personalizable que:  
1. Sincronice con Google Calendar (OAuth2) y priorice eventos “importantes” para sugerirte foco.  
2. Permita ciclos custom (trabajo/descanso/long break) con modos de energía.  
3. Gestione una cola de tareas (eventos Calendar + tareas manuales) con reglas de prioridad.  
4. Genere un reporte Word (.docx) con horas por proyecto/actividad en formato estándar para facturación.  

---

## ⚡ Modos de enfoque

1. **Berserk 🔥** – fuerza bruta, urgencia.  
2. **Netero-Kannon 🙏🥊** – disciplina, constancia, ritual.  
3. **Speed Force ⚡** – rapidez, agilidad.  
4. **Bebop 🌀** – reflexión, calma, improvisación.  
5. **Séptimo Sentido 🌌** – cosmos, trascendencia, visión.  
6. **Timba 🎶** (opcional extra) – flow, creatividad, alegría.
7. **Ike 🏀** – avance, arranque explosivo, motivación Slam Dunk.

---

## ✅ Requerimientos funcionales

### F1. Sesiones Pomodoro
- F1.1 Configurar duraciones (work/break/long break), número de ciclos, auto-start opcional.  
- F1.2 Modos de enfoque seleccionables.  
- F1.3 Pausa/reanudar/cancelar; etiquetas de contexto (proyecto, cliente, actividad).  

### F2. Integración con Google Calendar
- F2.1 OAuth2 (scopes de lectura) y sincronización programada (cada X min/cron).  
- F2.2 Filtros: por calendario, palabra clave, color/etiqueta, “importancia”.  
- F2.3 Conversión de eventos a tareas en cola con prioridad sugerida.  
- F2.4 Resolución de conflictos (evento en curso vs. foco actual).  

### F3. Cola y Prioridad
- F3.1 Vista de **Backlog / Hoy / Ahora** con drag&drop.  
- F3.2 Score de prioridad (ejemplo: urgencia + impacto + compromiso + energía personal).  
- F3.3 Regla “Focus Gate”: confirmar tarea antes de iniciar ciclo.  

### F4. Tareas y Proyectos
- F4.1 Crear tareas manuales (Proyecto, Cliente, Actividad, Estimación, Tags).  
- F4.2 Plantillas de actividades comunes.  
- F4.3 Time-boxing automático.  

### F5. Tracking y Reporte
- F5.1 Registro automático de cada ciclo: tarea, proyecto, notas.  
- F5.2 Reporte Word (.docx) por rango de fechas → horas por Proyecto/Cliente/Actividad.  
- F5.3 Export extra: CSV/JSON; totales por día y por proyecto.  
- F5.4 Campos para facturación: número de factura, moneda, tarifa, total.  

### F6. UX/Notificaciones
- F6.1 Notificaciones al iniciar/terminar ciclo; alerta “últimos 2 minutos”.  
- F6.2 “Do Not Disturb” (modo Berserk).  
- F6.3 Hotkeys (iniciar/pausar/next).  

### F7. Configuración avanzada
- F7.1 Reglas de importancia (keywords, asistentes, duración mínima, busy/free).  
- F7.2 Mapa de colores Calendar → proyectos predeterminados.  
- F7.3 Auto-etiquetado (regex/keywords: “Surescripts”, “eRx”, “T-Cuida”, “Zedis”).  
- F7.4 Idiomas (ES/EN), zonas horarias, formato 12/24h.  

### F8. Seguridad y cuentas
- F8.1 Login local (PIN) opcional; cifrado en reposo de tokens OAuth.  
- F8.2 Revocar/renovar credenciales; borrar datos localmente.  

---

## ⚙️ Requerimientos no funcionales

- **N1. Rendimiento**: UI reactiva; sync <1s por lote típico.  
- **N2. Confiabilidad**: persistencia local (SQLite), reintentos en sync.  
- **N3. Portabilidad**: desktop multiplataforma (Tauri/Electron) o Web PWA.  
- **N4. Seguridad**: tokens encriptados, mínimos scopes, HTTPS.  
- **N5. Observabilidad**: logs estructurados, métricas (sesiones/día, focus ratio).  
- **N6. UX**: accesibilidad, dark mode, baja fricción.  
- **N7. Extensibilidad**: arquitectura modular (Jira, Notion, Todoist).  
- **N8. Privacidad**: datos locales por defecto.  
- **N9. Internacionalización**: ES/EN, formatos PEN/USD.  

---

## 🗂️ Arquitectura sugerida

- **Frontend**: React + Tauri (desktop ligero) o PWA.  
- **Backend local**: Laravel + Queues (Redis) para sync y reportes.  
- **BD**: SQLite local (o MySQL si aplica).  
- **Integración Calendar**: Google API (OAuth2).  
- **Reportes Word**: PhpWord (Laravel) con plantilla parametrizable.  

---

## 📊 Modelo de datos (borrador)

- `users`: id, name, locale, tz.  
- `oauth_tokens`: provider, access_token_enc, refresh_token_enc, expires_at.  
- `projects`: id, name, client, rate, color.  
- `tasks`: id, source(manual|gcal), title, project_id, priority_score, tags, status.  
- `sessions`: id, task_id, mode, start_at, end_at, duration_min, notes, interrupted.  
- `settings`: key, value (json).  
- `reports`: id, range_start, range_end, file_path, totals.  

---

## 🔌 Endpoints/API internos (si separas front/back)

- `POST /auth/connect/google`  
- `POST /sync/calendar`  
- `GET /tasks/queue`  
- `PATCH /tasks/{id}`  
- `POST /sessions/start|pause|resume|stop`  
- `POST /reports/generate?from=YYYY-MM-DD&to=YYYY-MM-DD`  

---

## 🧮 Reglas de prioridad (ejemplo)

```text
priority = (urgencyWeight * days_until_deadline^-1)
         + (impactWeight * impact_score)
         + (commitmentWeight * is_calendar_committed)
         + (fitWeight * energy_fit)
````

* Urgencia: eventos hoy/mañana suben score.
* Compromiso: si hay invitados o “busy”, sumar más.
* Mapa keywords → impacto (“Prod Incident”, “Facturación”, “Demo Day”).

---

## 🔄 Flujo de sincronización

1. Cron/Job cada 5–10 min: refresca token y hace pull de próximos eventos (48–72h).
2. Filtra según reglas (importancia, duración, asistentes).
3. Normaliza → `tasks (source=gcal)`; upsert si ya existe.
4. Notifica cambios relevantes (nuevo top-1, choques).
5. Al iniciar ciclo → “Focus Gate” con recomendación.

---

## 📝 Plantilla de reporte Word

* Portada: Cliente/Proyecto, rango de fechas, rol, tarifa.
* Resumen ejecutivo: horas totales por proyecto (tabla).
* Detalle diario:

  * Fecha | Proyecto | Actividad | # Ciclos | Horas | Notas.
* Totales: por proyecto y actividad.
* Pie: condiciones, firma, número de factura.

**Campos dinámicos:**

```text
{{period_start}} – {{period_end}}
{{client_name}}
{{project_name}}
{{total_hours_project}}
{{table_rows}}
```

---

## 🚀 Roadmap en fases

* **Fase 0 (PoC)**: UI mínima, ciclos custom, persistencia local, export CSV.
* **Fase 1 (Sync)**: OAuth + sync Calendar con filtros y queue; Focus Gate.
* **Fase 2 (Reportes)**: generación DOCX con PhpWord.
* **Fase 3 (Modos)**: implementar modos (Berserk, Netero-Kannon, Speed Force, Bebop, Séptimo Sentido).
* **Fase 4 (Pulido)**: reglas avanzadas, auto-etiquetado, empaquetado Tauri.

---

## 📈 Métricas útiles

* Focus ratio (% de minutos en foco vs. planificados).
* Profundidad de sesión (ciclos encadenados).
* Interrupciones por sesión.
* Distribución por Proyecto/Actividad.
* % de acierto de la recomendación (aceptar tarea top).

---

## 📜 Licencia

Pendiente de definir (MIT / GPL / Propietaria).
¿Quieres que también te prepare un **`/docs/template.docx` de ejemplo con PhpWord** (con los placeholders listos) para que subas junto al README?
```
