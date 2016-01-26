# Документация {{ book["contest.name"] }}
Документация для [{{ book["contest.name"] }}]({{ book["contest.landing-page.url"] }}).


# Как начать участие
- Зарегистрируйтесь на сайте [{{ book["contest.landing-page.name"] }}]({{ book["contest.landing-page.url"] }}).
- Отправьте одну из стратегий-примеров, находящихся в интерфейсе участника по умолчанию. Подробнее в разделе [Интерфейс участника](./docs/web-interface/README.md).
- Скачайте пакет для локальной разработки на С++. Пакет опубликован на [GitHub](https://github.com/hftbattle/local_pack). 
Его можно скачать следующей командой (должен быть установлен [Git](http://git-scm.com/download)):
```
git clone https://github.com/hftbattle/local_pack.git
```
Подробнее в разделе [Пакет для локальной разработки](./docs/local-pack/README.md).
- Для анализа процесса реальных торгов, вы можете использовать [viewer]({{ book["viewer.url"] }}). Подробнее в разделе [Viewer](./docs/viewer/README.md).
- Прочитайте документацию: вам помогут разделы [Написание стратегий](docs/strategy/README.md), [Симулятор торгов](docs/simulator/README.md), а также [Словарь терминов](docs/glossary.md) и [FAQ](docs/FAQ.md).
- Можете приступать к написанию своей стратегии!

{% include "./docs/general/rules.md" %}

# Вопросы и поддержка
Свои вопросы вы можете задавать, написав нам на почту {{ book["contest.support.email"] }}.
