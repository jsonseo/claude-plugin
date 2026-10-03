# JSON SEO для Claude

Плагин подключает к Claude MCP-сервер [JSON SEO](https://jsonseo.ru): поисковую
выдачу Яндекса, Google и Bing, позиции сайта по запросу, поисковые подсказки,
Яндекс Вордстат (частотность, похожие запросы, сезонность, география) и прогноз
бюджета Яндекс Директа. Три скилла объясняют Claude, какой инструмент брать
в какой задаче и как не переплачивать за лишние страницы и токены.

*English summary: this plugin connects Claude to the JSON SEO MCP server for
Yandex, Google and Bing search results, site rankings, autocomplete
suggestions, Yandex Wordstat keyword data and Yandex Direct budget forecasts.
Skills teach Claude which tool to use for which SEO task. A JSON SEO API key is
required; usage is billed per the JSON SEO tariff.*

## Что умеет

- **Позиции и выдача** — `yandex_search`, `google_search`, `bing_search`,
  `*_position`, `*_suggest`, справочники регионов `yandex_regions`
  и `google_regions`.
- **Семантика** — `wordstat`, `wordstat_frequency`, `wordstat_graph`,
  `wordstat_map`.
- **Прогноз Директа** — `direct_forecast`: показы, клики, ставки и бюджет по
  списку фраз одним вызовом.

Полная документация по инструментам и параметрам: <https://jsonseo.ru/mcp-docs>.

## Как пользоваться

1. Зарегистрируйтесь на [jsonseo.ru](https://jsonseo.ru) и возьмите API-ключ
   в личном кабинете. Пополните баланс — инструменты платные.
2. Установите плагин. При включении Claude Code спросит API-ключ и сохранит
   его в защищённом хранилище; в файлы плагина ключ не попадает.
3. Спрашивайте в чате: «Какая позиция у example.com в Яндексе по запросу
   „купить ноутбук“ в Казани?», «Собери семантику вокруг „ремонт айфона“
   с точной частотностью», «Посчитай бюджет Директа для этого списка фраз
   по Москве».

Claude сам подберёт регион через `yandex_regions`/`google_regions`, выберет
экономный инструмент (`*_position` вместо полной выдачи, `wordstat_frequency`
вместо `wordstat`) и отдаст список фраз в `direct_forecast` одной пачкой.

## Тарификация

Такая же, как у обычного API JSON SEO: 0,01 ₽ за страницу выдачи, 0,01 ₽ за
запрос к Вордстату или подсказкам, 0,01 ₽ за пачку фраз в прогнозе Директа.
Поиск регионов бесплатен. Подробнее: <https://jsonseo.ru/pricing>.

## Какие данные и куда отправляются

Плагин не запускает локального кода и ничего не хранит на компьютере. Все
вызовы инструментов уходят по HTTPS на `https://jsonseo.ru/mcp` с заголовком
`Authorization: Bearer <ваш API-ключ>`. На сервер передаются только параметры
инструментов: поисковые запросы, домены, регионы, списки фраз. Ответы
приходят от JSON SEO в структурированном виде. Условия использования:
<https://jsonseo.ru/offer>.

## Ручное подключение без плагина

Сервер можно подключить и напрямую:

```bash
claude mcp add --transport http jsonseo https://jsonseo.ru/mcp \
  --header "Authorization: Bearer <API-ключ>"
```

## Конфиденциальность

Обработка персональных данных описана в разделе 8 оферты:
<https://jsonseo.ru/offer#privacy> (Privacy Policy).

## Лицензия

MIT. Сам сервис JSON SEO — коммерческий, см. [оферту](https://jsonseo.ru/offer).
