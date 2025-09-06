# Google Sheets — быстрая настройка

Цель: завести один файл "LoL Progress", лист Data (сырые записи) и лист Summary (сводка/графики).

Шаг 1 — Создай файл и лист Data
1) Открой sheets.new (или Google Диск → Создать → Таблица).
2) Переименуй первый лист в Data.
3) Вставь в строку 1 такие заголовки (как в progress_template.csv):
date | queue | champ | role | autofill | result | cs10 | first_clear_status | deaths10 | deaths_total | ward_by_3 | early_objective | firstDeathTag | session_minutes | notes
4) Формат:
- date: Дата (YYYY-MM-DD).
- autofill: Y/N.
- result: W/L.
- cs10: число (может быть пустым для леса).
- first_clear_status: для леса "clean"/"unclean".
- ward_by_3, early_objective: 1/0.
- session_minutes: целое.

Шаг 2 — Лист Summary (сводка и формулы)
1) Создай второй лист Summary.
2) В ячейку A1: "Current Week (yyyy-Www)". В A2 впиши текущую неделю, например 2025-W36.
3) Вставь формулы (см. ниже). Если лист Data называется по‑другому — поменяй имя в формулах.

Средний CS@10 (мид, без автофилла, за весь период):
=AVERAGEIFS(Data!G:G; Data!D:D; "mid"; Data!E:E; "N"; Data!G:G; ">0")

Средний Deaths<10 (мид, без автофилла):
=AVERAGEIFS(Data!I:I; Data!D:D; "mid"; Data!E:E; "N")

Clean first clear rate (лес):
=IFERROR( COUNTIFS(Data!D:D;"jungle"; Data!H:H;"clean") / COUNTIFS(Data!D:D;"jungle"); 0 )

Участие в ранних объектах (лес):
=IFERROR( AVERAGEIFS(Data!L:L; Data!D:D;"jungle"); 0 )

Процент автофилла:
=IFERROR( COUNTIFS(Data!E:E;"Y") / COUNTA(Data!A:A); 0 )

Топ‑тег первой смерти (по частоте) — можно посчитать фильтром или формулой:
=INDEX(UNIQUE(FILTER(Data!M2:M; Data!M2:M<>"")); MATCH( MAX(COUNTIF(Data!M2:M; UNIQUE(FILTER(Data!M2:M; Data!M2:M<>"")))); COUNTIF(Data!M2:M; UNIQUE(FILTER(Data!M2:M; Data!M2:M<>""))); 0 ))

Метрики за текущую неделю (подставляет ключ из A2, например 2025-W36):
Сначала добавь на лист Data служебную колонку P (WeekKey) с формулой в P2:
=TEXT(A2;"yyyy")&"-W"&TEXT(WEEKNUM(A2;2);"00")
Скопируй вниз.

Теперь в Summary:
CS@10 (мид, текущая неделя):
=AVERAGEIFS(Data!G:G; Data!D:D;"mid"; Data!E:E;"N"; Data!P:P;$A$2; Data!G:G;">0")

Deaths<10 (мид, текущая неделя):
=AVERAGEIFS(Data!I:I; Data!D:D;"mid"; Data!E:E;"N"; Data!P:P;$A$2)

Clean clear rate (лес, текущая неделя):
=IFERROR( COUNTIFS(Data!D:D;"jungle"; Data!H:H;"clean"; Data!P:P;$A$2) / COUNTIFS(Data!D:D;"jungle"; Data!P:P;$A$2); 0 )

Участие в ранних объектах (лес, текущая неделя):
=IFERROR( AVERAGEIFS(Data!L:L; Data!D:D;"jungle"; Data!P:P;$A$2); 0 )

Сумма времени за неделю (мин):
=SUMIFS(Data!N:N; Data!P:P; $A$2)

Шаг 3 — Графики
- Вставка → Диаграмма → Линейная: по Data!A:A (даты) и Data!G:G (CS@10, отфильтровать role=mid, autofill=N).
- Второй график: Deaths<10 по датам (аналогично).

Подсказки
- Если Google Sheets недоступен, используй Excel/LibreOffice и те же колонки.
- Поля ward_by_3 и early_objective можно заполнять только для релевантных ролей.