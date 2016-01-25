# Документация {{ book["contest.name"] }}
Документация для [{{ book["contest.name"] }}]({{ book["contest.landing-page.url"] }}).


# Как начать участие
Для участия в соревновании:
- Зарегистрируйтесь на сайте [{{ book["contest.landing-page.name"] }}]({{ book["contest.landing-page.url"] }}).
- Отправьте одну из стратегий-примеров, находящихся в интерфейсе участника по умолчанию. Подробнее в разделе [Интерфейс участника](./docs/web-interface/README.md).
- Скачайте С++ проект на [GitHub](({{ book["contest.local-pack.url"]}}) для разработки у себя на компьютере. Для тех, у кого установлена консольная утилита [Git]({{ book["git-download.url"] }}), можно выполнить следующую команду:
```
git clone https://github.com/hftbattle/local_pack.git
```
Подробнее в разделе [Local Pack](./docs/local-pack/README.md).

- Прочитайте документацию: вам помогут разделы [Написание стратегий](docs/strategy/README.md), [Симулятор торгов]()
- Для анализа процесса реальных торгов, вы можете использовать [viewer]({{ book["viewer.url"] }}). Подробнее в разделе [Viewer](./docs/viewer/README.md).
- Можете приступать к написанию своей стратегии!

{% include "./docs/general/rules.md" %}

# Вопросы и поддержка
Свои вопросы вы можете задавать, написав нам на почту {{ book["contest.support.email"] }}.
