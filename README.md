# 🧬 AgingBioHypotheses

**Научно строгий генератор гипотез для исследований биологии старения**

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Research](https://img.shields.io/badge/status-research-orange.svg)]()

## 📖 Описание

AgingBioHypotheses — это интеллектуальная система для генерации научно обоснованных гипотез в области биологии старения. Система использует анализ больших сетей биологических терминов и современные языковые модели для создания механистических гипотез с конкретными экспериментальными протоколами.

### 🎯 Основные возможности

- **🔬 Научно строгий анализ**: 5-компонентная система оценки качества терминов
- **📊 Структурный анализ сетей**: Детальная характеристика подграфов (плотность, центральность, hubs)
- **💡 Умная генерация гипотез**: Множественные раунды с вариацией фокуса исследования
- **🎯 Категоризация терминов**: 8 биологических категорий с автоматической классификацией
- **🚫 Контроль качества**: Автоматическое обнаружение дублирования гипотез (TF-IDF)
- **📚 Управление историей**: Отслеживание всех сгенерированных гипотез
- **🧪 Количественные эксперименты**: Конкретные протоколы с размерами выборок и статистикой

## 🚀 Быстрый старт

### Установка

```bash
# Клонирование репозитория
git clone https://github.com/yourusername/AgingBioHypotheses.git
cd AgingBioHypotheses

# Установка зависимостей
pip install -r requirements.txt

# Установка дополнительных библиотек для NLP
pip install scispacy
pip install https://s3-us-west-2.amazonaws.com/ai2-s2-scispacy/releases/v0.5.1/en_ner_bionlp13cg_md-0.5.1.tar.gz
```

### Требования

```python
networkx>=2.8
pandas>=1.5.0
numpy>=1.21.0
scikit-learn>=1.1.0
matplotlib>=3.5.0
spacy>=3.4.0
scispacy>=0.5.0
openai>=1.0.0
```

### Настройка данных

```python
import pandas as pd
from src.graph_builder import build_graph_with_metadata

# Загрузка данных (Excel файл с предложениями и метаданными)
df = pd.read_excel('data/merged_sentences_with_meta.xlsx')

# Построение графа биологических терминов
G, node_full_meta, edge_full_meta = build_graph_with_metadata(df)

print(f"Создан граф: {G.number_of_nodes():,} узлов, {G.number_of_edges():,} рёбер")
```

### Быстрый запуск

```python
from src.hypothesis_generator import launch_complete_system

# Запуск полной интерактивной системы
launch_complete_system()
```

## 📋 Примеры использования

### 1. Интерактивный режим с выбором терминов

```python
from src.hypothesis_generator import enhanced_interactive_mode

enhanced_interactive_mode()
```

```
🎯 КАТЕГОРИИ ТЕРМИНОВ ДЛЯ АНАЛИЗА
================================================================

1. 🧬 Процессы старения (15 терминов)
   Топ-5: aging, senescence, longevity, mortality, lifespan

2. 🔬 Молекулярные механизмы (12 терминов)  
   Топ-5: autophagy, apoptosis, dna repair, transcription

👆 Выберите категорию (1-8), 0 для ручного ввода, 99 для случайного:
```

### 2. Научно строгий анализ одного термина

```python
from src.hypothesis_generator import ScientificHypothesisGenerator
from openai import OpenAI

# Настройка
client = OpenAI(base_url="your-llm-endpoint", api_key="your-key")
generator = ScientificHypothesisGenerator(G, node_full_meta, edge_full_meta, client)

# Анализ термина
result = generator.analyze_term_scientific("autophagy", radius=3)
```

**Пример вывода:**
```
📊 СТРУКТУРНЫЙ АНАЛИЗ ПОДГРАФА:
  📈 Базовые метрики:
    • Узлов: 47
    • Рёбер: 156
    • Плотность: 0.145
  🌟 Центральные узлы (hubs):
    1. autophagy (степень: 18, центральность: 0.156)

📊 ОЦЕНКА КАЧЕСТВА (0-5 по каждому критерию):
  • Новизна: 4.2/5
  • Научная обоснованность: 4.7/5
  • Тестируемость: 3.8/5
  🎯 ОБЩИЙ БАЛЛ: 4.2/5.0
```

### 3. Генерация множественных раундов гипотез

```python
from src.hypothesis_generator import MultiHypothesisGenerator

multi_gen = MultiHypothesisGenerator(generator)

# Раунд 1: Общий анализ
result1 = generator.analyze_term_scientific("senescence")

# Раунд 2: Фокус на молекулярные механизмы  
result2 = multi_gen.generate_additional_hypotheses("senescence", 
                                                   result1['quality_terms'], 
                                                   round_number=2)

# Раунд 3: Фокус на клеточные процессы
result3 = multi_gen.generate_additional_hypotheses("senescence", 
                                                   result1['quality_terms'], 
                                                   round_number=3)

# Показать все гипотезы
multi_gen.show_all_hypotheses("senescence")
```

### 4. Пакетный анализ

```python
from src.hypothesis_generator import batch_scientific_analysis

# Анализ списка терминов
terms = ["aging", "autophagy", "senescence", "inflammation", "mitochondria"]
results = batch_scientific_analysis(terms)

# Сохранение результатов
with open('results/batch_results.json', 'w') as f:
    json.dump(results, f, indent=2)
```

## 🏗️ Архитектура проекта

```
AgingBioHypotheses/
├── src/
│   ├── graph_builder.py          # Построение графа из данных
│   ├── hypothesis_generator.py   # Основная система генерации
│   ├── term_selector.py          # Категоризация и выбор терминов
│   ├── quality_metrics.py        # Метрики качества терминов
│   └── utils.py                  # Вспомогательные функции
├── data/
│   ├── merged_sentences_with_meta.xlsx  # Исходные данные
│   └── sample_data.csv          # Пример данных
├── results/                     # Результаты анализа
├── notebooks/
│   ├── demo.ipynb              # Демонстрация возможностей
│   └── analysis_examples.ipynb # Примеры анализа
├── tests/                      # Тесты
├── requirements.txt
├── setup.py
└── README.md
```

## 🔬 Научная методология

### Метрики качества терминов (0-10 баллов)

1. **Degree Centrality** (0-2): Количество связей в сети
2. **PageRank** (0-2): Важность в структуре графа  
3. **Betweenness Centrality** (0-2): Роль в соединении компонентов
4. **Биологическая релевантность** (0-3): Соответствие категориям
5. **Качество метаданных** (0-1): Наличие DOI, авторов, годов

### Оценка качества гипотез (0-5 по критерию)

- **Новизна**: Избегание банальных связей
- **Научная обоснованность**: Глубина биологического обоснования
- **Тестируемость**: Конкретность экспериментальных протоколов
- **Разнообразие**: Уникальность используемых терминов
- **Детальность**: Полнота описания механизмов

### Контроль дублирования

- **TF-IDF векторизация** гипотез
- **Косинусное сходство** для обнаружения похожих гипотез (>50%)
- **Автоматические предупреждения** о дублировании

## 📊 Примеры результатов

### Качественные термины для "autophagy"

| Термин | Качество | Степень | PageRank | Биол. рел. |
|--------|----------|---------|-----------|------------|
| autophagy | 8.2 | 18 | 0.0156 | 3.0 |
| senescence | 7.8 | 15 | 0.0134 | 3.0 |
| mitochondria | 7.1 | 12 | 0.0121 | 2.5 |

### Пример сгенерированной гипотезы

```
🧬 Термины: autophagy, circadian rhythm, proteostasis

🔬 Механизм: Нарушение циркадной регуляции аутофагии в нейронах 
приводит к накоплению агрегированных белков и ускоренному 
когнитивному старению через дисфункцию протеостаза.

📚 Обоснование: Аутофагия имеет циркадную ритмичность, и её 
нарушение может критически влиять на клиренс белковых агрегатов 
в долгоживущих нейронах.

🧪 Эксперимент: Рандомизированное исследование n=80 мышей 
(40 контроль, 40 Bmal1-/-). Измерение LC3-II/LC3-I ratio 
методом Western blot каждые 4 часа в течение 48 часов. 
Ожидаемое снижение циркадной амплитуды на 60-80% 
(p<0.001, двухфакторный ANOVA).
```

## 🤝 Вклад в проект

Мы приветствуем вклад в развитие проекта! 

### Как внести вклад

1. Форкните репозиторий
2. Создайте ветку для новой функции (`git checkout -b feature/amazing-feature`)
3. Внесите изменения и добавьте тесты
4. Сделайте коммит (`git commit -m 'Add amazing feature'`)
5. Отправьте в ветку (`git push origin feature/amazing-feature`)
6. Создайте Pull Request

### Области для улучшения

- 🔬 **Дополнительные метрики**: Новые способы оценки качества терминов
- 🧠 **LLM интеграции**: Поддержка других языковых моделей
- 📊 **Визуализация**: Интерактивные графики сетей и результатов
- 🗃️ **Базы данных**: Интеграция с биологическими базами данных
- 🔄 **Валидация**: Автоматическая проверка гипотез по литературе

## 📄 Лицензия

Этот проект распространяется под лицензией MIT. См. файл [LICENSE](LICENSE) для подробностей.
 
 

## 📚 Цитирование

Если вы используете этот проект в своих исследованиях, пожалуйста, цитируйте:

```bibtex
@software{agingbiohypotheses2025,
  title={AgingBioHypotheses: Scientific Hypothesis Generator for Aging Biology},
  author={Margarita Soloshenko},
  year={2025},
  url={https://github.com/MargoSolo/AgingBioHypotheses}
}
```

---

⭐ **Понравился проект? Поставьте звезду!** ⭐
