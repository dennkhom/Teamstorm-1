# Прогресс выполнения задачи

В TeamStorm встроено решение по постановке и мониторингу целей и ключевых результатов (методология OKR — Objectives and Key Results).

В качестве основного инструмента методологии используется расчет прогресса задачи.

TeamStorm позволяет рассчитывать прогресс выполнения задачи тремя способами:

* по статусу;
* по прогрессу подзадач;
* по метрике.

Отслеживать прогресс выполнения задачи можно в карточке задачи или в столбце **Прогресс** в [представлении задач](https://docs.teamstorm.io/rukovodstva/rukovodstvo-polzovatelya-teamstorm/rabota-s-zadachami/predstavlenie-zadach/otslezhivanie-progressa-v-predstavlenii) «Таблица».&#x20;

## Расчет прогресса по статусу

Прогресс выполнения задачи по статусу содержит только два значения:&#x20;

* 0% — для любых категорий статусов кроме **Выполнено**;
* 100% — для категории статусов **Выполнено.**

Расчет прогресса по статусу включается по умолчанию при активации расчета прогресса в карточке задачи.

## Расчет прогресса по прогрессу подзадач

При выборе механизма расчета по прогрессу подзадач прогресс родительской задачи равен среднему арифметическому прогрессов подзадач.

Дочерние задачи в статусе категории **Отменено** не учитываются при расчете прогресса родительской задачи, при этом прогресс родительской задачи, переведенной в статус категории **Отменено**, сохраняется.

Если у задачи нет подзадач, то прогресс устанавливается со значением 0%.

## Расчет прогресса по метрике

Расчет прогресса по метрике подразумевает, что пользователь задает стартовое и целевое значение, а затем в ходе работы над задачей редактирует текущее значение.&#x20;

При изменении текущего значения система пересчитывает прогресс по формуле:

&#x20;_Цель - Старт / 100% = Текущее значение / Прогресс, %._

## Включение и настройка расчета прогресса в карточке задачи

Для включения расчета прогресса:

1. Перейти в карточку задачи, для которой требуется включить расчет прогресса.
2. Нажать кнопку <img src="../../../.gitbook/assets/изображение (198).png" alt="" data-size="line"> в правом верхнем углу карточки и выбрат**ь Включить расчет прогресса**.&#x20;

В карточке задачи отобразится виджет с отображением прогресса ![](<../../../.gitbook/assets/изображение (199).png>)

Для настройки расчета прогресса:

1. Нажмите кнопку настроек <img src="../../../.gitbook/assets/изображение (200).png" alt="" data-size="line">на виджете.
2. Выберите из выпадающего списка механизм расчета прогресса.
3. В случае выбора расчета по метрике введите название метрики (опционально), стартовое и целевое значения.
4. Нажмите **Применить**.

<figure><img src="../../../.gitbook/assets/изображение (201).png" alt=""><figcaption></figcaption></figure>

Для выключения расчета прогресса:

1. Нажмите кнопку настроек <img src="../../../.gitbook/assets/изображение (200).png" alt="" data-size="line">на виджете.
2. Выберите из выпадающего списка **Не рассчитывать**.
3. Нажмите **Применить**.

Виджет будет скрыт из карточки задачи.&#x20;

## Изменение текущего значения прогресса при расчете по метрике

1. Перейдите в карточку задачи или в представление «Таблица» с включенным столбцом **Прогресс**.
2. Кликните на отображение прогресса <img src="../../../.gitbook/assets/изображение (203).png" alt="" data-size="line">.
3. В открывшемся виджете введите новое значение и комментарий (опционально).
4.  Нажмите **Обновить**.&#x20;

    <figure><img src="../../../.gitbook/assets/изображение (202).png" alt=""><figcaption></figcaption></figure>

Изменение прогресса отражается в [истории изменений](https://docs.teamstorm.io/rukovodstva/rukovodstvo-polzovatelya-teamstorm/rabota-s-zadachami/prosmotr-istorii-sozdaniya-i-izmeneniya-zadachi) в карточке задачи.