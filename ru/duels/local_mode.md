##Локальный запуск дуэлей

Дуэли можно запускать не только на сайте, но и локально.

####Зачем нужна возможность локального запуска

Так вы сможете проверить, какой из вариантов одной стратегии работает быстрее (ведь стратегию можно написать по-разному), а также посмотреть, как себя ведут несколько стратегий в одном биржевом стакане.

####Как запустить несколько стратегий

Принцип запуска нескольких стратегий не отличается от [запуска одной стратегии](/local-pack/run_strategy.md).

Предположим, что ваши стратегии называются *first_strategy* и *second_strategy*.
Их может быть больше двух.
О том, как создавать новые стратегии, читайте [здесь](/local-pack/add_strategy.md).

В случае командной строки, нужно запустить скрипт:
```bash
./run.py first_strategy second_strategy
```

В случае CLion нужно прописать во вкладке *Run > Edit configurations* в строчке *Program arguments*:
```
strategies/first_strategy/first_strategy.json strategies/second_strategy/second_strategy.json
```