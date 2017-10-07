**Обучение модели**

1. Скачайте файл с векторным представлением слов, готовые модели вы можете найти на сайте http://rusvectores.org/ru/models/ или выполните `scripts/reader/download_w2v.sh`

2. Трансформируйте файл с представлениями слов в текстовый формат: для этого выполните все ячейке в ноутбуке
   [scripts/reader/BinaryW2VToSpaceSepartor.ipynb](`scripts/reader/BinaryW2VToSpaceSepartor.ipynb`)

3. Конвертируйте данные в формат, подходящий для обучения: `PYTHONPATH=.:$PYTHONPATH python3 scripts/reader/preprocess.py --tokenizer SimpleTokenizer train.csv data/datasets/output_filename.json`

4. Разделите файл на обучающую выборку и валидационную

5. В [scripts/reader/train.sh](`scripts/reader/train.sh`) вы можете найти пример запуска обучения модели

6. После обучения можете делать сабмит: [scripts/reader/train.sh](`sh create_zip.sh`) положит все необходимые файлы (убедитесь, что среди них есть модель, если вы переименовали модель незабудьте )

7. Также вы можете запустить сессию в интерактивном режиме `PYTHONPATH=.:$PYTHONPATH python3 scripts/reader/interactive.py --model models/20171007-1ce20c3f.mdl`

**Параметры обученной модели**

Текст разбивается на токены с помощью простейшего регулярного выражения (см `drqa/reader/simple_tokenizer.py`)
Все слова приводятся к леммам с помощью `pymorphy2`, переводятся в lowercase и кодируются соответствующими `word2vec`-представлениями. Информация о частях речи, именованных сущностях и т.д. не используется. 

Слова, которых нет в предобученном `word2vec` игнорируются.

В качестве валидационной метрики используется `exact_match` - число полностью верных ответов на вопросы.
