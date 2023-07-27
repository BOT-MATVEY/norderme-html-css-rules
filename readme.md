# Верстка Guide

1. Скачать и установить VS Code – [https://code.visualstudio.com/](https://code.visualstudio.com/)
2. Установить расширение для VS Code: Prettier – Code Formatter
3. Установить расширения Code Spell Checker и Russian - Code Spell Checker

Базовая структура проекта

```vhdl
root
	|--public
			|---styles (папка со стилями которые используются повсеместно)
						main.scss (файл со стилями которые используются по всему сайту, прим. .button) )
						_variables.scss (файл с переменными, миксинами : цветами, миксинами шрифтов, миксином border и т.д. )
			|---images (папка с картинками)
			|---fonts (папка со шрифтами)

	|--module (пример bitrix модуля)
			module.html (файл с версткой)
			module.scss (файл со стилями)
			module.js (файл со скриптами )
	.prettierrc.json (файл с конфигурацией Prettier)
```

.prettierrc.json – файл конфигурации prettier, необходимо добавлять в корневую папку проекта

```json
{
	"printWidth": 100,
	"tabWidth": 4,
	"useTabs": true,
	"semi": true,
	"singleQuote": true,
	"embeddedLanguageFormatting": "auto",
	"singleAttributePerLine": true
}
```

## Bootstrap

Т.к. движок Bitrix основан на бутстрапе (версия 4.6), верстка сайта также должна быть основана на bootstrap’овских сетках подробнее можно изучить тут: [https://getbootstrap.com/docs/4.6/layout/overview/](https://getbootstrap.com/docs/4.6/layout/overview/)

Также в файлы необходимо подключать файл bootstrap css и скрипты bootstrap. как делать прочитать тут: [https://getbootstrap.com/docs/4.6/getting-started/introduction/](https://getbootstrap.com/docs/4.6/getting-started/introduction/)

## Сброс стандартных стилей CSS

Рекомендуется (но не обязательно), в main.scss добавить сброс дефолтных стилей

-   сброс стилей
    ```scss
    * {
    	padding: 0px;
    	margin: 0px;
    	border: none;
    }

    *,
    *::before,
    *::after {
    	box-sizing: border-box;
    }

    /* Links */

    a,
    a:link,
    a:visited {
    	text-decoration: none;
    }

    a:hover {
    	text-decoration: none;
    }

    /* Common */

    aside,
    nav,
    footer,
    header,
    section,
    main {
    	display: block;
    }

    h1,
    h2,
    h3,
    h4,
    h5,
    h6,
    p {
    	font-size: inherit;
    	font-weight: inherit;
    }

    ul,
    ul li {
    	list-style: none;
    }

    img {
    	vertical-align: top;
    }

    img,
    svg {
    	max-width: 100%;
    	height: auto;
    }

    address {
    	font-style: normal;
    }

    /* Form */

    input,
    textarea,
    button,
    select {
    	font-family: inherit;
    	font-size: inherit;
    	color: inherit;
    	background-color: transparent;
    }

    input::-ms-clear {
    	display: none;
    }

    button,
    input[type='submit'] {
    	display: inline-block;
    	box-shadow: none;
    	background-color: transparent;
    	background: none;
    	cursor: pointer;
    }

    input:focus,
    input:active,
    button:focus,
    button:active {
    	outline: none;
    }

    button::-moz-focus-inner {
    	padding: 0;
    	border: 0;
    }

    label {
    	cursor: pointer;
    }

    legend {
    	display: block;
    }
    ```

## Написание классов

Для написания классов используется методология БЭМ – где класс состоит из класса родителей в виде префиксов и класса элемента, префиксы отделяются двойным подчеркивание : “\_\_”

Пример:

```html
<div class="popup">
	<h3 class="popup__title">Заголовок</h3>
	<div class="popup__text">Текст</div>
	<button class="popup__button button__primary">Кнопка</button>

	<div class="popup__buttons-group">
		<button class="popup__buttons-group__button button__primary">Кнопка 2</button>

		<button class="popup__buttons-group__button button__primary">Кнопка 3</button>
	</div>
</div>
```

Для удобства написания стилей используется scss, стили рекомендуется писать вложено используя оператор &, это позволит держать стили одной группы в одном месте

Пример:

```scss
.popup {
	&__title {
		...
	}

	&__text {
		...
	}

	&__button {
		...
	}

	&__buttons-group {
		...

		&__buttons {
		...
		}
	}
}
```

## Переменные

Для удобства поддержки и переиспользования кодовой базы применяются переменные scss variables или встроенные css variables на усмотрение разработчика

переменные обязательны к использованию в следующих случаях:

-   указание цвета

Верно:

```scss
//_variables.scss
$clr-primary: #FFFFFF
$clr-secodary: #FEFEFE

//example.scss

.example {
	color:
	background-color: $clr-primary
}
```

Неверно

```scss
//example.scss

.example {
	color: #FEFEFE
	background-color: #FFFFFF
}
```

-   указание шрифтов (если дизайнер не использует стили для шрифтов в фигме не бойся указать на это)

За исключением случае когда шрифт используется как визуальный а не текстовый элемент, в картинке ниже + и - выступают скорее как визуальные элементы а не текстовые.

Верно:

```scss
//_variables.scss
@mixin font-main-text {
	//styleName: Основной шрифт;
	font-family: Helvetica;
	font-size: 16px;
	font-weight: 400;
	line-height: 24px;
	letter-spacing: 0em;
}

//example.scss
.example {
	@include font-main-text;
}
```

Неверно

```scss
//example.scss
.example {
	font-family: Helvetica;
	font-size: 16px;
	font-weight: 400;
	line-height: 24px;
	letter-spacing: 0em;
}
```

-   другие часто повторяющиеся стили (border, box-shadow, и др. )

```scss
//_variables.scss
@mixin border-main {
	border: 1px solid $clr-primary;
}

@mixin shadow-soft {
	box-shadow: 0px 8px 20px 0px rgba(0, 0, 0, 0.05);
}

//example.scss
.example {
	@include font-main-text;
	@include shadow-soft;
}
```

## Порядок написания стилей

1. Все стили на протяжении всего проекта должны писаться с сохранением определенного порядка. Если удобнее сначала писать стили связанные с отступами (margin, padding) а потом стили связанные с расположением детей (display) то эта структура должна сохранятся на протяжении всего проекта.
2. Стили которые отвечают за одну группу свойств должны идти одной группы. разные группы разделяются enter’ом

Верно

```scss
//example.scss
.example {
	display: flex;
	flex-direction: column;
	align-items: center;

	margin: 0 20px 0;
	padding: 10px 0;

	background-image: url("");
	background-size: contain;
	background-position: center

	color: white;
	text-transofrm: uppercase;
	text-decoration: underline;
}
```

Неверно

```scss
//example.scss
.example {
	margin: 0 20px 0;
	display: flex;
	flex-direction: column;
	background-size: contain;
	background-position: center
	text-transofrm: uppercase;
	padding: 10px 0;
	align-items: center;
	background-image: url("");
	text-decoration: underline;
	color: white;
}
```

1. Следует по максимуму избегать написания z-index и ограничиться использованием position: relative, position: absolute. в случаях когда избежать использования z-index невозможно следует применять z-index через переменные. Предлагаемый список переменных для z-index:

```scss
$zindex-dropdown: 1000;
$zindex-sticky: 1020;
$zindex-fixed: 1030;
$zindex-modal-backdrop: 1040;
$zindex-offcanvas: 1050;
$zindex-modal: 1060;
$zindex-popover: 1070;
$zindex-tooltip: 1080;
```

помимо этого можно устанавливать низкоуровневые z-index на временные состояния такие как active, hover, focus, если того требует макет. Но z-index должен быть установлен в именно на само состояние а не на класс элемента. Пример:

Верно

```scss
.tooltip {
	...
	&:hover {
	...
		z-idnex: 1
	}
}
```

Неверно

```scss
.tooltip {
	...
	z-idnex: 1
	&:hover {
		...
	}
}
```

## Обязательные общие правила

-   <img> должен содержать атрибут alt
-   <a> должен содержать href
-   id html тегов не должны повторяться

## Скрипты

выполнение скриптов должно быть обернуто в document.addEventListener(”DOMContentLoaded, () ⇒ {….}), дабы избежать ошибок когда элемент обращается к не отрендереным элементам

скрипты связанные с аналитикой и которые никак не влияют на UI или бизнес логику. будь то счетчик посещений, рекламные скрипты и т.д. должны быть вставлены с тэгом параметром async (если порядок выполнения скриптов неважен) или defer (если порядок выполнения важен)

## Компиляция SCSS в CSS

1. для компиляции можно использовать npm пакет sass и команды sass {название scss файла}:{название компилируемого css файла}
   команда sass —watch example.scss:example.css скомпилирует example.scss в файл example.css
2. Использование плагина Live Sass Compiler от Glenn Marks
