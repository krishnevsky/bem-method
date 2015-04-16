# FAQ

На этой странице мы собираем часто возникающие вопросы по БЭМ-методологии и отвечаем на них:

* [Почему в БЭМ используются CSS-классы, а не любой другой тип селекторов](#bem-css-class)
* [Почему не существует элементов элементов](bem-elem-elem)
* [Почему модификатор не может использоваться без указания его владельца]()
* [Почему один модификатор не может применяться одновременно и к блоку, и к элементу]()
* [В каком случае создавать блок, в каком элемент]()
* []()
* [Не нашли ответ? Задайте вопрос команде на форуме](https://ru.bem.info/forum/)

## Почему в БЭМ используются CSS-классы, а не любой другой тип селекторов

Использование CSS-классов позволяет соблюдать основные принципы методологии:

* блоки — независимые компоненты;
* блоки используются повторно;
* изменение правил одного блока не влияет правила на других блоков.

Использование CSS-классов
Опираясь на эти принципы, объясняем, почему в БЭМ не используют следующие типы селекторов:

**Почему не используем id**

Привязка к id подразумевает уникальность блока на странице. Это исключает возможность его повторного использования.

Рассматривалась возможность использовать id для блоков, которые точно будут использоваться на странице один раз, например, блок формы поиска. Запись в HTML была следующей:

*HTML*

```html
<div id="search"> ... </div>
```

*CSS*

```css
#search { ... }
```

Потом стало понятно, что нужно иметь возможность использовать повторно любой блок и при этом делать как можно меньше изменений в коде.

Так, например, форма поиска однажды появлялась внизу выдачи, чтобы было удобнее переформулировать запрос после прочтения результатов. Используя ID, пришлось бы создавать новый блок и присваивать ему уникальный идентификатор.

**Почему не используем кастомные теги**




HTML:
<div class="search"> ... верхняя форма поиска ... </div>
...
<div class="search"> ... нижняя форма поиска ... </div>

CSS:
.search { ... }


<a name="bem-elem-hierarchy"></a>

## Почему не существует элементов элементов

Неймспейсом служит только имя блока. Отражать вложенность в именах элементов не имеет смысла, так как это не позволит повторно использовать элементы независимо друг от друга.

Для выражения семантической связи достаточно указать более точное имя элемента.

*Неверно*

```css
.nav__item__link {}
```

*Верно*

```css
.nav__item-link {}
```

<a name="bem-mod-usage"></a>
## Может ли модификатор использоваться без указания его владельца? Или применяться одновременно и к блоку, и к элементу?

В методологии БЭМ модификатор не может использоваться в отрыве от своего владельца, так как это, в первую очередь, ограничит использование миксов — невозможно определить, к какому блоку или элементу относится модификатор.

*Сравни*

* блок с модификатором и другой блок

```b-menu b-menu_layout_horiz b-head-menu``` и ```_layout_horiz b-head-menu```

* блок с модификатором и другой блок с модификатором

```b-menu b-menu_layout_horiz b-head-toolbar b-head-toolbar_theme_black```
и ```_layout_horiz _theme_black```

Также не бывает такой БЭМ-сущности, как модификатор блока и модификатор элемента одновременно, поэтому следующее строковое представление будет невалидным:

'block_block-mod-name_block-mod-val__elem-name_elem-mod-name_elem-mod-val'



## Why do I need CSS classes for block instead of using semantic custom tags?

Blocks can be represented as custom tags which we may define CSS rules for. Looks like we do not need CSS classes for blocks at all. They can be used for modifiers only, like <button class="mod"/>.
Using custom tags as block selectors is indeed one of the BEMish solutions and can be used. However this variant is less flexible than the recommended "class" approach.

This is more likely that you would need to prefix modifier classes with their block name to provide them namespace. The details are uncovered in "Why the modifier CSS classes are prefixed with their parent block name?" question. So, finally the custom-tag version of a block is like <block class="block_mod"/>. This does not look very different from <div class="block block_mod"> especially assuming that being tag-independent you can use any custom node and stay with <block class="block block_mod">.

Second drawback is that "tag" version makes using the mixes of blocks impossible whereas the "class" version represent that naturally by <div class="block1 block2">.

And the last clench against such an approach is that in many cases you are not able to represent your blocks with custom tags at all. For a link block you definitely need <a> tag, and the same for <input>.

Why do I need to combine block and prefixed modifier class for a modified block?
Why does both block's and modifier's class sit together in the modified block like <div class=”block block_mod”>? Everything about a modified block can be decribed in .block_mod. If there is something common between 2 modifiers, it's possible to use preprocessor's mixins to avoid copy-paste.
This approach is possible thanks to preprocessors. However it brings some drawbacks which you should be aware of.

In case of combining 2 or more modifiers at the same block <div class="block_theme_christmas block_size_big"> you would get the core block's styles twice. However this depends on the preprocessor algorythms.

When operating modifiers dynamically with JavaScript, additional modifier is more handy. Switching it off would mean only removing one CSS class from the DOM node with no need to add the core block CSS class back as it sits there forever.


Изначально были id-шники, и для блоков (например, формы поиска) мы писали как-то так:
HTML:
<div id="search"> ... </div>

CSS:
#search { ... }


Потом мы поняли, что блоки могут повторяться на странице и нам хочется как можно меньше делать изменений при этом. Так например форма поиска однажды появлялась внизу выдачи, чтобы было удобнее перезадавать запрос по прочтении результатов. И мы приняли решение, всегда использовать классы, чтобы лишний раз не менять css, если блок будет повторяться:
HTML:
<div class="search"> ... верхняя форма поиска ... </div>
...
<div class="search"> ... нижняя форма поиска ... </div>

CSS:
.search { ... }


Как отличить элемент от блока   https://github.com/bem/bem-forum-content-ru/issues/324#issuecomment-86242078


Если хочется переиспользовать кусок кода вне контекста родителя — это точно блок.
Если кусок кода не имеет смысла без родителя — это скорее всего элемент.
Аналогией служит Shadow DOM в Web Components.

Исключением может быть ситуация, когда у такого элемента оказывается слишком богатый внутренний мир и возникает желание сделать его собственные элементы. Тогда это можно представить как служебный «приватный» блок.

