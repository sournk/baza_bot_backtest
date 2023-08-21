# Бектест максимальных просадок торгового советника Eve

## TLDR

Советник Eve на ценах золота четырех лет с 2019 по 2022:
1. Сливает депозит при любом лоте на ценовой ситуации золота 30.05.2019 года при открытии сетки против глобального минимума цены. Доливы не спасают депозит и будут слиты для обеспечения постоянно растущей маржы. Отключение автоторговли тоже не спасает, т.к. рост цены золота опять же будет требовавать пополения депозита для обеспечения маржи. Единственный выход в этой ситуации - это закрыть сетку в убыток вручную. Причем чем раньше тем лучше. Но распознать такую ситуацию пока робот работает по алгоритму вплоть до 20-го ордера невозможно.
2. При лоте 0.01/$1000 потребует долив 4 раза от 15% до 167% депозита.
3. При лоте 0.015/$1000 потребует долив 5 раз от 48% до 304% депозита.
4. При лоте 0.02/$1000 потребует долив 10 раз от 8% до 406% депозита.

## Параметры тестирования

1. Период с 2019 года по 2022 включительно.
2. В MT4 загружены архивные данные цен M1, M5, M15, M30, H1, H4, D1.
3. Настроены графики для отображения большого числа баров для прогона истории.
4. Тестируется робот Eve.
5. Исопльзуется модель тестирования "Все тики (наиболее точный метод на основе всех наименьших доступных таймфреймов)". Она самая медленная, но единственная подходит для Eve, которая торгует разнонаправленными сетками и обновляет ордерам TP.
6. В бектестах используются параметры лотов для депозита в $10000. Хотя сам стартовый депозит в тестах установлен заведомо больший $30_000 (иногда $100_000), чтобы получить абсолютные значения просадок, по которым рассчитывается просадка и необходый долив.
7. Используется режим оптимизации, который прогоняет тесты на каждом периоде для 3-х значений лота:
    - 0.1 по консервативным рекомендациям 0.01 на $1000;
    - 0.15 для 0.015 на $1000;
    - 0.2 для 0.02 на $1000.
11. Просадки больше 90% депозита дополнительно проверены на графиках, чтобы получить состав и параметры собранных сеток (количество ордеров, цены открытий, количества и TP).

## Результаты тестирования

1. Результаты работы оптимизатора по каждому месяцу года находятся в файлах внутри каталогов годов.
2. Месяцы с просадками депозита более 100% для лота 0.01/$1000 содержат подробный отчет со всей историей открытия и изменения каждого ордера.

# 2019 год

**ВЫВОД по 2019 году:** 
В двух месяцах возникают критические ситуации:
- в мае 2019 года: уникальная ценовая ситуация, которая приводит к сливу любого депозита при любых настройках и любом доливе, потому что открывается сетка в SELL против глобального минимума цены золота. Единственная возможность уменьшить убыток - это как можно раньше закрыть сетку вручную, зафиксировав убыток. Распознать эту ситуацию по графикам в реальном времени вряд ли возможно.
- в июле 2019 года: при лоте 0.01/$1000 простадка составляет почти 100%. В тестере робот проходит эту ситуацию без долива. Но для больших лотов требуется доливы 45% и 85% соответсвенно.

![Просадки в 2019 год](https://github.com/sournk/baza_bot_backtest/blob/main/2019.%20Results.png)

![30.05.2019](https://github.com/sournk/baza_bot_backtest/blob/main/2019/2019-05-30-XAUUSDM1.png)

# 2020 год

**ВЫВОД по 2020 году:** 
1. Для лота 0.01/$1000 долив потребуется дважды: в августе 2020 около 15% депозита и в ноябре - больше 167% депозита. В обоих случаех можно было бы обойтись без долива, если успесть отключить автоторговлю  на 17-ом и 15-ом ордере, и дождаться отскока цены.
2. Для лота 0.015/$1000 долив потребовался также дважды. Только максимальный объем долива почти 304%  депозита.
3. Для лота 0.02/$1000 долив потребовался также 4раза. Максимальный объем долива почти 406% депозита.

![Просадки в 2020 год](https://github.com/sournk/baza_bot_backtest/blob/main/2020.%20Result.png)

![12.08.2020](https://github.com/sournk/baza_bot_backtest/blob/main/2020/2020-08-12-XAUUSDM1.png)

![10.11.2020](https://github.com/sournk/baza_bot_backtest/blob/main/2020/2020-11-10-XAUUSDM1.png)


# 2021 год

**ВЫВОД по 2021 году:** 
1. Для лота 0.01/$1000 долив потребуется дважды: в июне 2020 около 81% депозита и в августе - больше 41% депозита. В обоих случаех можно было бы обойтись без долива, если успесть отключить автоторговлю на 16-ом и 14-ом ордере, и дождаться отскока цены.
2. Для лота 0.015/$1000 долив потребовался также дважды. Только максимальный объем долива почти 173%  депозита.
3. Для лота 0.02/$1000 долив потребовался также 3 раза. Максимальный объем долива почти 242% депозита.

![Просадки в 2021 год](https://github.com/sournk/baza_bot_backtest/blob/main/2021.%20Result.png)

![14.06.2021](https://github.com/sournk/baza_bot_backtest/blob/main/2021/2021-06-14-XAUUSDM1.png)

![14.06.2021](https://github.com/sournk/baza_bot_backtest/blob/main/2021/2021-08-09-XAUUSDM1.png)


# 2022 год

**ВЫВОД по 2022 году:** Долив потребуется только для лота 0.02/$1000 два раза в максимальном объеме 23%.

![Просадки в 2022 год](https://github.com/sournk/baza_bot_backtest/blob/main/2022.%20Result.png)
