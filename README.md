# ДЗ 5: CTE та Materialized View на даних Olist

У роботі використано [Olist Brazilian E-Commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce): таблиці клієнтів, замовлень, позицій, товарів і продавців. Дані завантажуються повністю, без вибірки.

## Дані та запуск

CSV-файли **не зберігаються в репозиторії**. Notebook сам шукає їх у `data/olist/`, а якщо папка порожня — завантажує датасет через KaggleHub або з [репозиторію Olist](https://github.com/olist/work-at-olist-data/tree/master/datasets). Якщо жодне джерело недоступне, використовується навчальна stub-схема з умови завдання.

Відкрийте `hw5_olist_features.ipynb` у Google Colab і запустіть **Runtime → Restart session and run all**. Вручну налаштовувати шляхи або таблиці не потрібно.

## Що зроблено

- CSV завантажуються в raw-таблиці, після чого дані переносяться в typed-схему PostgreSQL з ключами та обмеженнями.
- CTE-ланцюжок обчислює RFM, квартал першої покупки, середній чек за 30 днів, частку скасувань і найпопулярнішу категорію. Ознаки рахуються за `customer_unique_id`; фінальний результат має один рядок на `customer_id`.
- Рекурсивний CTE проходить синтетичне дерево запрошень і демонструє захист від циклу.
- Повний результат зберігається в Materialized View з унікальним індексом; показано `REFRESH MATERIALIZED VIEW CONCURRENTLY` і порівняно плани повного обчислення та читання з MV через `EXPLAIN (ANALYZE, BUFFERS)`.

Notebook містить код, результати виконання та висновки до кожного етапу.
