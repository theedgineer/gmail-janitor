# 🧹 Gmail Janitor - Workflow Simple para n8n

[![n8n](https://img.shields.io/badge/n8n-workflow-blue.svg)](https://n8n.io/)
[![Gmail](https://img.shields.io/badge/Gmail-API-red.svg)](https://developers.google.com/gmail/api)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

Un workflow automatizado simple y eficiente para mantener tu Gmail limpio eliminando correos de Papelera y Spam cada 2 horas.

## 🚀 Características Principales

- ⚡ **Simple y Directo**: Sin complejidades innecesarias
- 🔄 **Automático**: Se ejecuta cada 2 horas sin intervención
- 📊 **Reportes**: Notificaciones con estadísticas de limpieza
- 🛡️ **Seguro**: Solo elimina papelera y spam
- 📱 **Notificaciones**: Integración con Pushover

## 📋 Descripción

**Gmail Janitor** es un agente automatizado que se ejecuta cada 2 horas para mantener tu cuenta de Gmail organizada. Elimina permanentemente todos los correos de las carpetas de Papelera (Trash) y Spam, y te notifica sobre las acciones realizadas.

## ✨ Características

- ⏰ **Ejecución automática**: Cada 2 horas
- 🗑️ **Limpieza de Papelera**: Elimina todos los correos en TRASH
- 🚫 **Limpieza de Spam**: Elimina todos los correos en SPAM
- 📊 **Conteo automático**: Cuenta correos eliminados
- 📱 **Notificaciones**: Informa cuando se realizan eliminaciones
- 🔄 **Procesamiento paralelo**: Maneja Papelera y Spam simultáneamente
- ⚡ **Optimizado para n8n v2**: Usa las últimas versiones de nodos
- 🎯 **Diseño simple**: Sin complejidades innecesarias

## 🏗️ Arquitectura del Workflow

```
[Cada 2 Horas] 
       ↓
   ┌─────────┐     ┌─────────────┐
   │ Get Trash │ ──→ │ Delete Trash │
   └─────────┘     └─────────────┘
       ↓               ↓
   ┌─────────┐     ┌─────────────┐
   │ Get Spam │ ──→ │ Delete Spam │
   └─────────┘     └─────────────┘
       ↓               ↓
       └───→ [Wait for Both] ──→ [Count Summary] ──→ [IF > 0] ──→ [Pushover Notif]
```

## 🔧 Configuración Requerida

### 1. Credenciales de Gmail OAuth2
- Ve a **Settings > Credentials** en n8n
- Crea una nueva credencial de tipo **"Gmail OAuth2 API"**
- Configura la autenticación con tu cuenta de Google
- Asigna esta credencial a los nodos:
  - `1. Get Trash`
  - `2. Get Spam`
  - `Delete Trash Msg`
  - `Delete Spam Msg`

### 2. Credenciales de Pushover (Opcional)
- Si quieres recibir notificaciones, configura Pushover
- Asigna la credencial al nodo `Pushover Notif`

### 3. Permisos de Gmail
Asegúrate de que tu cuenta de Google tenga:
- **Gmail API** habilitado
- **Permisos de lectura y eliminación** de correos

## ⚡ Instalación Rápida

1. **Descargar**: Haz clic en `janitor.json` y descárgalo
2. **Importar**: En n8n → Workflows → Import from File → Selecciona `janitor.json`
3. **Configurar**: Agrega tus credenciales de Gmail OAuth2
4. **Activar**: Activa el workflow y ¡listo!

## 📦 Instalación Detallada

1. **Importar el Workflow**:
   - Descarga el archivo `janitor.json`
   - En n8n, ve a **Workflows > Import from File**
   - Selecciona el archivo `janitor.json`

2. **Configurar Credenciales**:
   - Configura las credenciales de Gmail OAuth2
   - (Opcional) Configura las credenciales de Pushover

3. **Activar el Workflow**:
   - Una vez configuradas las credenciales, activa el workflow
   - El workflow comenzará a ejecutarse automáticamente cada 2 horas

## 🎯 Funcionamiento

### Flujo de Ejecución

1. **Trigger**: El workflow se activa cada 2 horas
2. **Obtención Paralela**: 
   - Obtiene todos los correos de la carpeta TRASH
   - Obtiene todos los correos de la carpeta SPAM
3. **Eliminación Directa**: 
   - Elimina permanentemente cada correo de Papelera
   - Elimina permanentemente cada correo de Spam
4. **Sincronización**: Espera a que ambas operaciones terminen
5. **Conteo**: Cuenta cuántos correos se eliminaron en total
6. **Notificación**: Si se eliminó al menos un correo, envía una notificación

### Ejemplo de Notificación
```
🧹 Janitor Report
• Trash: 15
• Spam: 8
• Total: 23
• Time: 2024-01-15 14:30:00
```

## ⚙️ Personalización

### Cambiar la Frecuencia
Para modificar la frecuencia de ejecución, edita el nodo `Cada 2 Horas`:
- Cambia el valor de `interval` (ej: 1, 4, 6, 12)
- Cambia la `unit` (hours, minutes)

### Agregar Límites (Opcional)
Si tienes muchos correos, puedes agregar límites en los nodos de Gmail:
```json
"parameters": {
  "resource": "message",
  "operation": "getAll",
  "limit": 1000,  // Límite de correos
  "filters": {
    "labelIds": ["TRASH"]
  },
  "simplify": true
}
```

### Deshabilitar Notificaciones
Para deshabilitar las notificaciones:
- Elimina o desconecta el nodo `Pushover Notif`
- O configura `continueOnFail: true` en el nodo

## 🛡️ Consideraciones de Seguridad

- **Eliminación Permanente**: Los correos eliminados NO se pueden recuperar
- **Permisos Mínimos**: El workflow solo necesita acceso a Gmail
- **Credenciales Seguras**: Las credenciales se almacenan de forma segura en n8n
- **Logs de Actividad**: n8n mantiene logs de todas las ejecuciones

## 🔍 Troubleshooting

### Problema: "No se pueden leer los correos de Gmail"
**Solución**: Verifica que las credenciales de Gmail OAuth2 estén configuradas correctamente

### Problema: "Workflow no se ejecuta automáticamente"
**Solución**: Asegúrate de que el workflow esté activado (`active: true`)

### Problema: "Notificaciones no llegan"
**Solución**: Verifica que las credenciales de Pushover estén configuradas

### Problema: "Error de límites de API"
**Solución**: Agrega un límite en los nodos de Gmail o reduce la frecuencia de ejecución

## 📊 Monitoreo

- **Logs de Ejecución**: Revisa los logs en n8n para ver el historial
- **Notificaciones**: Recibe reportes de cada limpieza realizada
- **Métricas**: El workflow cuenta automáticamente los correos eliminados

## 🔄 Versiones

- **Versión Actual**: v1.117.2-SIMPLE-CLEAN-2H
- **Compatibilidad**: n8n v2.x
- **Última Actualización**: Enero 2024
- **Diseño**: Simple y directo, sin complejidades innecesarias

## 📝 Notas Importantes

- ⚠️ **Los correos eliminados NO se pueden recuperar**
- 🔒 **Mantén tus credenciales seguras**
- 📊 **Revisa periódicamente los logs de ejecución**
- 🎯 **El workflow está optimizado para uso personal**
- 🚀 **Diseño simple para máxima confiabilidad**

## 🎨 Ventajas del Diseño Simple

- **Menos puntos de falla**: Menos nodos = menos posibilidades de error
- **Fácil mantenimiento**: Estructura clara y directa
- **Mejor rendimiento**: Sin validaciones innecesarias
- **Configuración rápida**: Setup en minutos
- **Confiabilidad**: Funciona consistentemente

## 🤝 Contribuciones

¡Las contribuciones son bienvenidas! Si quieres mejorar este proyecto:

1. **Fork** el repositorio
2. **Crea** una rama para tu feature (`git checkout -b feature/nueva-funcionalidad`)
3. **Commit** tus cambios (`git commit -am 'Agrega nueva funcionalidad'`)
4. **Push** a la rama (`git push origin feature/nueva-funcionalidad`)
5. **Abre** un Pull Request

### 🐛 Reportar Problemas

Si encuentras algún problema:
1. Verifica que no esté ya reportado en [Issues](../../issues)
2. Incluye logs de error si es posible
3. Describe tu configuración de n8n
4. Especifica la versión de n8n que usas

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo [LICENSE](LICENSE) para más detalles.

---

**¡Mantén tu Gmail limpio y organizado con Gmail Janitor! 🧹✨**

*Diseño simple, funcionamiento confiable.*
