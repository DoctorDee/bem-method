# Уровни переопределения

* [Понятие уровня переопределения](#Понятие-уровня-переопределения)
* [Задачи уровней переопределения](#Задачи-уровней-переопределения)
* [Порядок использования уровней переопределения](#Порядок-использования-уровней-переопределения)
* [Примеры](#Примеры-использования-уровней-переопределения)

## Понятие уровня переопределения

**Уровень переопределения** — это директория в БЭМ-проекте, которая содержит файлы реализаций [блоков](../key-concepts/key-concepts.ru.md#Блок), [элементов](../key-concepts/key-concepts.ru.md#Элемент) и [модификаторов](../key-concepts/key-concepts.ru.md#Модификатор).

Любой БЭМ-проект состоит из уровней переопределения. Минимальное количество уровней в проекте — один, максимальное – не ограничено. 

Пример файловой структуры БЭМ-проекта с одним уровнем переопределения:

```files
project/
    common.blocks/  # уровень переопределения с блоками проекта
        header/
        footer/
```

Уровни переопределения позволяют:

* [Разделять проект на платформы](#Разделение-проекта-на-платформы)
* [Легко обновлять библиотеки блоков, подключенные в проект](#Обновление-подключенных-библиотек-блоков)
* [Использовать общие блоки для разработки разных проектов](#Разработка-проектов-в-которых-используются-общие-блоки)
* [Менять темы оформления, не затрагивая логику работы проекта](#Создание-разных-тем-оформления-проекта)  
* [Проводить эксперименты в рабочем проекте](#Эксперименты-в-рабочем-проекте)  

## Задачи уровней переопределения

Уровни переопределения решают следующие задачи:

* [добавляют новые блоки в проект](#Добавление-блоков-в-проект)
* [изменяют существующие блоки](#Изменение-реализации-блока)

### Добавление блоков в проект

Блоки с любого уровня могут использоваться в проекте без изменений.

Пример ниже показывает, как использовать кнопку из сторонней библиотеки в проекте. Для этого необходимо подключить библиотеку с блоком `button` на отдельный уровень. Копировать код блока `button` на уровень с общими проектными блоками не нужно.

Файловая структура проекта с подключенным уровнем библиотеки:

```files
project/
    common.blocks/    # уровень переопределения с блоками проекта
        header/
        logo/
    library.blocks/   # уровень переопределения c блоками библиотеки
        button/       # блок button 
```

В результате [сборки проекта](../build/build.ru.md) блок `button` будет подключен в проект: 

```css
@import "common.blocks/header/header.css";  /* header с уровня общих блоков проекта */
@import "common.blocks/logo/logo.css";      /* logo с уровня общих блоков проекта */
@import "library.blocks/button/button.css"; /* button с уровня библиотеки */
```

> Подробнее о [сборке БЭМ-проектов и порядке подключения БЭМ-сущностей в проект](../build/build.ru.md).

### Изменение реализации блока

Блоки с любого уровня можно изменять под требования проекта на другом уровне переопределения: 

<a name="redefine"></a>
* **доопределять** — добавлять новые свойства блоку;
* **переопределять** — изменить существующие свойства блока.

> **Важно** В БЭМ-проекте любая технология реализации блока можно переопределить или доопределить. Подробнее читайте в разделе [Платформа](https://ru.bem.info/platform/bem-xjst/runtime/).

Количество уровней, с которых собирается конечная реализация блока, и порядок их подключения может быть любым. Исходная реализация блока дополняется или переопределяется реализациями с каждого последующего уровня. Поэтому в сборку сначала должна попасть исходная реализация, а затем — изменения со всех уровней переопределения.

> **Важно** При переопределении или доопределении исходная реализация блока не изменяется.

Схема показывает подключение БЭМ-сущностей с разных уровней переопределения в сборку:

![схема работы уровней переопределения](https://cdn.rawgit.com/bem-site/bem-method/bem-info-data/method/redefinition-levels/redefinition-levels__levels.svg)

Пример ниже показывает, как изменить реализации блока `button` из сторонней библиотеки, которая подключена в проект как отдельный уровень (`library.blocks`): 

```files
project/
    common.blocks/    # уровень переопределения с блоками проекта
        header/
        logo/
    library.blocks/   # уровень переопределения c блоками библиотеки
        button/       # блок button 
```

Исходная CSS-реализация блока `button`:

CSS-реализация:

```css
/* Блок button в технологии CSS на уровне library.blocks */

.button {
    position: absolute;
    border: 1px solid rgba(0,0,0,.2);
    border-radius: 3px;
    background-color: #fff;
```

Отображение:

![button-default](https://cdn.rawgit.com/bem-site/bem-method/bem-info-data/method/redefinition-levels/redefinition-levels__button-default.svg)

Чтобы внести изменения, необходимо:

* Переопределить блок — изменить цвет и размер кнопки.
* Доопределить блок — добавить кнопке тень.

Для этого необходимо создать блок `button` на уровне проекта `common.blocks` и разместить в нем файл `button.css` с новыми стилями для кнопки.

Файловая структура проекта с блоком `button` на уровне `common.blocks`:

```files
project/
    common.blocks/       # уровень переопределения с блоками проекта
        header/
        logo/
        button/
            button.css   # новые правила для блока button
    library.blocks/      # уровень переопределения c блоками библиотеки
        button/          # блок button 
            button.css
            button.js
```

Новые CSS-правила:

```css
/* Блок button в технологии CSS на уровне common.blocks */

.button {
    background-color: #ffdf3a;            /* Новый цвет кнопки */
    width: 150px;                         /* Ширина кнопки */
    box-shadow: 0 0 10px rgba(0,0,0,0.5); /* Параметры тени */
}
```

В результате сборки реализация блока `button` будет состоять из исходных CSS-правил с уровня `library.blocks` и добавленных — с уровня `common.blocks`:

```css
@import "library.blocks/button/button.css";  /* Исходные CSS-правила с уровня библиотеки */
@import "common.blocks/button/button.css";   /* Особенности с уровня common.blocks*/
```

Повторяющееся свойство (`background-color`) будет переопределено (фон кнопки изменится на желтый), а новые свойства (`width` и `box-shadow`) — добавлены. Блоку `button` применится следующий набор свойств:

```css
.button {
    position: absolute;
    border: 1px solid rgba(0,0,0,.2);
    border-radius: 3px;
    background-color: #ffdf3a;             /* Новый цвет кнопки */
    width: 150px;                          /* Ширина кнопки */
    box-shadow: 0 0 10px rgba(0,0,0,0.5);  /* Параметры тени */
}
```
Новый вид кнопки:

![Переопределенная кнопка](https://cdn.rawgit.com/bem-site/bem-method/bem-info-data/method/redefinition-levels/redefinition-levels__button-redefined.svg)

В результате:

* Исходная реализация блока `button` не изменяется.
* Проектные изменения для блока `button` применятся ко всем кнопкам проекта.
* При обновлении библиотеки до новой версии, изменения блоков, которые сделаны для проекта, сохранятся на другом уровне переопределения. Если в новой версии библиотеки изменится фон кнопки или ее размеры, в проекте для блока `button` все равно применятся переопределенные правила. 

## Порядок использования уровней переопределения

В одном проекте можно настраивать разные варианты сборки: определять последовательность и множество уровней для каждого отдельного случая. Например, для каждой страницы проекта можно настроить свое используемое множество уровней. 

Пример показывает [разделение проекта на платформы по уровням переопределения](#Разделение-проекта-на-платформы). 

На схеме показана сборка проекта для разных платформ в зависимости от [юзер-агента](https://ru.wikipedia.org/wiki/User_Agent):

![Уровни переопределения](https://cdn.rawgit.com/bem-site/bem-method/bem-info-data/method/build/build__levels.svg)

## Примеры использования уровней переопределения

Наиболее распространенные способы использования уровней переопределения: 
* [Разделение проекта на платформы](#Разделение-проекта-на-платформы)
* [Обновление библиотеки блоков в проекте](#Обновление-подключенных-библиотек-блоков)
* [Разработка проектов, в которых используются общие блоки](#Разработка-проектов-в-которых-используются-общие-блоки)
* [Создание разных тем оформления проекта](#Создание-разных-тем-оформления-проекта)  
* [Эксперименты в рабочем проекте](#Эксперименты-в-рабочем-проекте)

### Разделение проекта на платформы

В проекте, поддерживающем разные платформы (например, `mobile` и `desktop`), часть кода описывает общую функциональность и часть — специфичную для каждой платформы. Чтобы не копировать общий код для реализации каждой из платформ, используются уровни переопределения. 

Общие реализации блоков для всех платформ находятся на одном уровне, например, `common.blocks`, а специфические реализации блоков для мобильных и настольный устройств на двух других: 

* `common.blocks` — общие реализации блоков для всех платформ;
* `desktop.blocks` — специфические реализации блоков для настольных устройств;
* `mobile.blocks` — специфические реализации блоков для мобильных устройств.

Пример файловой структуры проекта с разными платформами:

```files
project/
    common.blocks/
        button/
            button.css   # базовая CSS-реализация кнопки

    desktop.blocks/   
        button/
            button.css   # особенности кнопки для настольных устройств

    mobile.blocks/
        button/
            button.css   # особенности кнопки для мобильных устройств
```

При сборке в файл `desktop.bundles/bundle/bundle.css` попадут все базовые CSS-правила кнопки с уровня `common.blocks` и переопределенные правила с уровня `desktop.blocks`.

```css
@import "common.blocks/button/button.css";   /* Базовые CSS-правила */
@import "desktop.blocks/button/button.css";  /* Особенности для настольных устройств */
```

Файл `mobile.bundles/bundle/bundle.css` будет включать базовые CSS-правила кнопки с уровня `common.blocks` и переопределенные правила с уровня `mobile.blocks`.

```css
@import "common.blocks/button/button.css";   /* Базовые CSS-правила */
@import "mobile.blocks/button/button.css";   /* Особенности для мобильных устройств */
```

Разделение кода по отдельным уровням переопределения позволяет иметь одновременно разные сборки одного проекта и предоставлять необходимый вариант в зависимости от юзер-агента. 

### Обновление подключенных библиотек блоков

[Переопределение](#redefine) или [доопределение](#redefine) блоков из библиотеки на другом уровне обеспечивает сохранность изменений, сделанных для проекта, при обновлении библиотеки.

В примере библиотека подключена в проект как уровень переопределения `library.blocks`:

```files
project/
    common.blocks/     # уровень переопределения с блоками проекта
        header/
        logo/
    library.blocks/    # уровень переопределения c блоками библиотеки
        button/                          
```

Чтобы использовать кнопку из библиотеки (блок `button`) в проекте, необходимо изменить высоту кнопки с 18px на 24px. Для этого нужно переопределить блок `button` на уровне проекта:

```files
project/
    common.blocks/       # уровень переопределения с блоками проекта
        button/
            button.css   # переопределенные правила кнопки
        header/
        logo/
    library.blocks/      # уровень переопределения c блоками библиотеки
        button/          # реализация кнопки в библиотеке
```

При обновлении библиотеки, переопределенное правило блока `button` (высота 24px) сохранится, так как [переопределение не затрагивает исходную реализацию блока](#Изменение-реализации-блока) и находится на другом уровне переопределения.

### Разработка проектов, в которых используются общие блоки

Блоки, использующиеся в нескольких проектах, могут быть вынесены на отдельный уровень и подключаться при сборке.

Ниже показан пример файловой структуры проекта, где общие для двух проектов блоки вынесены на отдельный уровень `common.blocks`:

```files
projects/
    common.blocks/       # общие блоки для нескольких проектов
        button/
        input/

    project-1/           # проект 1
        button/          # переопределение блока button для проекта 1
        logo/
        modal/          

    project-2/           # проект 2
        button/          # переопределение блока b1 для проекта 2
        search/
        spin/         
```

### Создание разных тем оформления проекта

Логику работы и внешний вид проекта можно разделить на разные уровни переопределения. Это позволит создавать разные варианты оформления, переключаться между темами проекта, комбинировать стили, не изменяя поведение проекта.

В примере разные темы оформления реализованы на отдельных уровнях. Для изменения внешнего вида проекта достаточно подключить нужный уровень в сборку.

```files
project/
    common.blocks/       # общие блоки для описания бизнес-логики проекта
        button/
        input/
        ...
    alfa/                # тема оформления alfa
        button/
        input/
    beta/                # тема оформления beta
        button/
        input/
```

### Эксперименты в рабочем проекте

Уровни переопределения позволяют проводить [A/B-тестирование](https://ru.wikipedia.org/wiki/A/B-тестирование) непосредственно в рабочем проекте. Во время экспериментов код рабочего проекта не изменяется, так как каждый эксперимент находится на отдельном уровне переопределения.

Можно проводить эксперименты, меняя стили блока, его поведение или разметку страницы. Например, чтобы обеспечить доступность сайта ([a11y](https://en.wikipedia.org/wiki/Computer_accessibility)), необходимо провести эксперименты по добавлению новых тегов на страницу. Для этого на уровне эксперимента необходимо переопределить шаблоны и JavaScript-код.

Чтобы убрать из проекта неподходящий вариант, достаточно удалить директорию — уровень переопределения неудачного эксперимента.

Пример ниже показывает добавление нескольких экспериментов в файловую структуру рабочего проекта: 
* изменение аватарки пользователя (блок `user-pic`);
* изменение отступов в шапке (блок `header`);
* изменение шрифта для имени пользователя (блок `user-name`).

```files
project/                    
    common.blocks/      # блоки проекта
        header/               
        user-name/          
        user-pic/           
        ...
    exps/
        exp-1/          # уровень для эксперимента №1 
            header/     # новые отступы в шапке          
            user-name/  # новый шрифт для имени пользователя    
            user-pic/   # новый вид аватарки пользователя       
        exp-2/          # уровень для эксперимента №2
            header/     # новые отступы в шапке          
            user-name/  # новый шрифт для имени пользователя    
            user-pic/   # новый вид аватарки пользователя       
        exp-n/          # уровень для любого нового эксперимента
            header/     # новые отступы в шапке          
            user-name/  # новый шрифт для имени пользователя    
            user-pic/   # новый вид аватарки пользователя       
```

Чтобы увидеть изменения, достаточно подключить уровень эксперимента в сборку.
