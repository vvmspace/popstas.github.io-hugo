+++
title = "CHANGELOG.md: ручное и автоматическое ведение истории изменений проекта в Git"
date = "2016-03-06T10:09:13"
slug = "changelog-dot-md-generate-from-git-conventions"
Tags = ["transmission-cli", "git", "changelog", "github", "docs"]
+++

С начала января я веду свой [проектик](/blog/2016/01/17/torrent-transmission-client-for-weburg/), на котором обкатываю новые для меня технологии:

- Статический анализ кода, phpcs, phpmd, Scrutinizer
- Автоматическая сборка, Travis CI
- Unit тесты, PHPUnit
- Покрытие кода, Coveralls
- Работу через задачи для любых изменений, Github Issues, PhpStorm tasks
- Документирование всего: README, CHANGELOG, сайт проекта, --help

В этом посте изложена история изменений моего мнения о разных генераторах историй изменения.

Tl;dr: conventional-changelog, стандартизация коммитов.

<img src="/images/2016-03/changelog.png" />

<!--more-->

# CHANGELOG.md
Понятная для человека история изменений проекта нужна. Тут надо заметить что такими историями не являются:

- Issues проекта, ветка в менеджере задач, доска проекта и т.п.
- git log проекта

Файл CHANGELOG.md в корне проекта стал стандартом де-факто для проектов, в котором ведется история изменений, Gitlab даже делает для него отдельную вкладку на странице репозитория.

Про это, конечно, есть [сайт](http://keepachangelog.com/), [репозиторий на Github](https://github.com/olivierlacan/keep-a-changelog) с тысячей звезд, проблема явно беспокоит людей.

Про ведение CHANGELOG я задумался, когда изучал проект [otto](https://github.com/hashicorp/otto/), когда писал про него [статью на хабр](http://habrahabr.ru/post/273009/).

#### Структура у CHANGELOG более-менее у всех одна и та же:
- Версия и дата релиза
- Сломанные обратные совместимости
- Новые фичи
- Прочие изменения и улучшения
- Исправленные баги

Вести такой документ достаточно просто, я за 120 коммитов почти не забывал это делать. В файле нужно всегда держать вверху секцию Next Release с подготовленными заголовками, как-то так:

```
## Next Release

BREAKING CHANGES:

FEATURES:

IMPROVEMENTS:

BUG FIXES:
```

Перед коммитом я всегда просматриваю дифф, в это время я записываю в коммент к коммиту кратко изменение в первую строку и более подробно в третью, если изменений больше одного, делаю в виде списка. Если про это есть задача, нужно упомянуть ее в виде #123 ссылки, Github умный и такие ссылки делает активными.

Так вот, нужно просто добавить в этот процесс копипасту коммента к коммиту в CHANGELOG, с раскладыванием по категориям изменений.

Во время релиза называем секцию, ставим ей дату, копипастим заголовки.

Процедура очень простая, настолько простая, что хочется ее поручить роботу.


## github_changelog_generator

[github_changelog_generator](https://github.com/skywinder/github-changelog-generator) - ruby утилита, которая умеет генерировать CHANGELOG.md из любого репозитория. На выходе получаем документ типа [этого](https://github.com/skywinder/github-changelog-generator/blob/master/CHANGELOG.md), наполненный ссылками на задачи и пулл-реквесты, разбитый по категориям, все круто, как в рекламе. У меня получилось совсем не так красиво.

Что мне не понравилось в этом генераторе:

- Текст коммитов никак не учитывает, как и текст задач.
- Чтобы она нормально работала, нужно по полной использовать Github Issues и метки для них, пулл-реквесты, в общем сильно завязано на Github (кто бы мог подумать?), иначе будут генериться просто ссылки на диффы между тегами.
- Нельзя указывать свои секции (например, Breaking changes встроенного нет), но есть [issue #316](https://github.com/skywinder/github-changelog-generator/issues/316) про это, судя по активности проекта, они скоро появятся.

Что понравилось:

- Поведение из коробки что-то генерирует, даже если вы не думали про CHANGELOG.md до этого и не использовали Github фишки, это лучше, чем ничего. Но не намного.
- Можно привязывать свои метки к существующим секциям лога.
- Можно настраивать как параметрами к команде, так и конфигом. При запуске скрипт говорит: `Performing task with options`, так вот, каждую строку из перечисленного ниже конфига можно вставить в файл `.github_changelog_generator` и переопределить, заменив `_` на `-`.
- Поддерживает сосуществование заполняемой вручную версии (которая все равно лучше автоматической) и генерируемого лога, для этого нужно переложить старый CHANGELOG.md в HISTORY.md (или другой файл, указав его в конфиге).

В общем, github_changelog_generator в моем случае подходит хорошо,
если вся работа ведется на Github, это самый простой способ получить красивый CHANGELOG.md

Но на этом я не успокоился, основная причина в том, что на рабочие проекты на Github я не делаю. Хотелось более общего решения.


## git-extras changelog
[tj/git-extras](https://github.com/tj/git-extras) - это [огромный](https://github.com/tj/git-extras/blob/master/Commands.md) (около 50) пакет дополнительных команд, упрощающих работу с git. Я его раньше уже видел, но в то время подумал, что мне и встроенных в git команд слишком много. Но в поисках генератора снова набрел на него, у него есть такая команда.

Вот таким нехитрым способом можно в одну команду сгенерировать и запушить лог для проекта, где его не было, но версии помечались тегами и комменты к коммитам были осмысленными:
```
git changelog -a -p -x > CHANGELOG.md && git add CHANGELOG.md && git commit CHANGELOG.md -m "add CHANGELOG.md" && git push origin master
```

Для пробы сделал лог для [site-setup](https://github.com/popstas/site-setup/blob/5cb4f52bfc5909bac8b8bc77540cf3283b94ff2a/CHANGELOG.md), [server-scripts](https://github.com/popstas/server-scripts/blob/009d82420fa4623417cf437b00df36c662c759a2/CHANGELOG.md), [drupal-scripts](https://github.com/popstas/drupal-scripts/blob/b0b7a5907798ebde714471fbf1611c3232df5925/CHANGELOG.md), на этом успокоился, больше в общем и тестить не на чем.

Ниже я отказался от него в пользу `conventional changelog`.

#### Плюсы:
- Простой как дверь, выполняешь команду, получаешь список изменений, разделенных версиями

#### Минусы:
- Нет почти никаких настроек


## rafinskipg/git-changelog
[rafinskipg/git-changelog](https://github.com/rafinskipg/git-changelog) - node.js cкрипт, который парсит коммиты, написанные по [стандартам Angular](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#). Я их прочитал, оказалось, что стандарты годные, к angular никак не привязаны.

Конфликтует с git-extras, так как оба они хотят называться git-changelog. Этот я сделал симлинком `git-changelog-angular`.

Параметров у скрипта немного, я с ними поигрался, но ничего хорошего у меня с этим тулом не вышло. Идем дальше.


## conventional-changelog
[stevemao/conventional-changelog-cli](https://github.com/stevemao/conventional-changelog-cli) - node.js скрипт, также нацелен на стандарты Angular, но, [по заявлениям](https://github.com/stevemao/conventional-changelog-cli#why) авторов это как раз то, что нужно:

- поддерживает свои форматы коммитов и несколько общих: 'angular', 'atom', 'codemirror', 'ember', 'eslint', 'express', 'jquery', 'jscs', 'jshint'
- поддерживает шаблоны
- протестирован, в отличие от github_changelog_generator
- отвязан от Github
- имеет модульную структуру и несколько модулей вокруг себя

Воспользовавшись `conventional-commits-detector`, узнал, что мои комменты к коммитам больше всего похожи на стандарт `eslint`.

Сгенерированный лог дал понять, что в eslint принято указывать категорию и через двоеточие суть, так коммиты в релизе разбиваются по категориям. Но в целом, конечно, коммиты были названы неправильно и хорошего лога не получилось.

Зато запуск без указания пресета сообщений выдал почти то же, что и `git-extras`, но вдобавок к этому задал мажорным и минорным версиям разный уровень и указал ссылку на коммит на Github для каждого коммита.

Сгенерировать лог с нуля можно командой:
```
conventional-changelog -i CHANGELOG.md -s -r 0
```

После этого я конечно побежал исправлять логи у проектов, которым сделал логи час назад, вот что вышло: [site-setup](https://github.com/popstas/site-setup/blob/fd159ed7848aaf8695642bcb53c795922d307dd6/CHANGELOG.md), [server-scripts](https://github.com/popstas/server-scripts/blob/ef6138faf0179f31929ff0d90d98466749d4f85b/CHANGELOG.md), [drupal-scripts](https://github.com/popstas/drupal-scripts/blob/3eb923c09e319a163f9fea9669dfa735b60044c1/CHANGELOG.md).

Для проектов на своем Gitlab все сложнее: чтобы правильно делались ссылки на коммиты, нужно, во-первых, указать адрес проекта через файл package.json:
```
{
  "name": "myproject",
  "repository": {
    "type": "git",
    "url": "http://my.gitlab.ru/projects/myproject.git"
  }
}
```
А во-вторых не знаю, что надо сделать, он генерит ссылки с сокращенными хэшами, которые Github понимает, а Gitlab открывает страницу списка коммитов, т.к. ему нужен полный хэш, шаблон сходу не нашел.

Дальше искать не стал, думаю это оно самое.

Кроме лучшего результата из коробки и полной кастомизации мне в нем понравились модули:

- [angular-precommit](https://github.com/ajoslin/angular-precommit) - готовый валидатор сообщений к коммитам
- [conventional-changelog-lint](https://github.com/marionebl/conventional-changelog-lint) - скрипт для pre-commit хука, проверяющий сообщения коммитов на соответствие стандартам, стандарты описываются в файле
- [conventional-github-releaser](https://github.com/stevemao/conventional-github-releaser) - автоматическое создание релизов на Github. У меня они уже создаются, но приходится вручную заходить туда и править сообщение к релизу



# Выводы
Для того, чтобы генератор создавал по-настоящему хорошие логи, важно определиться с форматом сообщений к коммитам, научиться следовать ему и научить роботов понимать наш формат, чтобы роботы ~~поработили людей~~ помогали правильно и не напрягаясь вести историю изменеий проекта в процессе, а не после работы над проектом.

Для себя я нашел инструмент, которым я теперь могу за 5 минут создавать историю изменений для проектов на основе коммитов.

Генерация CHANGELOG.md - шаг в сторону хорошей и актуальной документации по проекту, которая не будет занимать часы или дни, она будет частью рабочего процесса, конечно для маленького проекта из одного программиста это избыточно, мягко говоря, но надо же с чего-то начинать.

## UPD 08.03.2016
Добавил валидатор:
```
npm install -g conventional-changelog-lint
echo 'conventional-changelog-lint -e' > .git/hooks/commit-msg
chmod +x .git/hooks/commit-msg
```
После этого коммиты с неправильными сообщениями перестанут проходить.

Перед релизом генерирую CHANGELOG.md:
```
conventional-changelog -p angular -i CHANGELOG.md -s
```
Это допишет в лог содержимое коммитов с последнего релиза (semver тега). После этого остается поправить руками то, что не нравится, проставить версию.

После этого я генерирую документацию специфичной для проекта командой, коммит, тег, пуш:
```
git add .
git commit -m 'docs: v0.6.0'
git push --follow-tags
```

После этого релиз. Релиз будем делать через `conventional-github-releaser`:
```
npm install -g conventional-github-releaser
CONVENTIONAL_GITHUB_RELEASER_TOKEN=your_public_repo_token conventional-github-releaser -p angular
```
Еще не разобрался с тем, как это скрестить с выкладкой PHAR архива с Travis: для `github-releaser` нужно, чтобы релиза еще не было, но он создается автоматически при пуше тега на Github. После удаления релиза (превращения в Draft), github-releaser отработал, вставил данные CHANGELOG в релиз, все как надо.