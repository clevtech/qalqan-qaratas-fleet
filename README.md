# qalqan-qaratas-fleet

Канал команд парка Қаратас (глобальный стоп/резюм/вайп).

`fleet-command.json` — последний ПОДПИСАННЫЙ конверт `{cmd_b64, sig_b64, key_id}` (Ed25519, ключ
владельца 3ca49f81). Команда внутри: `{type: stop|resume|wipe, seq, scope, issued, confirm?}`.

- Триггерит только владелец: `beren-zik2-firmware/tools/fleet_key.py sign ... > fleet-command.json`,
  затем commit + push. `seq` строго растёт; `wipe --scope all` требует `confirm`.
- Планшет фоном тянет raw-файл при интернете и молча раздаёт ружьям по mTLS (POST /api/fleet-command).
- Проверка подписи, монотонного seq и scope — на РУЖЬЕ (FleetGate). Правка файла без приватного
  ключа = мусор (bad_signature) или откат (stale_seq) → ничего не триггерит.

Пустой `{}` = нет активной команды (no-op).
