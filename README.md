# 🏭 Sistema de Control de Producción por QR

Sistema web de registro de tiempos de producción mediante códigos QR, con almacenamiento en Google Sheets y visualización en tiempo real.

![Estado](https://img.shields.io/badge/Estado-Activo-success)
![Versión](https://img.shields.io/badge/Versión-1.0.0-blue)
![Licencia](https://img.shields.io/badge/Licencia-MIT-green)

## 🎯 Características Principales

- ✅ Escaneo de códigos QR con cámara del dispositivo
- ✅ Registro de operarios, máquinas y órdenes de trabajo
- ✅ 5 tipos de eventos: Inicio, Pausa, Reanudar, Fin Jornada, Fin Definitivo
- ✅ Almacenamiento automático en Google Sheets
- ✅ Dashboard en tiempo real para supervisores
- ✅ Gráficos y reportes de eficiencia
- ✅ Sistema de notificaciones por email
- ✅ PWA instalable en dispositivos móviles
- ✅ 100% gratuito (Google Sheets + GitHub Pages)

## 🚀 Demo en Vivo

**URL:** [https://TU-USUARIO.github.io/control-produccion/](https://TU-USUARIO.github.io/control-produccion/)

## 📋 Requisitos Previos

- Cuenta de Google (Gmail)
- Cuenta de GitHub
- Navegador moderno con soporte de cámara
- Dispositivo móvil o computadora con cámara

## 🔧 Instalación Rápida

### 1. Configurar Google Sheets

1. Crea una nueva hoja de cálculo en Google Sheets
2. Nómbrala "Control_Produccion"
3. Crea las siguientes columnas en la primera fila:
   ```
   Timestamp | Operario_ID | Maquina_ID | OT_ID | Evento | Comentarios | Turno | Duracion_Min | Eficiencia | Status_OT
   ```

### 2. Configurar Google Apps Script

1. En tu Google Sheet: **Extensiones → Apps Script**
2. Copia el código de `backend/codigo.gs`
3. **Implementar → Nueva implementación → Aplicación web**
4. Configuración:
   - Ejecutar como: **Yo**
   - Acceso: **Cualquiera**
5. Copia la URL generada

### 3. Desplegar en GitHub Pages

1. Haz fork de este repositorio o clónalo
2. En `index.html`, busca:
   ```javascript
   const API_URL = 'TU_URL_DE_GOOGLE_APPS_SCRIPT_AQUI';
   ```
3. Reemplaza con tu URL del paso 2
4. Ve a **Settings → Pages**
5. Activa GitHub Pages desde la rama `main`

### 4. Generar Códigos QR

Usa [QR Code Generator](https://www.qr-code-generator.com/) para crear QRs con estas URLs:

**Operarios:**
```
https://TU-USUARIO.github.io/control-produccion/?type=operario&id=OP-001
```

**Máquinas:**
```
https://TU-USUARIO.github.io/control-produccion/?type=maquina&id=TORNO-CNC-01
```

**Órdenes:**
```
https://TU-USUARIO.github.io/control-produccion/?type=ot&id=OT-2025-001
```

## 📱 Uso del Sistema

### Para Operarios

1. Abre la aplicación en tu móvil
2. Escanea tu QR personal
3. Escanea el QR de la máquina
4. Escanea el QR de la orden de trabajo
5. Selecciona el evento (INICIO, PAUSA, etc.)
6. Presiona "Enviar Registro"

### Para Supervisores

- Accede al **Dashboard** para ver actividad en tiempo real
- Consulta **Reportes** para análisis detallados
- Revisa Google Sheets para datos completos

## 🏗️ Arquitectura

```
┌─────────────────┐
│   Frontend      │
│  (GitHub Pages) │
│                 │
│  - index.html   │
│  - dashboard    │
│  - reportes     │
└────────┬────────┘
         │
         │ HTTPS
         │
┌────────▼────────┐
│   Backend       │
│ (Apps Script)   │
│                 │
│  - Validación   │
│  - Cálculos     │
│  - Notificaciones│
└────────┬────────┘
         │
         │
┌────────▼────────┐
│   Database      │
│ (Google Sheets) │
│                 │
│  - Registros    │
│  - Analytics    │
└─────────────────┘
```

## 📊 Estructura de Datos

### Campos Registrados

| Campo | Tipo | Descripción |
|-------|------|-------------|
| Timestamp | DateTime | Fecha/hora automática |
| Operario_ID | String | ID del operario (QR) |
| Maquina_ID | String | ID de la máquina (QR) |
| OT_ID | String | Número de orden |
| Evento | Enum | Tipo de evento |
| Comentarios | String | Observaciones |
| Turno | String | Calculado automáticamente |
| Duracion_Min | Number | Minutos transcurridos |
| Eficiencia | Number | Porcentaje vs estimado |
| Status_OT | String | Estado actual |

## 🎨 Personalización

### Colores y Estilos

Edita las variables CSS en `index.html`:

```css
--primary: #667eea;
--secondary: #764ba2;
--success: #10b981;
--warning: #f59e0b;
```

### Eventos Personalizados

Modifica el array de eventos en `index.html`:

```javascript
const eventos = ['INICIO', 'PAUSA', 'REANUDAR', 'FIN_JORNADA', 'FIN_DEFINITIVO'];
```

## 🔒 Seguridad

- ✅ Validación de datos en frontend y backend
- ✅ Sanitización de inputs
- ✅ Rate limiting por operario
- ✅ Detección de duplicados
- ✅ Logs de seguridad
- ✅ HTTPS obligatorio

## 📧 Notificaciones

El sistema envía alertas automáticas por email para:

- ⚠️ Pausas prolongadas (> 30 min)
- 🔴 Operarios sin actividad (> 60 min)
- 📉 Eficiencia por debajo del objetivo
- 📊 Reporte diario a las 22:00

Configura destinatarios en `backend/codigo.gs`:

```javascript
const EMAIL_CONFIG = {
  supervisores: ['supervisor@tuempresa.com'],
  gerencia: ['gerente@tuempresa.com']
};
```

## 📈 Métricas y KPIs

- **OEE** (Overall Equipment Effectiveness)
- **Tiempo Ciclo Real vs Estimado**
- **Eficiencia por Operario**
- **Utilización de Máquinas**
- **Análisis de Pausas**

## 🐛 Solución de Problemas

### La cámara no funciona

1. Verifica permisos del navegador
2. Asegúrate de usar HTTPS (GitHub Pages lo tiene automático)
3. Recarga la página

### Los datos no se guardan

1. Verifica la URL del Apps Script en `index.html`
2. Confirma que el Web App está implementado como "Cualquiera"
3. Revisa la consola del navegador (F12)

### QR no se escanea

1. Mejora la iluminación
2. Limpia la lente de la cámara
3. Asegúrate de que el QR mida al menos 5x5 cm

## 🤝 Contribuciones

Las contribuciones son bienvenidas:

1. Haz fork del proyecto
2. Crea una rama para tu feature (`git checkout -b feature/nueva-funcionalidad`)
3. Commit tus cambios (`git commit -m 'Agregar nueva funcionalidad'`)
4. Push a la rama (`git push origin feature/nueva-funcionalidad`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver archivo `LICENSE` para más detalles.

## 👥 Autores

- **Tu Nombre** - *Desarrollo inicial* - [TuGitHub](https://github.com/TU-USUARIO)

## 🙏 Agradecimientos

- Google Apps Script por el backend gratuito
- GitHub Pages por el hosting
- jsQR por la librería de escaneo
- Chart.js por las visualizaciones

## 📞 Soporte

- 📧 Email: soporte@tuempresa.com
- 📖 Wiki: [Documentación completa](https://github.com/TU-USUARIO/control-produccion/wiki)
- 🐛 Issues: [Reportar problemas](https://github.com/TU-USUARIO/control-produccion/issues)

## 🗺️ Roadmap

- [ ] Integración con ERP
- [ ] App nativa iOS/Android
- [ ] Reconocimiento facial como backup
- [ ] Integración con IoT/sensores
- [ ] Machine Learning para predicciones
- [ ] Multi-idioma
- [ ] Modo oscuro

---

**⭐ Si este proyecto te fue útil, dale una estrella en GitHub!**

*Última actualización: Octubre 2025*