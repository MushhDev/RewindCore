# RewindCore

<p align="center">
  <img src="https://img.shields.io/badge/Version-1.0.0-blue.svg" alt="Version">
  <img src="https://img.shields.io/badge/Minecraft-1.20.4 1.21.11-green.svg" alt="Minecraft">
  <img src="https://img.shields.io/badge/Java-17+-orange.svg" alt="Java">
  <img src="https://img.shields.io/badge/Author-MushHX-purple.svg" alt="Author">
</p>

## Sistema Avanzado de Rollback de Inventarios

**RewindCore** es un plugin de InventoryRollback de nueva generación que redefine la forma en que los servidores gestionan la recuperación de inventarios. Su sistema avanzado de snapshots captura automáticamente el estado completo del jugador.

---

## Características Principales

### Sistema de Snapshots Completo
- **Inventario principal** (slots 0-35)
- **Armadura** (casco, pechera, pantalones, botas)
- **Offhand** (mano secundaria)
- **Experiencia** (nivel y XP total)
- **Efectos de pociones** activos
- **Estado del jugador** (salud, hambre, saturación)
- **Ubicación** al momento del snapshot
- **Ender Chest** contenido completo
- **Datos NBT** avanzados

### Eventos Inteligentes
| Evento | Descripción |
|--------|-------------|
| Muertes | PvP, mobs, void, caídas, fuego, etc. |
| Conexión | Join, quit, kick, timeout |
| Teletransporte | Comandos, portales, plugins |
| Cambio de mundo | Entre dimensiones |
| Gamemode | Cambios de modo de juego |
| Staff | Acciones administrativas |
| Manual | Snapshots bajo demanda |
| Programado | Backups automáticos |

### Restauración Contextual
- **Completa**: Restaura todo el estado del jugador
- **Selectiva**: Solo inventario, armadura, XP o efectos
- **Slots específicos**: Elige exactamente qué restaurar
- **Vista previa**: Inspecciona antes de restaurar
- **Comparación**: Compara snapshot vs estado actual

### Seguridad Anti-Exploit
- Detección de duplicación de items
- Bloqueo de restauraciones múltiples
- Verificación de anomalías (stacks imposibles, enchants excesivos)
- Protección de economía
- Rate limiting
- Alertas al staff en tiempo real

---

## Comandos

| Comando | Descripción | Permiso |
|---------|-------------|---------|
| `/rw` | Abre el menú principal | `rewindcore.view` |
| `/rw help` | Muestra la ayuda | - |
| `/rw history <jugador> [página]` | Ver historial de snapshots | `rewindcore.view` |
| `/rw restore <jugador> <id>` | Restaurar un snapshot | `rewindcore.restore` |
| `/rw preview <jugador> <id>` | Vista previa de snapshot | `rewindcore.view` |
| `/rw snapshot <jugador> [razón]` | Crear snapshot manual | `rewindcore.admin` |
| `/rw search <filtros>` | Buscar snapshots | `rewindcore.search` |
| `/rw confirm` | Confirmar restauración | - |
| `/rw cancel` | Cancelar operación | - |
| `/rw audit [página]` | Ver registro de auditoría | `rewindcore.audit` |
| `/rw purge <días> confirm` | Purgar datos antiguos | `rewindcore.purge` |
| `/rw reload` | Recargar configuración | `rewindcore.reload` |

---

## Permisos

```yaml
rewindcore.*                    # Acceso completo
rewindcore.admin               # Comandos administrativos
rewindcore.restore             # Restaurar propio inventario
rewindcore.restore.others      # Restaurar inventarios de otros
rewindcore.restore.selective   # Restauraciones selectivas
rewindcore.restore.offline     # Restaurar jugadores offline
rewindcore.view                # Ver propio historial
rewindcore.view.others         # Ver historial de otros
rewindcore.search              # Buscar en historial
rewindcore.audit               # Ver auditoría
rewindcore.reload              # Recargar configuración
rewindcore.purge               # Purgar datos
rewindcore.bypass.cooldown     # Bypass de cooldown
rewindcore.bypass.limit        # Bypass de límite diario
rewindcore.bypass.confirm      # Bypass de confirmación
rewindcore.notify              # Recibir notificaciones
```

---

## Configuración

RewindCore utiliza múltiples archivos de configuración para mayor organización:

| Archivo | Descripción |
|---------|-------------|
| `config.yml` | Configuración general y de snapshots |
| `storage.yml` | Configuración de base de datos |
| `events.yml` | Configuración de eventos y triggers |
| `messages.yml` | Mensajes personalizables |
| `gui.yml` | Configuración de interfaces gráficas |
| `security.yml` | Seguridad y anti-exploit |

### Almacenamiento Soportado
- **SQLite** (por defecto, ideal para servidores pequeños/medianos)
- **MySQL** (recomendado para servidores grandes)
- **MariaDB** (alternativa a MySQL)

---

## API para Desarrolladores

```java
// Obtener la API
RewindAPI api = Bukkit.getServicesManager().load(RewindAPI.class);

// Crear un snapshot
api.createSnapshot(player, SnapshotReason.API, "Mi razón personalizada")
    .thenAccept(snapshot -> {
        System.out.println("Snapshot creado: #" + snapshot.getId());
    });

// Obtener snapshots de un jugador
api.getSnapshots(playerUUID).thenAccept(snapshots -> {
    for (PlayerSnapshot snapshot : snapshots) {
        System.out.println(snapshot.getReason().getDisplayName());
    }
});

// Restaurar un snapshot
api.restoreSnapshot(target, snapshot, executor).thenAccept(result -> {
    if (result.isSuccess()) {
        System.out.println("Restauración exitosa!");
    }
});
```

---

## Instalación

1. Descarga `RewindCore.jar`
2. Coloca el archivo en la carpeta `plugins/`
3. Reinicia el servidor
4. Configura los archivos en `plugins/RewindCore/`
5. Usa `/rw reload` para aplicar cambios

### Requisitos
- Java 17 o superior
- Spigot/Paper 1.20.4+
- (Opcional) PlaceholderAPI
- (Opcional) Vault

---

## Dependencias Opcionales

| Plugin | Uso |
|--------|-----|
| PlaceholderAPI | Placeholders en mensajes |
| Vault | Integración con economía |
| LuckPerms | Permisos contextuales |

---

## Soporte Multi-Servidor

RewindCore soporta redes BungeeCord/Velocity:

```yaml
# En config.yml
multi-server:
  enabled: true
  server-id: "survival"
  sync-snapshots: true
```

---

## Rendimiento

- **Operaciones asíncronas**: Todo el I/O es no-bloqueante
- **Sistema de caché**: Reduce consultas a la base de datos
- **Compresión de datos**: Reduce uso de almacenamiento
- **Limpieza automática**: Elimina snapshots antiguos
- **Batch processing**: Operaciones masivas optimizadas

---

## Changelog

### v1.0.0
- Release inicial
- Sistema completo de snapshots
- GUI interactiva
- Soporte SQLite/MySQL
- API pública
- Sistema anti-exploit
- Auditoría completa

---

## Créditos

**Desarrollado por MushHX**

---

## Licencia

Este plugin es de uso privado. Para consultas de licenciamiento, contactar al autor.
