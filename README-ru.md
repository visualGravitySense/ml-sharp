# Резкий синтез монокулярных ракурсов менее чем за одну секунду

[![Страница проекта](https://img.shields.io/badge/Project-Page-green)](https://apple.github.io/ml-sharp/)
[![arXiv](https://img.shields.io/badge/arXiv-2512.10685-b31b1b.svg)](https://arxiv.org/abs/2512.10685)

Данный программный проект сопровождает научную статью:
*«Sharp Monocular View Synthesis in Less Than a Second»*

Авторы: *Lars Mescheder, Wei Dong, Shiwei Li, Xuyang Bai, Marcel Santos, Peiyun Hu, Bruno Lecouat, Mingmin Zhen, Amaël Delaunoy,
Tian Fang, Yanghai Tsin, Stephan Richter и Vladlen Koltun*.

![](data/teaser.jpg)

Мы представляем **SHARP** — метод фотореалистичного синтеза ракурсов по одному изображению. Имея всего одну фотографию, SHARP регрессирует параметры 3D-гауссового представления изображённой сцены. Это выполняется менее чем за одну секунду на стандартном GPU за один прямой проход через нейронную сеть. Полученное 3D-гауссово представление затем может быть визуализировано в реальном времени, обеспечивая высококачественные фотореалистичные изображения для близких ракурсов.

Представление является метрическим, с абсолютным масштабом, что поддерживает метрические перемещения камеры. Экспериментальные результаты показывают, что SHARP демонстрирует устойчивую zero-shot обобщающую способность на разных датасетах. Метод устанавливает новое состояние искусства (state of the art) на нескольких наборах данных, снижая LPIPS на 25–34% и DISTS на 21–43% по сравнению с лучшей предыдущей моделью, одновременно уменьшая время синтеза на три порядка.

## Начало работы

Рекомендуется сначала создать Python-окружение:

```
conda create -n sharp python=3.13
```

После этого установите проект с помощью:

```
pip install -r requirements.txt
```

Чтобы проверить корректность установки, выполните:

```
sharp --help
```

## Использование CLI

Для запуска предсказания:

```
sharp predict -i /path/to/input/images -o /path/to/output/gaussians
```

Контрольная точка модели будет автоматически загружена при первом запуске и сохранена локально в `~/.cache/torch/hub/checkpoints/`.

Также вы можете загрузить модель вручную:

```
wget https://ml-site.cdn-apple.com/models/sharp/sharp_2572gikvuh.pt
```

Чтобы использовать загруженную вручную модель, укажите её с помощью флага `-c`:

```
sharp predict -i /path/to/input/images -o /path/to/output/gaussians -c sharp_2572gikvuh.pt
```

Результатом будут 3D-гауссовы сплаты (3DGS) в выходной папке. Файлы 3DGS формата `.ply` совместимы с различными публичными рендерерами 3DGS. Мы используем систему координат OpenCV (x — вправо, y — вниз, z — вперёд). Центр сцены 3DGS приблизительно находится в точке (0, 0, +z). При использовании сторонних рендереров, пожалуйста, масштабируйте и поворачивайте сцену для корректного центрирования.

### Рендеринг траекторий (только CUDA GPU)

Дополнительно вы можете рендерить видео с траекторией камеры. Хотя предсказание гауссиан работает на CPU, CUDA и MPS, рендеринг видео с использованием опции `--render` в настоящее время требует GPU с поддержкой CUDA. Рендерер gsplat может инициализироваться некоторое время при первом запуске.

```
sharp predict -i /path/to/input/images -o /path/to/output/gaussians --render

# Или из уже предсказанных гауссиан:
sharp render -i /path/to/output/gaussians -o /path/to/output/renderings
```

## Оценка

Для количественной и качественной оценки, пожалуйста, обратитесь к статье.
Также рекомендуем посмотреть эту [страницу с качественными примерами](https://apple.github.io/ml-sharp/), содержащую видеосравнения с близкими по тематике работами.

## Цитирование

Если вы используете нашу работу, пожалуйста, процитируйте следующую статью:

```bibtex
@inproceedings{Sharp2025:arxiv,
  title      = {Sharp Monocular View Synthesis in Less Than a Second},
  author     = {Lars Mescheder and Wei Dong and Shiwei Li and Xuyang Bai and Marcel Santos and Peiyun Hu and Bruno Lecouat and Mingmin Zhen and Ama\"{e}l Delaunoyand Tian Fang and Yanghai Tsin and Stephan R. Richter and Vladlen Koltun},
  journal    = {arXiv preprint arXiv:2512.10685},
  year       = {2025},
  url        = {https://arxiv.org/abs/2512.10685},
}
```

## Благодарности

Кодовая база построена с использованием множества open-source проектов. Подробности см. в файле [ACKNOWLEDGEMENTS](ACKNOWLEDGEMENTS).

## Лицензия

Перед использованием кода ознакомьтесь с файлом [LICENSE](LICENSE),
а для выпущенных моделей — с [LICENSE_MODEL](LICENSE_MODEL).
