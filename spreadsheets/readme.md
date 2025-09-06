# Как пользоваться таблицами

Файл progress_template.csv — основная таблица по играм:
- date: дата игры.
- queue: обычно "Ranked".
- champ, role: из твоего пула.
- autofill: Y если автофилл/не твоя роль; иначе N.
- result: W/L.
- cs10: для мида. Для леса можно оставить пустым.
- first_clear_status: для леса — "clean" (без бэка/пустых пробежек) или "unclean".
- deaths10, deaths_total: смерти до 10:00 и за всю игру.
- ward_by_3: 1/0 — был ли вард до 3:00 (для мида/саппорта).
- early_objective: 1/0 — участие хотя бы в одном из первых 2 объектов (дракон/Геральд) — для леса.
- firstDeathTag: одна из категорий: Greed, Wave, Vision, Positioning, Overstay, Tempo, FightChoice.
- session_minutes: сколько минут заняла игровая «сессия» (полезно для контента «мало времени»).
- notes: короткий контрфакт (что сделал бы иначе).

Файл mistakes_template.csv — опционально для детального учёта ошибок (можно не вести отдельно, если хватает поля firstDeathTag в основной таблице).

Если используешь Google Sheets:
- Импортируй CSV (Файл → Импорт → Заменить лист).
- Создай второй лист "Summary" и вставь формулы из файла spreadsheets/summary_formulas.md.