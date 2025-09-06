# Формулы для Google Sheets (лист "Summary")

Важно: формулы ниже используют запятые как разделители (Google Sheets). Предполагается, что данные на листе "Data" (переименуй импортированный лист).

1) Идентификатор недели (добавь в столбец рядом с датой на листе Data, напр. в колонку P, строка 2):
=TEXT(A2,"yyyy")&"-W"&TEXT(WEEKNUM(A2,2),"00")

2) Средний CS@10 по миду (без автофилла), за всю таблицу:
=AVERAGEIFS(Data!G:G, Data!D:D, "mid", Data!E:E, "N", Data!G:G, ">0")

3) Средний Deaths<10 (мид, без автофилла):
=AVERAGEIFS(Data!I:I, Data!D:D, "mid", Data!E:E, "N")

4) Clean first clear rate (лес):
=IFERROR( COUNTIFS(Data!D:D,"jungle", Data!H:H,"clean") / COUNTIFS(Data!D:D,"jungle"), 0 )

5) Участие в ранних объектах (лес):
=IFERROR( AVERAGEIFS(Data!L:L, Data!D:D,"jungle"), 0 )

6) Процент автофилла:
=IFERROR( COUNTIFS(Data!E:E,"Y") / COUNTA(Data!A:A), 0 )

7) Топ-тег первой смерти (по частоте):
=INDEX(UNIQUE(FILTER(Data!M:M, Data!M:M<>"")), MATCH( MAX(COUNTIF(Data!M:M, UNIQUE(FILTER(Data!M:M, Data!M:M<>"")))), COUNTIF(Data!M:M, UNIQUE(FILTER(Data!M:M, Data!M:M<>""))), 0 ))

(Это "массовая" формула — в Google Sheets может потребоваться Ctrl+Shift+Enter. Если сложно — просто посчитай визуально по фильтру.)

8) Недельные метрики (по текущей неделе). В ячейке A2 листа Summary задай текущую неделю, например: 2025-W36. Далее:

Текущая неделя средний CS@10 (мид):
=AVERAGEIFS(Data!G:G, Data!D:D,"mid", Data!E:E,"N", Data!P:P,$A$2, Data!G:G,">0")

Текущая неделя Deaths<10 (мид):
=AVERAGEIFS(Data!I:I, Data!D:D,"mid", Data!E:E,"N", Data!P:P,$A$2)

Текущая неделя clean clear rate (лес):
=IFERROR( COUNTIFS(Data!D:D,"jungle", Data!H:H,"clean", Data!P:P,$A$2) / COUNTIFS(Data!D:D,"jungle", Data!P:P,$A$2), 0 )

Текущая неделя участие в ранних объектах (лес):
=IFERROR( AVERAGEIFS(Data!L:L, Data!D:D,"jungle", Data!P:P,$A$2), 0 )

9) Сумма времени за неделю (минуты):
=SUMIFS(Data!N:N, Data!P:P, $A$2)

Подсказка: чтобы график "было→стало", можно делать фильтр по неделям и строить линию для CS@10 и Deaths<10.