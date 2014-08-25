Требует Python 3.0, под вторым не взлетит, так как некоторые модули в 3.x были переименованы. 
Сохраняет ваш диалог с пользователем, `id` которого указывается при запуске. Дополнительно предусматривает:

*   вывод списка вашего списка друзей с их `id`
*   сохранение ваших диалогов с пользователями в файл `dialogs.txt` (на случай, если беседа велась с пользователем, которго нет у вас в друзьях), содержащий имена, `id`, дату и время последнего сообщения и начало беседы - этого должно хватить, чтобы вспомнить. 

    Диалоги отсортированы по дате от более ранних к более поздним. 

    В теории скрипт написан так, чтобы вытащить все диалоги, но так как у меня менее 200 диалогов, то возможности проверить, корректно ли обрабатывается случай, если диалогов более 200, не было. 

Диалог сохраняется в XML-файл conversation_<имя_собеседника>.

Структура XML-файла:

    <conversation friend="заголовок">
        <!-- 0 – полученное сообщение, 1 – отправленное сообщение, 2 - переслано -->
        <message datetime="дата и время сообщения" 
                direction="0|1|2" 
                author="Переслано|Вы|<имя_собеседника">текст сообщения
            <attachment type="photo" 
                url="URL_изображения"/>
            <attachment type="video" 
                duration="продолжительность, сек" 
                preview="URL_изображения_предпросмотра"/>заголовок_видео
                <description>описание видео</description></attachment>
            <attachment type="audio" 
                performer="исполнитель" 
                title="название" 
                url="URL_файла"/>
            <attachment type="doc" 
                size="размер, Кб" 
                url="URL_файла" 
                ext="расширение">заголовок_документа</attachment>
            <attachment type="wall" 
                owner="автор_записи" 
                from="кто_отправил" <!-- если запись со стены другого участника -->
                date="дата и время сообщения"/>текст_записи_на_стене
            <attachments type="тип_вложения"/></attachment>
        </message>
    <conversation>

После генерации XML-файла можно воспользоваться прилагаемым примерным файлом XSLT `vk.xsl` и с помощью любого XSLT-парсера (например [Saxon](http://saxon.sourceforge.net/)) сгенерировать HTML-файл. 

Пример:

    java -jar saxon9he.jar -xsl:vk.xsl conversation_%USERNAME%.xml -o:output.html

Известные ошибки:

* не по всем `id` из файла `dialogs.txt` можно получить диалог. ВКонтакте возвращает ошибку  **113 Invalid user id**, хотя при проверке такого `id` [средствами ВКонтакте](http://vk.com/dev/users.get) оказывается, что он верен. 

Известные недостатки:

* не во всех местах реализована обработка ошибок. 
* адреса URL в тескте не конвертируются в ссылки. 
* на Windows существует проблема, описанная мной в [#tievf](http://ap-codkelden.point.im/tievf).