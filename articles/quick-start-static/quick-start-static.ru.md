# Быстрый старт. Собираем статическую страницу

## Описание урока

Создание простой статической страницы помогает понять, как устроен БЭМ-проект. В документе показаны основны работы с БЭМ-технологиями, уровнями переопределения и библиотеками БЭМ.

**Вы научитесь:** 

* [Клонировать БЭМ-проект](#)
* [Создавать новые странцы в проекте](#)
* [Подключать блоки сторонней библиотеки](#)
* [Создавать новые блоки и добавлять им поведение и стили](#)

В результате выполнения всех шагов вы получите страницу с полем ввода, кнопкой и фразой приветствия пользователя, как показано на рисунке ниже. Имя, введенное в поле, при нажатии на кнопку, будет отображаться в приветсвии.

![Страница приветствия](quick-start-static__hello-user.ru.png)

Для работы с примерами, описанными в документе, необходимы базовые навыки:

* HTML
* CSS
* JavaScript
* БЭМ
>
> **Важно!** В документе не рассматриваются вопросы [сборки]() БЭМ-проекта.

### Минимальные требования

Чтобы начать работу, необходимо установить:

* [Node.js 4+](https://nodejs.org)
* [Git](https://git-scm.com)

> **Важно!** Пользователям операционной системы Windows необходимо установить [Git Bash](https://git-for-windows.github.io).

### Что используем 

* Шаблонный репозиторий project-stub  
* Технологии:  
  ** BEMJSON  
  ** i-bem.js  
  ** BEMHTML  

## Клонируем БЭМ-проект

Чтобы быстро развернуть БЭМ-проект, воспользуемся локальной копией шаблонного репозитория [project-stub](https://github.com/bem/project-stub), который содержит необходимый минимум конфигурационных файлов для создания БЭМ-проекта с нуля. В project-stub по умолчанию подключены основные [БЭМ-библиотеки](https://ru.bem.info/platform/libs/):

* [bem-core](https://ru.bem.info/platform/libs/bem-core/)
* [bem-components](https://ru.bem.info/platform/libs/bem-components/)

Копию project-stub можно сделать с помощью Git.

> **Примечание.** В операционных системах OS X или Linux все команды выполняются в терминале. Пользователям Windows необходимо выполнять команды в Git Bash. Убедитесь, что Git Bash запущен от имени администратора.

Чтобы создать локальную копию project-stub, выполните следующие дейсвия:

1. Склонируйте project-stub в директорию `start-project`:

    ```bash
    git clone https://github.com/bem/project-stub.git --depth 1 start-project
    ```

2. Перейдите в директорию проекта:

    ```bash
    cd start-project
    ```

3. Установите зависимости:

    ```bash
    npm install
    ```

    > **Примечание.** Не используйте права суперпользователя `root` при установке npm-зависимостей.

4. Запустите сервер с помощью [ENB](https://ru.bem.info/toolbox/enb/):

    ```bash
    npm start
    ```

    По умолчанию сервер запустится на порте 8080.

    > **Примечание.** Если порт `8080` используется другой программой, его можно переназначить:
    >
    > ```bash
    > npm start -- -p 8081
    > ```


5.  Откройте браузер и введите адрес [http://localhost:8080/desktop.bundles/index/index.html](http://localhost:8080/desktop.bundles/index/index.html).

    Должна открыться страница с примерами блоков библиотеки bem-components:

    ![Главная страница](quick-start-static__main-page.png)

После сборки и установки всех зависимостей файловая структура проекта будет имть следующий вид: 
// или подробнее о файловой структуре проекта можно почитать тут - и направить в описание файловой структуры проекта проджект стаба
```
start-project/
    .bem
    .enb/                 # Конфигурационные файлы для сборщика ENB
    common.blocks/        # Базовые реализации блоков
    desktop.blocks/
    desktop.bundles/      # Директории бандлов проекта
    node_modules/         # Установленные модули Node (пакеты)
    .bemhint.js           # Конфигурация линтера Bemhint
    .borschik             # Конфигурация сборщика файлов Borschik
    .eslintignore         # Исключение файлов и директорий в ESLint
    .eslintrc             # Конфигурация ESLint
    .gitignore            # Исключение файлов и директорий в Git
    .stylelintrc          # Конфигурация Stylelint
    .travis.yml           # Автоматический запуск линтеров в Continuous Integration
    nodemon.json          # Конфигурация для пакета Nodemon
    package.json          # Описание проекта для npm
    README.md             # Текстовое описание проекта
```

## Создаем страницу

Диреткория `desktop.bundles` в проекте содержит файлы, полученные в результате сборки. Такие файлы в БЭМ-методологии называются [бандлами](). В простейшем случае бандлы собираются для каждой страницы, тогда одной директории бандла соответствует одна страница проекта. По умолчанию в проекте присутствует страница `index` с примерами блоков библиотеки [bem-components](https://ru.bem.info/libs/bem-components/).

> подробнее про файловую структуру проекта project-stub смотри здесь:

Чтобы добавить новую страницу:

1. Создайте директорию с именем страницы (например, `hello`) в `desktop.bundles`.
2. Создайте файл `hello.bemjson.js` в директории `desktop.bundles/hello/`.

```
start-project/
    .bem
    .enb/                    # Конфигурационные файлы для сборщика ENB
    common.blocks/           # Базовые реализации блоков
    desktop.blocks/
    desktop.bundles/
        index/               # Директория бандлов страницы index
        hello/               # Директория бандлов страницы hello
            hello.bemjson.js # Описание страницы hello
```

> Имена файлов и директорий соответствуют [соглашению по именованию]().

Файл `hello.bemjson.js` — это единственный файл в директории `desktop.bundles/hello/`, который пишется вручную. Он содержит входные данные в формате [BEMJSON](https://ru.bem.info/platform/bemjson/) и описывает структуру страницы в терминах [блоков](), [элементов]() и [модификаторов](). Остальные файлы появятся в директории `desktop.bundles/hello/` после пересборки проекта.

### Описание страницы в BEMJSON-файле

> Подробнее о BEMJSON-формате входных данных.

Чтобы создать описание страницы, необходимо представлять ее структуру. В нашем случае, на странице должен быть блок `hello`, в котором будет приветствие (элемент `greeting` блока `hello`), поле ввода (блок `input`) и кнопка (блок `button`), которые можно взять из готовой библиотеки bem-components.

Чтобы описать структуру страницы, внесите следующие изменения в файл `desktop.bundles/hello/hello.bemjson.js`:

1.  Добавьте блок `hello`.

    ```js
    ({
        block : 'page',
        title : 'hello',
        head : [
            { elem : 'css', url : 'hello.min.css' }
        ],
        scripts : [{ elem : 'js', url : 'hello.min.js' }],
        mods : { theme : 'islands' },
        content : [
            {
                block : 'hello'
            }
        ]
    })
    ```

2.  Поместите элемент `greeting` с текстом приветствия пользователя (поле `content`) в блок `hello`.

    ```js
    content : [
        {
            block : 'hello',
            content : [
                {
                    elem : 'greeting',
                    content : 'Привет, %пользователь%!'
                }
            ]
        }
    ]
    ```

3.  Добавьте блоки `input` и `button` в блок `hello`.

    ```js
    content : [
        {
            block : 'hello',
            content : [
                {
                    elem : 'greeting',
                    content : 'Привет, %пользователь%!'
                },
                {
                    block : 'input',
                    mods : { theme: 'islands', size : 'm' },
                    name : 'name',
                    placeholder : 'Введите имя'
                },
                {
                    block : 'button',
                    mods : { theme : 'islands', size : 'm', type : 'submit' },
                    text : 'Поздороваться'
                }
            ]
        }
    ]
    ```

[Полный код](https://gist.github.com/innabelaya/837a96299de6fd488223) BEMJSON-файла.

Чтобы убедиться, что на странице появились все описанные блоки и элементы, откройте страницу `hello` в браузере: [http://localhost:8080/desktop.bundles/hello/hello.html](http://localhost:8080/desktop.bundles/hello/hello.html).

## Добавляем блоки

Все блоки появляются на странице, но они по-пренему не знают ничего друг о друге и не могут взаимодействовать. 





## Оживляем блок




### Создание блока

Чтобы элементы на странице работали должным образом, необходимо прописать дополнительную функциональность блока `hello` на своем [уровне переопределения](https://ru.bem.info/methodology/key-concepts/#Уровень-переопределения).

1.  Создайте вручную каталог блока `hello` на уровне `desktop.blocks`.
2.  Разместите в нем необходимые для проекта [файлы технологий реализации блока](https://ru.bem.info/methodology/key-concepts/#Технология-реализации) (`CSS`, `JS`, `BEMHTML`). Название каталога блока и вложенных в него файлов должны совпадать с именем блока, которое прописано в BEMJSON-файле.

    * `hello.js` — описывает динамическую функциональность страниц;
    * `hello.bemhtml.js` — шаблоны для генерации HTML-представления блока;
    * `hello.css` — изменяет внешний вид объектов на странице.

### Реализация блока hello

Для представления блока в терминах БЭМ необходимо реализовать его в следующих технологиях.

#### Реализация блока в технологии JavaScript

1.  Опишите в файле `desktop.blocks/hello/hello.js` реакцию блока на действие пользователя с помощью специального свойства `onSetMod`. При нажатии кнопки в текст приветствия будет подставляться имя пользователя, введенное в поле `input`.

    JavaScript-код написан с использованием декларативного JavaScript-фреймворка — [i-bem.js](https://ru.bem.info/platform/i-bem/).

    ```js
    onSetMod: {
        'js': {
            'inited': function() {
                this._input = this.findChildBlock(Input);
                
                this._domEvents().on('submit', function(e) {
                	e.preventDefault();
                    
                    this._elem('greeting').domElem.text('Привет, ' +
                    	this._input.getVal() + '!');
                });
            }
        }
    }
    ```

2.  Используйте модульную систему [YModules](https://github.com/ymaps/modules/blob/master/README.ru.md), чтобы представить данный JavaScript-код:

    ```js
    modules.define(
        'hello', // имя блока
        ['i-bem-dom', 'input'], // подключение зависимости

        // функция, в которую передаются имена используемых модулей
        function(provide, bemDom, Input) {
            provide(bemDom.declBlock('hello', { // декларация блока
                onSetMod: { // конструктор для описания реакции на события
                    'js': {
                        'inited': function() {
                            this._input = this.findChildBlock(Input);

                            // DOM-событие, на которое будет реакция
                            this._domEvents().on('submit', function(e) {
                                // предотвращение срабатывания события по умолчанию:
                                // отправка формы на сервер с перезагрузкой страницы
                                e.preventDefault();

                                this._elem('greeting').domElem.text('Привет, ' +
                                	this._input.getVal() + '!');
                            });
                        }
                    }
                }
            }));
        });
    ```

#### Реализация блока в технологии BEMHTML

[BEMHTML](https://ru.bem.info/platform/bem-xjst/) — технология, которая преобразует входные данные из BEMJSON-файла в HTML.

1.  Напишите [BEMHTML-шаблон](https://ru.bem.info/platform/bem-xjst/templates-syntax/) и укажите в нем, что блок `hello` имеет JavaScript-реализацию.
2.  Оберните блок `hello` в форму, добавив моду `tag`.

    ```js
    block('hello')(
        js()(true),
        tag()('form')
    );
    ```

#### Реализация блока в технологии CSS

1.  Для блока `hello` создайте свои CSS-правила. Например, такие:

    ```css
    .hello
    {
        color: green;
        padding: 10%;
    }

    .hello__greeting
    {
        margin-bottom: 12px;
    }

    .hello__input
    {
        margin-right: 12px;
    }
    ```

2.  Для добавления к блоку `input` CSS правил, уже реализованных в элементе `input` блока `hello`, подмешайте элемент с помощью поля `mix` во входных данных (BEMJSON).

    ```js
    {
        block : 'input',
        mods : { theme : 'islands', size : 'm' },

        // подмешиваем элемент для добавления CSS-правил
        mix : { block : 'hello', elem : 'input' },

        name : 'name',
        placeholder : 'Имя пользователя'
    }
    ```

[Полный код](https://gist.github.com/innabelaya/045ddfb063af3b262182) BEMJSON-файла.

## Результат

Чтобы увидеть итог проделанной работы, обновите страницу:

[http://localhost:8080/desktop.bundles/hello/hello.html](http://localhost:8080/desktop.bundles/hello/hello.html)

Поскольку проект состоял всего из одной страницы, то необходимость в полной сборке отсутствует. О том, как написать более сложный проект, читайте в статье [Создаем свой проект на БЭМ](https://ru.bem.info/platform/tutorials/start-with-project-stub/).
