# Документация {{ book["contest.name"] }}
Документация для [{{ book["contest.name"] }}]({{ book["contest.landing-page.url"] }}), соревнования по HFT стратегиям.


#  Как начать участие
- Зарегистрируйтесь на сайте [{{ book["contest.arena.name"] }}]({{ book["contest.arena.url"] }}).
- Отправьте одну из стратегий-примеров, находящихся в интерфейсе участника по умолчанию. Подробнее в разделе [Интерфейс участника](interface/README.md).
- Скачать пакет для локальной разработки на С++.  Для этого можно [скачать]({{ book["contest.local-pack.virtual.url"] }}) настроенный образ [VirtualBox]({{book["virtualbox.url"]}}), либо склонировать пакет, опубликованный на [GitHub](https://github.com/hftbattle/hftbattle) (должен быть установлен [Git](http://git-scm.com/download)):
```
git clone https://github.com/hftbattle/hftbattle.git
```
Подробнее в разделе [Пакет для локальной разработки](local-pack/README.md).
- Прочитайте документацию: вам помогут разделы [Написание стратегий](strategy/README.md), [Симулятор торгов](simulator/README.md), а также [Словарь терминов](terms.md) и [HFAQ](HFAQ.md).
- Можете приступать к написанию своей стратегии!

# Вопросы и поддержка
Свои вопросы вы можете задавать в комментариях на страницах документации или написав нам на почту {{ book["contest.support.email"] }}.
