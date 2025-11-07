# Estrategia de Transcripción Masiva - Máxima Calidad

## Análisis de la Situación Actual

### Videos Identificados
**Total: 18 videos** en `lessons/01_practica/`

| Lección | Video | Tamaño | Duración |
|---------|-------|--------|----------|
| 12-practice-02 | practica_02_1.mkv | 11 GB | ~2.75h |
| 12-practice-02 | practica_02_2.mkv | 2.8 GB | ~0.7h |
| 13-practice-03 | practica_03.mkv | 9.1 GB | ~2.36h |
| 14-practice-04 | practica_04.mkv | 13 GB | ~3.25h |
| 15-practice-05 | practica_05.mkv | 11 GB | ~2.75h |
| 16-practice-06 | practica_06.mkv | 12 GB | ~3h |
| 17-practice-07 | practica_07.mkv | 9.8 GB | ~2.45h |
| 18-practice-08 | practica_08.mkv | 12 GB | ~3h |
| 18-practice-08 | sersans.mkv | 553 MB | ~0.14h ✅ |
| 19-practice-09 | practica_09.mkv | 12 GB | ~3h |
| 20-practice-10 | practica_10.mkv | 12 GB | ~3h |
| 21-practice-11 | practica_11.mkv | 12 GB | ~3h |
| 22-practice-12 | practica_12.mkv | 11 GB | ~2.75h |
| 23-practice-13 | practica_13.mkv | 11 GB | ~2.75h |
| 24-practice-14 | practica_14.mkv | 12 GB | ~3h |
| 25-practice-15 | practica_15.mkv | 11 GB | ~2.75h |
| 26-practice-16 | practica_16.mkv | 12 GB | ~3h |
| 27-practice-17 | practica_17.mkv | 15 GB | ~3.75h |

**Total estimado: ~45 horas de contenido**

### Estado de Transcripciones
- **✅ Completada**: `18-practice-08/source/sersans.mkv` (8.4 min)
  - Calidad excelente: 80 segmentos, 5,704 caracteres
  - Detección correcta: Español
  - Formatos: JSON, TXT, VTT, SRT
- **⏳ Pendientes**: 17 videos principales (~45h total)

## Estrategia Propuesta: Procesamiento en 3 Fases

### Fase 1: Configuración y Validación (INMEDIATA)

#### 1.1 Modelo Whisper Óptimo
**Recomendación: `medium` o `large-v2`** para máxima calidad

| Modelo | Precisión | Velocidad | Memoria | Recomendación |
|--------|-----------|-----------|---------|---------------|
| tiny | Baja | 32x | 1 GB | ❌ No para contenido técnico |
| base | Media | 16x | 1 GB | ❌ Pérdida de términos técnicos |
| small | Buena | 6x | 2 GB | ✅ Ya probado (8:22 → 1:20) |
| medium | Muy buena | 2x | 5 GB | ✅✅ **RECOMENDADO** |
| large-v2 | Excelente | 1x | 10 GB | ✅ Para contenido crítico |

**Justificación para `medium`**:
- Contenido técnico (trading algorítmico)
- Términos especializados: RSI, medias móviles, Bollinger, etc.
- Balance óptimo: calidad vs tiempo
- AMD Ryzen 7 5800X puede manejarlo

#### 1.2 Prompt Optimizado para Trading
```python
TRADING_PROMPT = """
Transcripción de curso de trading algorítmico en español.
Términos clave: RSI, MACD, medias móviles, Bollinger Bands, backtesting,
TradeStation, TradingView, EasyLanguage, Pine Script, volatilidad,
stop loss, take profit, gestión monetaria, money management, drawdown,
ratio Sharpe, optimización, overfitting, walk-forward, out-of-sample.
"""
```

#### 1.3 Configuración FFmpeg para Audio de Alta Calidad
```python
ffmpeg_options = {
    'format': 'wav',
    'acodec': 'pcm_s16le',  # 16-bit PCM
    'ac': 1,  # Mono (suficiente para voz)
    'ar': '16000'  # 16kHz (óptimo para Whisper)
}
```

### Fase 2: Procesamiento por Lotes Inteligente

#### 2.1 Estrategia de Priorización
**Orden recomendado** (menor a mayor duración):

1. **Batch 1** - Videos cortos (~2-2.5h cada uno):
   - `12-practice-02/practica_02_2.mkv` (2.8 GB)
   - `13-practice-03/practica_03.mkv` (9.1 GB)
   - `17-practice-07/practica_07.mkv` (9.8 GB)

2. **Batch 2** - Videos medianos (~2.75h cada uno):
   - `12-practice-02/practica_02_1.mkv` (11 GB)
   - `15-practice-05/practica_05.mkv` (11 GB)
   - `22-practice-12/practica_12.mkv` (11 GB)
   - `23-practice-13/practica_13.mkv` (11 GB)
   - `25-practice-15/practica_15.mkv` (11 GB)

3. **Batch 3** - Videos largos (~3h cada uno):
   - `16-practice-06/practica_06.mkv` (12 GB)
   - `18-practice-08/practica_08.mkv` (12 GB)
   - `19-practice-09/practica_09.mkv` (12 GB)
   - `20-practice-10/practica_10.mkv` (12 GB)
   - `21-practice-11/practica_11.mkv` (12 GB)
   - `24-practice-14/practica_14.mkv` (12 GB)
   - `26-practice-16/practica_16.mkv` (12 GB)

4. **Batch 4** - Videos muy largos:
   - `14-practice-04/practica_04.mkv` (13 GB)
   - `27-practice-17/practica_17.mkv` (15 GB)

#### 2.2 Estimación de Tiempos (Modelo `medium`)

**Ratio esperado**: ~2:1 (2 horas de video = 1 hora procesamiento)

| Batch | Videos | Tiempo Video | Tiempo Procesamiento |
|-------|--------|--------------|----------------------|
| Batch 1 | 3 | ~7h | ~3.5h |
| Batch 2 | 5 | ~14h | ~7h |
| Batch 3 | 7 | ~21h | ~10.5h |
| Batch 4 | 2 | ~7h | ~3.5h |
| **TOTAL** | **17** | **~49h** | **~24.5h** |

**Tiempo total estimado: 24-30 horas** (con modelo `medium`)

#### 2.3 Procesamiento Nocturno Automatizado
```python
# Configuración para procesamiento desatendido
BATCH_CONFIG = {
    'model': 'medium',
    'language': 'es',
    'prompt': TRADING_PROMPT,
    'keep_audio': True,
    'formats': ['txt', 'vtt', 'srt', 'json'],
    'fp16': False,  # Mayor precisión (si CPU)
    'beam_size': 5,  # Mayor precisión vs velocidad
    'best_of': 5,  # Mejor resultado de 5 intentos
    'temperature': 0.0  # Determinístico
}
```

### Fase 3: Control de Calidad y Validación

#### 3.1 Métricas de Calidad Automáticas
```python
quality_metrics = {
    'segment_count': 'Expected: ~1 per minute',
    'char_count': 'Expected: ~60-80 chars/segment',
    'language_confidence': 'Expected: >0.95',
    'trading_terms_density': 'Expected: >5 terms per 100 words',
    'silence_detection': 'Flag segments >30s silence'
}
```

#### 3.2 Validación Post-Procesamiento
- **✅ Verificar generación de 4 formatos**: TXT, VTT, SRT, JSON
- **✅ Detección de términos clave**: RSI, medias móviles, etc.
- **✅ Duración vs segmentos**: ratio coherente
- **✅ Errores comunes**: "apolo" → "Apollo", "tradestation" → "TradeStation"

## Script de Procesamiento Masivo

### Estructura del Script
```python
#!/usr/bin/env python3
"""
batch_transcribe.py - Procesamiento masivo con máxima calidad
"""

import os
import json
import time
from pathlib import Path
from typing import List, Dict
import whisper
import ffmpeg

# Configuración
LESSONS_DIR = Path("lessons/01_practica")
MODEL_SIZE = "medium"  # o "large-v2" para máxima calidad
BATCH_SIZE = 3  # Videos por lote
LOG_FILE = "transcription_log.json"

# Funciones principales
def find_pending_videos() -> List[Path]:
    """Encuentra videos sin transcribir"""
    pending = []
    for lesson_dir in sorted(LESSONS_DIR.iterdir()):
        if not lesson_dir.is_dir():
            continue

        # Buscar .mkv files
        mkv_files = list(lesson_dir.glob("*.mkv"))
        for video in mkv_files:
            # Verificar si ya tiene transcripción
            transcription_dir = lesson_dir / "transcription"
            if not (transcription_dir / "audio.json").exists():
                pending.append(video)

    return pending

def transcribe_video(video_path: Path, model: whisper.Whisper) -> Dict:
    """Transcribe un video con máxima calidad"""
    lesson_dir = video_path.parent
    output_dir = lesson_dir / "transcription"
    output_dir.mkdir(exist_ok=True)

    # 1. Extraer audio de alta calidad
    audio_path = output_dir / "audio.wav"
    extract_audio(video_path, audio_path)

    # 2. Transcribir con Whisper
    result = model.transcribe(
        str(audio_path),
        language="es",
        initial_prompt=TRADING_PROMPT,
        fp16=False,
        beam_size=5,
        best_of=5,
        temperature=0.0
    )

    # 3. Exportar múltiples formatos
    export_formats(result, output_dir)

    # 4. Métricas de calidad
    metrics = calculate_quality_metrics(result, video_path)

    return metrics

def batch_process(videos: List[Path], batch_size: int = 3):
    """Procesa videos en lotes"""
    print(f"Loading Whisper model: {MODEL_SIZE}")
    model = whisper.load_model(MODEL_SIZE)

    total = len(videos)
    results = []

    for i, video in enumerate(videos, 1):
        print(f"\n[{i}/{total}] Processing: {video.name}")
        start_time = time.time()

        try:
            metrics = transcribe_video(video, model)
            elapsed = time.time() - start_time

            results.append({
                'video': str(video),
                'status': 'success',
                'duration': elapsed,
                'metrics': metrics
            })

            print(f"✅ Completed in {elapsed/60:.1f} min")

        except Exception as e:
            results.append({
                'video': str(video),
                'status': 'error',
                'error': str(e)
            })
            print(f"❌ Error: {e}")

        # Guardar log incremental
        save_log(results)

    return results

# Ejecución principal
if __name__ == "__main__":
    pending = find_pending_videos()
    print(f"Found {len(pending)} videos to process")

    results = batch_process(pending, BATCH_SIZE)

    print("\n" + "="*60)
    print("BATCH PROCESSING COMPLETE")
    print("="*60)
    print(f"Total videos: {len(results)}")
    print(f"Successful: {sum(1 for r in results if r['status'] == 'success')}")
    print(f"Failed: {sum(1 for r in results if r['status'] == 'error')}")
```

## Optimizaciones de Rendimiento

### Hardware
- **CPU**: AMD Ryzen 7 5800X (8 cores) - suficiente para modelo `medium`
- **RAM**: Mínimo 16 GB recomendado
- **Disco**: SSD recomendado para I/O rápido
- **GPU**: Opcional - puede acelerar 2-3x con CUDA

### Software
```bash
# Activar WSL Ubuntu
wsl -d Ubuntu

# Entorno virtual
source .venv-wsl/bin/activate

# Verificar FFmpeg
ffmpeg -version

# Procesamiento nocturno
nohup python tools/batch_transcribe.py > transcription.log 2>&1 &
```

## Monitoreo y Logs

### Dashboard de Progreso
```json
{
  "session_start": "2025-11-04T10:00:00",
  "model": "medium",
  "total_videos": 17,
  "completed": 5,
  "remaining": 12,
  "estimated_completion": "2025-11-05T14:30:00",
  "current_video": {
    "name": "practica_06.mkv",
    "progress": "45%",
    "eta": "1.2h"
  }
}
```

### Alertas
- Email cuando complete batch
- Slack/Discord cuando termine todos
- Alerta si falla >3 videos consecutivos

## Checklist de Implementación

### Pre-Procesamiento
- [ ] Verificar espacio en disco (~200 GB para audio extraído)
- [ ] Confirmar dependencias: `whisper`, `ffmpeg-python`, `tqdm`
- [ ] Descargar modelo `medium` (cache ~/.cache/whisper/)
- [ ] Backup de configuración actual

### Procesamiento
- [ ] Ejecutar test con 1 video corto
- [ ] Validar calidad de output
- [ ] Lanzar Batch 1 (3 videos)
- [ ] Revisar métricas y ajustar si necesario
- [ ] Lanzar Batch 2-4 overnight

### Post-Procesamiento
- [ ] Verificar 4 formatos por video
- [ ] Validar términos técnicos
- [ ] Generar informe de calidad
- [ ] Commit a Git por lección

## Mejoras Futuras

### Corto Plazo
1. **VAD (Voice Activity Detection)**: Eliminar silencios largos
2. **Speaker Diarization**: Identificar instructor vs estudiantes
3. **Corrección automática**: "apolo" → "Apollo"

### Mediano Plazo
1. **Whisper fine-tuning**: Entrenar con vocabulario de trading
2. **Paralelización**: Procesar múltiples videos simultáneamente
3. **GPU acceleration**: Reducir tiempo 2-3x

## Conclusión

**Estrategia recomendada**:
1. ✅ Usar modelo `medium` (balance calidad/velocidad)
2. ✅ Procesamiento nocturno en 4 batches
3. ✅ Validación automática de calidad
4. ✅ Tiempo total: ~24-30 horas

**Próximo paso**: Crear y ejecutar `tools/batch_transcribe.py`

---
*Documento generado: 2025-11-04*
*Última actualización: 2025-11-04*
