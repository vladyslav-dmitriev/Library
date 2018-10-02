# Sublime Text 3

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