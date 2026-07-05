# Summer Practice — Semantic Search Comparison

Проект содержит материалы учебной практики по теме «Навигатор по смыслу»: сравнение embedding-моделей для семантического поиска по фрагментам кода.

Работа приведена к структуре задания: исследование состоит из **трёх шагов**, и каждый шаг оформлен отдельным разделом в Jupyter Notebook `Jupyter/semantic_search_comparison.ipynb`.

## Что сделано

В проекте выполнено сравнение трёх мультиязычных моделей:

* `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`;
* `sentence-transformers/paraphrase-multilingual-mpnet-base-v2`;
* `sentence-transformers/distiluse-base-multilingual-cased-v2`.

В задании приведён пример с 100 фрагментами кода и 15 вопросами, однако фактически предоставленный датасет `dataset_v1.0` содержит **200 фрагментов кода** и **25 тестовых вопросов** с известными правильными ответами. Поэтому эксперимент выполнен на полном наборе данных: это расширяет проверку моделей и не исключает правильные ответы из поиска.

## Структура проекта

```text
summer_practice/
├── README.md
└── Shaposhnikov_AA_BVT2503_practice/
    ├── Пояснительная_записка_по_практике.docx
    ├── Дневник_по_практике.docx
    ├── Sources/
    │   ├── dataset_v1.0/
    │   │   ├── README.md
    │   │   ├── code_corpus.json
    │   │   ├── eval_questions.json
    │   │   └── categories.json
    │   └── Задание на практику БВТ25.pdf
    └── Jupyter/
        ├── semantic_search_comparison.ipynb
        ├── requirements.txt
        └── results/
            ├── embeddings_MiniLM_multilingual.npy
            ├── embeddings_MPNet_multilingual.npy
            ├── embeddings_DistilUSE_multilingual.npy
            ├── model_comparison.csv
            ├── language_comparison.csv
            ├── error_analysis.csv
            ├── error_type_summary.csv
            ├── search_results.csv
            ├── tsne_best_model.png
            └── final_conclusion.txt
```

## Три шага исследования в ноутбуке

1. **Шаг 1. Загрузка данных и моделей** — загружается датасет, проверяется наличие 200 фрагментов кода и 25 вопросов, задаются три embedding-модели.
2. **Шаг 2. Генерация эмбеддингов и поиск** — для каждой модели строятся или загружаются эмбеддинги, выполняется поиск top-3 по косинусному сходству и сохраняются результаты.
3. **Шаг 3. Метрика и визуализация** — рассчитывается Precision@3, строятся таблицы, выполняются анализ ошибок, сравнение русского/английского поиска, t-SNE-график и финальный вывод.

## Результаты

| Модель | Precision@3 | Попаданий | Время, сек. |
|---|---:|---:|---:|
| MPNet multilingual | 0,920 | 23 из 25 | 20,33 |
| DistilUSE multilingual | 0,920 | 23 из 25 | 28,47 |
| MiniLM multilingual | 0,880 | 22 из 25 | 12,93 |

Основной моделью выбрана **MPNet multilingual**, потому что при одинаковом Precision@3 с DistilUSE она быстрее по времени расчёта в сохранённом эксперименте. MiniLM используется как быстрый baseline, а DistilUSE добавлена как третья модель для расширенного сравнения мультиязычного поиска.

## Запуск

Из папки `Jupyter/`:

```bash
pip install -r requirements.txt
jupyter notebook semantic_search_comparison.ipynb
```

Или из корня проекта:

```bash
pip install -r Shaposhnikov_AA_BVT2503_practice/Jupyter/requirements.txt
jupyter notebook Shaposhnikov_AA_BVT2503_practice/Jupyter/semantic_search_comparison.ipynb
```

Ноутбук настроен так, чтобы находить датасет как при запуске из папки `Jupyter/`, так и при запуске из корня проекта. В папке `Jupyter/results/` уже лежат сохранённые результаты эксперимента: таблица сравнения моделей, подробная top-3 выдача, анализ ошибок, сравнение языков, t-SNE-график и финальный вывод.
