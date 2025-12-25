# Документация: Пайплайн восстановления картины в ComfyUI

## Описание проекта

Данный проект представляет собой пайплайн для восстановления картины, найденной на изображении интерьера, с использованием diffusion моделей в ComfyUI. Пайплайн использует комбинацию различных техник conditioning для достижения высококачественного результата.

## Исходные данные

### Входное изображение
- **Файл**: `adm_dressing_1.-.jpg` - исходное изображение интерьера
- **Обработанное изображение**: `adm_dressing_1.- — копия.jpg` - обрезанная картина со стены

### Референсные изображения
- **Файл 1**: `unbekannter-maler-des-19-20-jahrhunderts-nachfolge-boucher-738299.jpg`
- **Файл 2**: `quadro-pittura-ad-olio-scena-galante-xix-secolo-a-guilleminquadri-6369468.jpg`

### Выбранный стиль
**Франсуа Буше (François Boucher)** - французский художник рококо XVIII века
- Характеристики стиля: пасторальные сцены, мифологические сюжеты, мягкие цвета, декоративность
- Период: XVIII век, эпоха рококо

## Структура пайплайна

Пайплайн состоит из 15 узлов, организованных в следующие этапы:

### 1. Входные данные

#### Узел 1: LoadImage (Исходная картина)
- **Тип**: `LoadImage`
- **Файл**: `adm_dressing_1.- — копия.jpg`
- **Назначение**: Загрузка обрезанного изображения картины со стены
- **Выходы**: IMAGE, MASK

#### Узел 3: LoadImage (Референс)
- **Тип**: `LoadImage`
- **Файл**: `unbekannter-maler-des-19-20-jahrhunderts-nachfolge-boucher-738299.jpg`
- **Назначение**: Загрузка референсного изображения для стилевой адаптации
- **Выходы**: IMAGE, MASK

### 2. Предобработка

#### Узел 2: CannyEdgePreprocessor
- **Тип**: `CannyEdgePreprocessor`
- **Параметры**:
  - `low_threshold`: 100
  - `high_threshold`: 200
- **Вход**: Изображение из узла 1
- **Выход**: IMAGE (Canny edges)
- **Назначение**: Извлечение границ изображения для сохранения структуры картины

### 3. Conditioning

#### Узел 4: CLIPTextEncode (Positive Prompt)
- **Тип**: `CLIPTextEncode`
- **Промпт**: 
  ```
  masterpiece, high quality, detailed painting in the style of François Boucher, 
  rococo art, 18th century French painting, pastoral scene, 
  soft colors, decorative elements, mythological or romantic scene, 
  restored painting, fine art, oil painting
  ```
- **Вход**: CLIP из узла 8
- **Выход**: CONDITIONING
- **Назначение**: Текстовое описание желаемого результата

#### Узел 5: CLIPTextEncode (Negative Prompt)
- **Тип**: `CLIPTextEncode`
- **Промпт**: 
  ```
  blurry, low quality, distorted, artifacts, watermark, signature, 
  modern style, photorealistic, 3d render
  ```
- **Вход**: CLIP из узла 8
- **Выход**: CONDITIONING
- **Назначение**: Описание того, чего нужно избегать

#### Узел 6: ControlNetApplyAdvanced
- **Тип**: `ControlNetApplyAdvanced`
- **Параметры**:
  - `strength`: 1.0 (полный контроль структуры)
  - `start_percent`: 0.0
  - `end_percent`: 1.0
- **Входы**:
  - `positive`: CONDITIONING из узла 4
  - `negative`: CONDITIONING из узла 5
  - `control_net`: CONTROL_NET из узла 9
  - `image`: IMAGE (Canny edges) из узла 2
- **Выходы**: CONDITIONING (positive, negative)
- **Назначение**: Применение ControlNet с Canny edges для контроля структуры изображения

#### Узел 7: IPAdapterAdvanced
- **Тип**: `IPAdapterAdvanced`
- **Параметры**:
  - `weight`: 0.7 (умеренное влияние стиля)
  - `weight_type`: "linear"
  - `combine_embeds`: "concat"
  - `start_at`: 0.0
  - `end_at`: 1.0
  - `embeds_scaling`: "V only"
- **Входы**:
  - `model`: MODEL из узла 8
  - `image`: IMAGE из узла 3 (референс)
- **Выход**: MODEL (с адаптацией стиля)
- **Назначение**: Адаптация стиля модели по референсному изображению

### 4. Модели

#### Узел 8: CheckpointLoaderSimple
- **Тип**: `CheckpointLoaderSimple`
- **Модель**: `sd_xl_base_1.0.safetensors` (SDXL base model)
- **Выходы**: MODEL, CLIP, VAE
- **Назначение**: Загрузка базовой diffusion модели

#### Узел 9: ControlNetLoader
- **Тип**: `ControlNetLoader`
- **Модель**: `control_v11p_sd15_canny.pth`
- **Выход**: CONTROL_NET
- **Назначение**: Загрузка модели ControlNet для работы с Canny edges

#### Узел 14: UpscaleModelLoader
- **Тип**: `UpscaleModelLoader`
- **Модель**: `4x_ESRGAN.pth`
- **Выход**: UPSCALE_MODEL
- **Назначение**: Загрузка модели для увеличения разрешения

### 5. Генерация (Image-to-Image)

#### Узел 12: VAEEncode
- **Тип**: `VAEEncode`
- **Входы**:
  - `pixels`: IMAGE из узла 1 (исходная картина)
  - `vae`: VAE из узла 8
- **Выход**: LATENT
- **Назначение**: Кодирование исходного изображения в latent space для image-to-image генерации

#### Узел 10: KSampler
- **Тип**: `KSampler`
- **Параметры**:
  - `seed`: 12345
  - `steps`: 20
  - `cfg`: 7.0 (CFG Scale)
  - `sampler_name`: "euler"
  - `scheduler`: "normal"
  - `denoise`: 0.7 (уровень деноизинга для image-to-image)
- **Входы**:
  - `model`: MODEL из узла 7 (с IP-Adapter)
  - `positive`: CONDITIONING из узла 6
  - `negative`: CONDITIONING из узла 6
  - `latent_image`: LATENT из узла 12
- **Выход**: LATENT
- **Назначение**: Image-to-image генерация с использованием всех видов conditioning

#### Узел 11: VAEDecode
- **Тип**: `VAEDecode`
- **Входы**:
  - `samples`: LATENT из узла 10
  - `vae`: VAE из узла 8
- **Выход**: IMAGE
- **Назначение**: Декодирование из latent space в изображение

### 6. Постобработка

#### Узел 13: ImageUpscaleWithModel
- **Тип**: `ImageUpscaleWithModel`
- **Входы**:
  - `upscale_model`: UPSCALE_MODEL из узла 14
  - `image`: IMAGE из узла 11
- **Выход**: IMAGE (увеличенное до 2k+)
- **Назначение**: Увеличение разрешения изображения в 4 раза (до 2k+)

#### Узел 15: SaveImage
- **Тип**: `SaveImage`
- **Параметры**:
  - `filename_prefix`: "restored_painting"
- **Вход**: IMAGE из узла 13
- **Назначение**: Сохранение финального восстановленного изображения

## Последовательность выполнения

1. **Загрузка данных**: Загружаются исходная картина и референсное изображение
2. **Предобработка**: Извлекаются Canny edges из исходного изображения
3. **Текстовое conditioning**: Кодируются positive и negative промпты через CLIP
4. **ControlNet conditioning**: Применяется ControlNet с Canny edges для контроля структуры
5. **Стилевое conditioning**: Применяется IP-Adapter для адаптации стиля по референсу
6. **Кодирование**: Исходное изображение кодируется в latent space
7. **Генерация**: Выполняется image-to-image генерация через KSampler
8. **Декодирование**: Результат декодируется из latent space
9. **Увеличение разрешения**: Изображение увеличивается до 2k+ через ESRGAN
10. **Сохранение**: Финальное изображение сохраняется

## Компоненты пайплайна

### ✓ Image-to-Image
Реализовано через:
- VAEEncode (кодирование исходного изображения)
- KSampler с параметром `denoise: 0.7`
- Использование исходного изображения как начальной точки генерации

### ✓ Text Conditioning
Реализовано через:
- CLIPTextEncode для positive и negative промптов
- Интеграция через ControlNetApplyAdvanced

### ✓ Image Conditioning
Реализовано через:
- IP-Adapter с референсным изображением
- Weight: 0.7 для умеренного влияния стиля

### ✓ Structure Preservation
Реализовано через:
- CannyEdgePreprocessor для извлечения границ
- ControlNet с Canny edges (strength: 1.0)
- Сохранение оригинальной структуры картины

### ✓ Upscale до 2k+
Реализовано через:
- ESRGAN 4x модель
- ImageUpscaleWithModel для финального увеличения разрешения

## Требования

### Необходимые модели

1. **Базовая diffusion модель**:
   - `sd_xl_base_1.0.safetensors` (SDXL) или аналогичная SD 1.5 модель
   - Размещение: `ComfyUI/models/checkpoints/`

2. **ControlNet модель**:
   - `control_v11p_sd15_canny.pth`
   - Размещение: `ComfyUI/models/controlnet/`

3. **IP-Adapter** (если требуется):
   - Модели IP-Adapter для выбранной базовой модели
   - Размещение: `ComfyUI/models/ipadapter/`

4. **Upscale модель**:
   - `4x_ESRGAN.pth` или аналогичная
   - Размещение: `ComfyUI/models/upscale_models/`

### Необходимые расширения ComfyUI

- ControlNet (обычно встроен)
- IP-Adapter (может потребоваться установка)
- Custom Nodes для расширенного функционала (опционально)

## Инструкция по использованию

### 1. Подготовка

1. Убедитесь, что ComfyUI установлен и запущен
2. Поместите необходимые модели в соответствующие директории
3. Убедитесь, что входные изображения доступны:
   - `adm_dressing_1.- — копия.jpg`
   - `unbekannter-maler-des-19-20-jahrhunderts-nachfolge-boucher-738299.jpg`

### 2. Загрузка пайплайна

1. Откройте ComfyUI
2. Перейдите в меню: **File → Load**
3. Выберите файл `painting_restoration_pipeline.json`
4. Пайплайн загрузится со всеми узлами и связями

### 3. Настройка

1. **Проверьте пути к изображениям**:
   - Узел 1: путь к обрезанной картине
   - Узел 3: путь к референсному изображению

2. **Проверьте названия моделей**:
   - Узел 8: название checkpoint модели
   - Узел 9: название ControlNet модели
   - Узел 14: название upscale модели

3. **Настройте параметры** (опционально):
   - Узел 10 (KSampler): steps, cfg, denoise
   - Узел 6 (ControlNet): strength
   - Узел 7 (IP-Adapter): weight

### 4. Запуск

1. Нажмите кнопку **Queue Prompt** в ComfyUI
2. Дождитесь завершения генерации
3. Результат будет сохранен в `ComfyUI/output/` с префиксом `restored_painting`

## Параметры для настройки

### KSampler (Узел 10)
- **steps**: Количество шагов генерации (рекомендуется: 20-30)
- **cfg**: CFG Scale (рекомендуется: 7.0-9.0)
- **denoise**: Уровень деноизинга для image-to-image (0.0-1.0)
  - 0.0 = минимальные изменения
  - 1.0 = полная генерация
  - 0.7 = умеренные изменения (рекомендуется)

### ControlNet (Узел 6)
- **strength**: Сила влияния ControlNet (0.0-1.0)
  - 1.0 = полный контроль структуры
  - 0.5-0.8 = умеренный контроль

### IP-Adapter (Узел 7)
- **weight**: Сила влияния стиля (0.0-1.0)
  - 0.7 = умеренное влияние (рекомендуется)
  - 1.0 = сильное влияние стиля

### Canny Edge (Узел 2)
- **low_threshold**: Нижний порог (рекомендуется: 100)
- **high_threshold**: Верхний порог (рекомендуется: 200)
- Настройка влияет на детализацию границ

## Возможные улучшения

1. **Добавление дополнительных референсов**: Можно использовать несколько референсных изображений
2. **Использование других ControlNet**: Depth, OpenPose, и т.д.
3. **Многоэтапный upscale**: Использование нескольких этапов увеличения разрешения
4. **Post-processing**: Добавление цветокоррекции, шумоподавления
5. **Вариации стиля**: Эксперименты с разными весами IP-Adapter

## Заключение

Пайплайн обеспечивает комплексный подход к восстановлению картины, сочетая:
- Сохранение структуры через ControlNet
- Адаптацию стиля через IP-Adapter
- Текстовое описание через CLIP
- Image-to-image генерацию для сохранения исходной композиции
- Высокое разрешение через upscale

Результат - высококачественное восстановленное изображение картины в стиле выбранного художника с сохранением оригинальной структуры.

