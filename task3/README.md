## Задание 3. Соревнование по детекции текста на датасете Coco-TEXT

### Описание
В этом задании вам надо натренировать детектор текста на основе датасета [Coco-Text](https://bgshih.github.io/cocotext/). Можно использовать (почти) что угодно - оценка основана прежде всего на метрике вашей модели.

### Результаты экспериментов (Отчет)

По резульататам проведенных экспериментов с сетью RetinaNet, лучшее значение метрики было получено для модели RetinaNet, в качестве backbone сети у которой была использована архитектура EfficientNet-b2. 
После 15 эпох обучения с batch_size=4, начальным lr=0.0005 и последующим домножением его каждую эпоху на 0.5 значение AP@50 составило 41.38. Код для воспроизведения результатов а также логи тренировок представлены в Jupyter Notebook my_train.ipynb.

Также были проведены эксперименты с архитектурами backbone моделей EfficientNet-b3 и EfficientNet-b4, за 15 эпох обучения не удалось достичь сходимости для моделей, поэтому AP@50 составило только 36 и 39 AP@50 соответственно.


### Метрика и сабмит
Используется модифицированная метрика AveragePrecision (AP@IoU=0.5, PASCAL VOC AP) - отличие в том, что объекты GT, доля площади которых на изображении меньше (1/32)^2 выбрасываются из разметки.
Average в AP делается по confidence от 0 до 1 с шагом 0.1.

Метрика вычисляется по файлу predictions.json, который лежит рядом с README. Чтобы сохранить и оценить ваше решение - вам нужно обновить этот файл и сделать коммит.

Файл predictions.json вы должны создать сами с использованием вашей обученной модели. Проще всего это сделать, используя функцию `a4_course_cvdl_t3.utils.dump_detections_to_cocotext_json`. Формат файла тот же, который используют авторы coco-text в своем [репозитории](https://github.com/andreasveit/coco-text).

Метрику AP вы можете вычислить, используя функцию  `a4_course_cvdl_t3.utils.evaluate_ap_from_cocotext_json`. Метрика также вычисляется автоматически через github-action (раздел `Task3 AP` -> `Compute Average Precision`).

Для использования `a4_course_cvdl_t3` надо установить пакет: `pip install -e task3/`

### Пример baseline
В ноутбуках baseline_train.ipynb и baseline_eval.ipynb есть пример тренировки детектора, создания файла и вычисления AP.
Также там есть пример работы с CocoText, визуализациями аннотаций и.т.д.

### Условия и требования
Вы можете использовать любой torch код для этого соревнования, включая претренированные модели.
Вы можете использовать дополнительные данные, если хотите.

Следующие требования являются обязательными, чтобы зачесть задание:
1. **Вы не должны использовать Val данные Coco-Text в обучении. В обучении должны участвовать только Train данные.** В соревновании нет приватного теста, т.к. используется публично доступный датасет.
1. Ваше решение должно быть натренировано (затюнено) вами на Coco-Text.
1. Нельзя использовать модели, которые были предобучены уже на Coco-Text (если такие найдутся где-то).
1. Код для воспроизведения файла-сабмита predictions.json должен находиться в вашем форке в task3
1. Ваш AP должен быть выше AP baseline, который уже лежит в репозитории.

### Баллы и сроки
Учитывается две составляющих работы: набранная метрика (max 15 баллов) и оформление результатов (max 5 баллов).
Также в обоих пунктах можно набрать бонусные баллы.

**Баллы за набранную метрику (15)**
1. Выполнить все пункты `условий и требований`, в том числе превзойти baseline: +5 баллов
1. Набрать `AP` > 0.20: +3 балла
1. Набрать `AP` > 0.25: +3 балла
1. Набрать `AP` > 0.30: +3 балла
1. Набрать Максимальный балл среди группы на момент 14.11.2023-23:59:59: +1 балл

Бонусные баллы за метрику:
1. За каждые 0.03 AP после 0.3 (т.е. начиная с 0.33): 1 бонусный балл

**Баллы за оформление (5)**
Ваши результаты должны быть описаны в отдельном файле-отчёте `REPORT.md` (либо `REPORT.pdf`) рядом с этим README.

1. Описать в `REPORT` в разделе `Лучшая модель` вашу модель (какие статьи, какая архитектура, какой бэкбон, откуда взят код, какой лосс, параметры тренироки), в чём отличия от бэйзлайна и привести логи обучения для train и val (можно ссылкой на wandb или на какой-то ноутбук): +1 балл
1. Описать в `REPORT` в разделе `Воспроизведение` полную работчую инструкцию для воспроизведения вашей лучшей модели: +1 балл
1. Описать в `REPORT` в разделе `Эксперименты` минимум 2 эксперимента (помимо лучшего), которые вы провели (архитектуры, ауги, лоссы, постпроцессинги - что угодно), с указанием набранных метрик для каждого эксперимента(можно ссылкой на wandb): +2 балла
1. Описать в `REPORT` в разделе `Ошибки` проведенный анализ ошибок - на каких изображениях модель ошибается, с чем это может быть связано: +1 балл

Бонусный балл за оформление:
1. Оформить свой код в python-пакете, а не в одном огромном jupyter ноутбуке: 1 бонусный балл
**Бонусные баллы за AP начисляются до 12.12 без штрафов.**

## Сроки сдачи
В срок [24.10 – 14.11] `максимум баллов за задание` = 20. Начиная с 14.11, `максимум баллов за задание` = 10. Начиная с 12.12 (жесткий дедлайн), `максимум баллов за задание` = 0.

**Бонусные баллы за AP начисляются до 12.12 без штрафов.**

### Лидерборд
При загрузке нового `predictions.json`, ваш AP\*100 будет обновлен на лидерборде [https://keepthescore.com/board/xcmgfzogolr/](https://keepthescore.com/board/xcmgfzogolr/).
Лидерборд не несет какой-то важной функции и добавлен просто для интереса.
