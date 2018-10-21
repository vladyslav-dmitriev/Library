## IDE && Bash Commands
 
## Sublime Text 3

- `ctrl+shift+p` - **открыть** Install Package
- [Горячие клавиши](http://sublimetext.ru/documentation/hotkeys/windows)


### e​CSStractor
[e​CSStractor](https://packagecontrol.io/packages/eCSStractor) - компилирует `HTML` код в `CSS`/`Sass(SCSS)`

1. Выбрать `Preferences` > `Package Settings` > `eCSStractor`
2. Скопировать содержимое файла `Settings - Default` в `Settings - User`
3. Внести изменения в файл `Settings - User`, чтобы указанные настройки плагина имели следующий вид:

```javascript
- "brackets": false, // скобки {}, по умолчанию true
- "bem_nesting": true, // включить для работы BEM нотификации
- "bem.element_separator": "__", // разделитель между блоком и элементом
- "bem.modifier_separator": "_", // разделитель между элементом и модификатором
- "preprocessor.parent_symbol": "&", // символ разделителя
- "add_comments": false, // автоматически добавляет комментарии в скомпилированный код
- "comment_style": "CSS" /* комментарии имеют вид CSS SCSS */
```
`(!)` Настройки, которых нет в списке выше, не изменять.

Быстрый вызов плагина - `ctrl+shift+x`.

---

## PhpStorm

### Настройка автоматической компиляции PhpStorm

1. Переходим в настройки `File->Settings->Tools->File Watchers`
2. Выбираем нужную компиляцию (`SCSS`/`Sass`)
3. Устанавливаем `npm install sass -g`
4. Настраиваем слудующим образом:
    - Program: `sass` | `C:\Users\vladyslav\AppData\Roaming\npm\sass.cmd`
    - Arguments: `--style compressed --update $FileName$:$FileNameWithoutExtension$.css`
    
Если возникают вопросы, то вот [статья по настройке](http://mel0ne.ru/2016/06/22/compile-sass-straight-from-phpstorm/) в помощь. 

### Установка Git Bash консолью по-умолчанию в терминале PhpStorm

1. Естественно на компьютере должен быть установлен `Git` и `Git Bash`
2. Переходим в настройки `File->Settings->Tools->Terminal`
3. Меняем настройку `Shell path:`на `C:\Program Files\Git\bin\sh.exe` (подставить нужный путь)
4. Перезапустить `PhpStorm`, если команды все еще не работают

### Как сравнить два файла в PhpStorm

1. Открыть дерево проекта и выделить оба файла через `Ctrl`
2. Нажать `ПКМ`, затем `Compare Files`

### Настройка PhpStorm для вёрстки на ОС Windows + Работа с удаленными серверами
- https://habr.com/post/282003/


---

## Bash Commands

- `pwd` - отображает абсолютный путь к текущей директории (например: /c/Vladyslav/Web Development/Projects/test
)
- `ls` - отображает все файлы и директории в текущей директории
- `cd` (сокращенно от change directory):
    - `cd -` - возвращает на последний путь, с которого осуществлялся переход
    - `cd` | `cd ~` - переходит в домашнюю директорию пользователя
    - `cd ..` - переходит в родительскую директорию
    - `cd ../..` - переходит на две директории вверх (можно выходить в одной команде вверх бесконечно, например, ../../.. перейдет на три директории верх)
    - `cd /c` - путь начинающийся с `/` является абсолютным, например, `/c` перейдет в корень диска `C:`
- `cat file.txt` - отображает содержимое файла file.txt
- `file file.txt` - отображает настоящий тип файла (например для файла с расширением `md` выведет `HTML document, UTF-8 Unicode text, with CRLF line terminators`
)
- `touch file.txt` - создает файл file.txt
- `cp file1.txt file2.txt` (сокращенно от copy) - копирует файл file1.txt по пути file2.txt
- `mv file1.txt file2.txt` (сокращенно от move) - перемещает файл file1.txt по пути file2.txt
- `rm` (сокращенно от remove):
    - `rm file.txt` - удаляет file.txt
    - `rm -r directoryName` - удаляет директорию directoryName
- `makedir directoryName` (сокращенно от make directory) - создает директорию с именем directoryName
- `echo 'Hello World'` - выводит на экран консоли строку Hello World
- `exit` - позволяет завершить текущий сеанс и закрыть терминал
- `history`:
    - `history` - выводит последние 1000 использованных команд
    - `history 10` - выводит 10 последних использованных команд