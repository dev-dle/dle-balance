/*
| 
| Если вы планируете использовать функционал для автоподнятия новостей, вам стоит предварительно проставить настроить модуль и проставить
| ссылки через кнопку "обновить эмбеды" без галочек "поднимать новость при выходе новых серий/качеств". Далее уже настроить и запустить 
| крон-команду и активировать нужную опцию. Так при первом срабатывании меньше новостей "взлетит" на главную.
| 
| При первом срабатывании крон поднимет много новостей (все, где сезон/серия не совпадает с нашими), но далее будет поднимать только 
| обновления.
| 
| Поскольку модуль работает по крону, во избежание излишних нагрузок на API команду стоит задавать с умеренной частотой. 
| 
| 
|---------------------------------------------
| Что нужно что бы начать работать с модулем 
|---------------------------------------------
| 
| 1) Укажите свой API ключ
| 2) Укаживе поля с доп полями для поиска.
| 	2.1) Можно указать как одно так и несколько полей для поиска. Тогда модуль будет искать эмбеды по приоритету
| 	2.2) Приоритет поиска по доп полях: kinopoisk.ru, ImDb, World Art, по названию!
| 3) Выберите категории
| 4) укажите куда будет вставлен эмбед
| 
| 
|---------------------------------------------------------------------------------------------------------
| Что бы добавить кнопку поиска на страницах "Добавление новости" и "Редактирование публикаций" нужно... 
|---------------------------------------------------------------------------------------------------------
|
| 1) Найдите строчку // End XFields Call 
| 
| 2) Вставьте под ней эту строчку
| if (file_exists(ENGINE_DIR . '/inc/CCDN/button.php')) { $output .= include ENGINE_DIR . '/inc/CCDN/button.php';}
|
| В файлах: "engine/inc/addnews.php" "engine/inc/editnews.php"
| 
| 
| 
| --------------------------------------------------
| Поля который модуль будет изменять в ходе роботы
| --------------------------------------------------
| 
| Доп. поле для вставки эмбеда. + поля с данными об озвучке и качеству видео (если выбраны)
| 
| --------------------------------------------------------------------------------------------------------------------------
| Поля которые модуль будет изменять в ходе роботы, если включена опция "Обновлять дату новости когда появится новая серия"
| --------------------------------------------------------------------------------------------------------------------------
| 1) Доп. поле для выбора сезона
| 2) Доп. поле для выбора серии 
| 3) Служебное доп. поле с информацией об общем количестве эпизодов сериала
| 4) Также, при необходимости, есть возможность дописывать слово "сезон" и "серия" к соответствующим доп. полям.
| 
|
| -------------------------
| Добавлений новых франшиз
| -------------------------
| 
| Чтобы модуль создал новость с указанными в уго настройках доп. полями нужно сначала сохранить указанные настройки,
| а потом осуществить добавление новых франшиз.
| 
| -------------------------------
| Описание всех полей плагина
| -------------------------------
|
*/
1) API ключ - поле, в которое нужно указать ваш API ключ. Найти его можно в личном кабинете балансера в разделе "Документация"
2) ID видео на kinopoisk.ru из поля - стоит указать доп поле, в котором вы храните id Kinopoisk (если есть) по даным с этого поля будет осуществляться поиск.
3) ID видео на ImDb из поля - стоит указать доп поле, в котором вы храните id IMDB (если есть) по даным с этого поля будет осуществляться поиск. !!! ВАЖНО !!! у данного балансера id imdb хранится без приставок tt поэтому плагин игнорирует текст в данном поле.
4) ID видео на World Art из поля - стоит указать доп поле, в котором вы храните id WorldArt (если есть) по даным с этого поля будет осуществляться поиск.
5) Искать видео по названию - стоит указать доп поле, в котором вы храните русское название фильма. По даным с этого поля будет осуществляться поиск (возможны ошибки в проставлении для фильмов и сериалов с одинаковым названием. Для более точного поиска стоит использовать поля, описанные выше.
6) Доп. поле для вставки эмбеда - в данное доп. поле плагин будет писать ссылку на нужный материал.
7) Выберите категории фильмов/сериалов - здесь нужно указать категории и субкатегории, по которым будет работать плагин. Если не указать ничего модуль работать не будет.
8) Доступы для кнопки поска в новостях - здесь можно указать для каких категорий пользователей будет доступна кнопка "найти эмбед" в создании и редактировании новостей. Доступ будут иметь пользователи указанной категории и всех "старших" категорий пользователей.
9) Доп. поле для вставки качества видео - а это поле модуль будет выводить информацию о максимальном доступном качесве материала.
10) Доп. поле для вставки озвучек - сюда модуль будет проставлять все озвучки, доступные в материале.
11) Доп. поле для статуса новости. Значение по умолчанию должно быть - Включено!  - переключатель, который определяет работать ли модулю с конкретной новостью. Если в одной из новостей данный переключатель будет в позиции "выключено", то данная новость будет игнорироваться модулем.
12) Отображать в плеере только один указаный сезон - если активно, то модуль будет проставлять ссылку только на указанный в поле "Доп. поле для выбора сезон" сезон сериала. (Полезно, если используете посезонный вывод плееров)
13) Доп. поле для выбора сезона  - из этого доп. поля модуль будет брать информацию о сезоне сериала и использовать ее при формировании ссылки. !!! ВАЖНО !!! Если вы активировли настройку для автоподнятия новости при выходе новых эпизодов сериалов, то модуль будет менять данные этого доп. поля, проставляя номер последнего доступного сезона в базе балансера. Но только если структура сайта такова, что все сезоны сериала доступны в одной новости.
14) Доп. поле для выбора серии - из этого доп. поля модуль будет брать информацию о серии сериала и использовать ее при формировании ссылки. !!! ВАЖНО !!! Если вы активировли настройку для автоподнятия новости при выходе новых эпизодов сериалов, то модуль будет менять данные этого доп. поля, проставляя номер последнего доступного эпизода в базе балансера.
15) Текст для добавления в поле с сезонами - в данное доп. поле вы можете вписать текст, который бедт дописан в поле "Доп. поле для выбора сезона" при автоматическом обновлении номера сезона.
16) Текст для добавления в поле с серией - в данное доп. поле вы можете вписать текст, который бедт дописан в поле "Доп. поле для выбора серии" при автоматическом обновлении номера серии.
17) Доп. поле для вставки количества серий - обязательное служебное доп. поле. В него плагин будет писать информацию об общем количестве эпизодов сериала, доступных на данный момент в базе балансера.
18) Обновлять дату новости когда появится новая сери - настройка, позволяющая настроить автоматическое поднятие новости, при появлении новых эпизодов сериала в базе балансера. (Работает только если указаны доп. поля с сезоном/серией и настроен соответствующий крон).
19) Обновлять дату новости когда появится новое качество - настройка, позволяющая настроить автоматическое поднятие новости, при появлении материала в базе балансера с новым качеством. (Работает только если указаны доп. поля с сезоном/серией и настроен соответствующий крон).
20) Обновление сериалов "Не обновлять сериалы" - если активно, то модуль не будет поднимать сериалы при выходе новых эпизодов.
21) Обновление сериалов "Обновлять если у Вас все сезоны в одной новости - если активно, то модуль будет поднимать новости с сериалом и изменять данные о последней доступной серии и сезоне.
22) Обновление сериалов "Обновлять если у Вас сериал разбит по сезонам" - если активно, то модуль будет поднимать новость с последним доступным сезоном сериала, обновляя данные о последней доступной серии.
23) Опубликовать новсть на сайте, при добавлении? - Если активно, то новость будет опубликована на сайте.
24) Добавлять описание с Kinopoisk.ru в поле "Полное описание"? - если активно, то модуль будет вписывать описание материала с Kinopoisk в поле "Полное описание".
25) Поле для вставки оригинального названия - в данное поле модуль впишет оригинальное название материала.
26) Поле для вставки года выхода - в данное поле модуль впишет год выхода материала в прокат.
27) Поле для вставки рейтига на IMDB - в данное поле модуль впишет значение рейтинга imdb для материала.
28) Поле для вставки рейтига на Kinopoisk - в данное поле модуль впишет значение рейтинга kinopoisk для материала.
29) Поле для вставки рейтига на WorldArt - в данное поле модуль впишет значение рейтинга WorldArt для материала.
30) Поле для вставки постера - в данное поле модуль впишет ссылку на постер к материалу.
31) Поле для вставки стран - в данное поле модуль впишет список стран, в которых снимался материал.
32) Поле для вставки режиссеров - в данное поле модуль впишет информацию о режиссерах.
33) Поле для вставки актеров - в данное поле модуль впишет информацию об актерах.
34) Поле для вставки возраста - в данное поле модуль впишет информацию о возрасном ограничении материала.
35) Поле для вставки длительности видео - в данное поле модуль впишет информацию о длительности материала.
36) Поле для вставки премьеры (мир) - в данное поле модуль впишет дату мировой премьеры материала.
37) Поле для вставки премьеры (РФ) - в данное поле модуль впишет дату российской премьеры материала.
38) Поле для вставки трейлера - в данное поле модуль вставит ссылку на трейлер к материалу из базы балансера.
