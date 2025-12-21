# NYA Core Language SDK

Create custom language files for NYA Core by following this guide.

## File Format

Language files are JSON files with the following structure:

```json
{
    "info": {
        "id": "es",
        "name": "Spanish",
        "native_name": "Español",
        "author": "Your Name",
        "version": "1.0.0"
    },
    "strings": {
        "nav.counter": "Contador de Golpes",
        "nav.obs": "Integración OBS",
        ...
    }
}
```

## Creating a Language File

1. Create a new JSON file in the `languages` folder (e.g., `fr.json` for French)
2. Update the `info` section with your language details
3. Translate all strings in the `strings` section
4. Restart NYA Core to see your language in Settings > Language

## String Keys Reference

### Navigation
| Key | English | Description |
|-----|---------|-------------|
| `nav.counter` | Hit Counter | Main counter menu item |
| `nav.obs` | OBS Integration | OBS settings menu item |
| `nav.multirun` | Multi-Run | Multi-run mode menu item |
| `nav.games` | Games | Games management menu item |
| `nav.stats` | Statistics | Statistics menu item |
| `nav.settings` | Settings | Settings menu item |

### Counter
| Key | English | Description |
|-----|---------|-------------|
| `counter.hits` | HITS | Hits display label |
| `counter.addHit` | Add Hit | Add hit button |
| `counter.undoHit` | Undo | Undo hit button |
| `counter.bossDefeated` | Boss Defeated | Boss defeated button |
| `counter.resetRun` | Reset Run | Reset run button |
| `counter.splits` | Splits | Splits section title |
| `counter.progress` | Progress | Progress label |

### Status
| Key | English | Description |
|-----|---------|-------------|
| `status.idle` | IDLE | Idle status |
| `status.active` | ACTIVE | Active status |
| `status.completed` | COMPLETED | Completed status |
| `status.hit` | HIT | Hit status |

### Timer
| Key | English | Description |
|-----|---------|-------------|
| `timer.start` | Start | Start timer button |
| `timer.stop` | Stop | Stop timer button |
| `timer.reset` | Reset | Reset timer button |

### Multi-Run
| Key | English | Description |
|-----|---------|-------------|
| `multirun.title` | Multi-Run Mode | Multi-run mode title |
| `multirun.start` | Start Multi-Run | Start multi-run button |
| `multirun.clearAll` | Clear All | Clear all button |
| `multirun.addGame` | Add Game | Add game button |
| `multirun.saveAsPB` | Save as PB | Save as personal best button |
| `multirun.totalHits` | Total Hits | Total hits label |
| `multirun.presets` | Saved Presets | Saved presets section |

### Settings
| Key | English | Description |
|-----|---------|-------------|
| `settings.title` | Settings | Settings title |
| `settings.hotkeys` | Hotkeys | Hotkeys section |
| `settings.theme` | Theme | Theme section |
| `settings.language` | Language | Language section |
| `settings.data` | Data | Data section |
| `settings.export` | Export JSON | Export button |
| `settings.import` | Import JSON | Import button |

### Dialogs
| Key | English | Description |
|-----|---------|-------------|
| `dialogs.resetRun` | Reset Run | Reset dialog title |
| `dialogs.resetRunConfirm` | Are you sure you want to reset the run? | Reset confirmation |
| `dialogs.saveAsPB` | Save as PB | Save PB dialog title |
| `dialogs.saveAsPBConfirm` | Save this run as your personal best? | Save PB confirmation |
| `dialogs.clearStats` | Clear Statistics | Clear stats dialog title |
| `dialogs.clearStatsConfirm` | Are you sure you want to clear all statistics? | Clear stats confirmation |
| `dialogs.factoryReset` | Factory Reset | Factory reset dialog title |
| `dialogs.factoryResetConfirm` | This will delete all data. Continue? | Factory reset confirmation |

### Common
| Key | English | Description |
|-----|---------|-------------|
| `common.save` | Save | Save button |
| `common.cancel` | Cancel | Cancel button |
| `common.confirm` | Confirm | Confirm button |
| `common.ok` | OK | OK button |
| `common.delete` | Delete | Delete button |
| `common.yes` | Yes | Yes option |
| `common.no` | No | No option |
| `common.attempts` | Attempts | Attempts label |
| `common.completions` | Completions | Completions label |
| `common.bestHits` | Best Hits | Best hits label |
| `common.bestTime` | Best Time | Best time label |

## Example: Spanish Translation

```json
{
    "info": {
        "id": "es",
        "name": "Spanish",
        "native_name": "Español",
        "author": "NYA Core",
        "version": "1.0.0"
    },
    "strings": {
        "nav.counter": "Contador de Golpes",
        "nav.obs": "Integración OBS",
        "nav.multirun": "Multi-Juego",
        "nav.games": "Juegos",
        "nav.stats": "Estadísticas",
        "nav.settings": "Configuración",

        "counter.hits": "GOLPES",
        "counter.addHit": "Añadir Golpe",
        "counter.undoHit": "Deshacer",
        "counter.bossDefeated": "Jefe Derrotado",
        "counter.resetRun": "Reiniciar",

        "status.idle": "INACTIVO",
        "status.active": "ACTIVO",
        "status.completed": "COMPLETADO",

        "common.save": "Guardar",
        "common.cancel": "Cancelar",
        "common.confirm": "Confirmar",
        "common.ok": "Aceptar",
        "common.delete": "Eliminar"
    }
}
```

## Tips

- You don't need to translate every string - missing strings will fall back to English
- Keep the string keys exactly as shown - they are case-sensitive
- Test your translation by selecting it in Settings > Language
- Share your translations with the community!

## Submitting Translations

Want to contribute your translation? Create a pull request or open an issue on GitHub with your language file.
