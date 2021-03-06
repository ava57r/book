# Игра "Угадай число"

Давайте начнём программирование с Rust, работая вместе над практическим проектом! Эта часть ознакомит вас с некоторыми базовыми концепции языка Rust, показывая их применение в реальной программе. Вы узнаете про `let`, `match`, методы, ассоциированные функции, использование внешних модулей и многое другое! Последующие части расскажут про эти идеи более детально. В этой части Вы займётесь практикой над основами.

Мы реализуем классическую программу для начинающих: угадывание числа. Вот как это работает: программа генерирует случайное целое число от 1 до 100. Затем она предлагает игроку ввести и отгадать число. Если оно больше или меньше предложенного игроком, то  программа сообщит об этом. Если игрок угадал число, то программа выведет поздравление и завершится.

## Настройка нового проекта

Для создания нового проекта, в строке терминала перейдите в папку *projects*, которую вы создали в главе 1. С помощью уже знакомой Вам утилиты `cargo` создайте новый проект:

```console
$ cargo new guessing_game
$ cd guessing_game
```

Первая команда, `cargo new` принимает в качестве первого аргумента имя нового проекта (`guessing_game`). Вторая команда изменяет текущий каталог на директорию проекта.

Посмотрите на созданный файл *Cargo.toml*:

<span class="filename">Файл: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/Cargo.toml}}
```

Если информация об авторе, полученная Cargo из вашей среды окружения является неверной, то исправьте её в файле и снова сохраните.

Как вы уже видели в главе 1, `cargo new` создаёт программу "Hello, world!". Посмотрите файл *src/main.rs*:

<span class="filename">Файл: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/src/main.rs}}
```

Теперь давайте скомпилируем программу "Hello, world!" и сразу запустим её, используя команду `cargo run`:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/output.txt}}
```

Команда `run` удобна, когда нужно быстро скомпилировать и запустить на выполнение. Например в этой игре мы будем тестировать каждую часть перед переходом к следующей.

Заново откройте файл *src/main.rs*. Весь код вы будете набирать в этом файле.

## Обработка вводимых данных

Первая часть программы игры в угадывание будет запрашивать ввод у пользователя, обрабатывать значение ввода и проверять, находится ли значение в ожидаемой форме. Для начала мы позволим игроку ввести его предположение. Введите код из Листинга 2-1 в *src/main.rs*.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:all}}
```

<span class="caption">Листинг 2-1: Программа просит ввести догадку, а потом печатает её</span>

Этот код содержит много информации, поэтому давайте рассмотрим его строка за строкой. Чтобы получить пользовательский ввод и затем распечатать результат как вывод, нам нужно перенести библиотеку `io` (ввод/вывод) в область видимости. Библиотека `io` происходит из стандартной библиотеки (известной как `std`):

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:io}}
```

По умолчанию, Rust автоматически подключает несколько типов данных в область видимости каждой программы ([*авто-импорт - prelude*](https://doc.rust-lang.org/std/prelude/index.html))<!-- <comment></comment> -->. Если типы данных, которые вы хотите использовать в программе не входят в авто-импорт, то вам нужно подключить их в область видимости явно с помощью выражения <code>use</code>. Библиотека ввода/вывода <code>std::io</code> предоставляет множество полезных функциональных возможностей, включая обработку вводимых данных пользователя.

Как вы видели в главе 1, функция `main` является точкой  начала выполнения программы:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:main}}
```

Ключевое слово `fn` объявляет новую функцию, круглые скобки `()`, показывают что у функции нет входных параметров, фигурная скобка `{` - обозначение начала тела функции.

Как вы уже узнали из главы 1, макрос `println!` выводит строку на экран:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print}}
```

Этот код печатает подсказку с указанием игры и приглашает пользователя ввести число.

### Хранение данных с помощью переменных

Далее, мы создаём место хранения введённых игроком данных:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:string}}
```

Теперь программа становится интересней. В этой маленькой строчке происходит много всего. Прежде всего здесь есть выражение `let`, которое используется для создания *переменной*. Вот другой пример:

```rust,ignore
let foo = bar;
```

В этой строке создаётся переменная с именем `foo`, которая связывается со значением из переменной `bar`. Особенностью языка Rust является то, что переменные по умолчанию неизменяемые. Мы рассмотрим эту концепцию более детально в разделе ["Переменные и изменяемость"](ch03-01-variables-and-mutability.html#variables-and-mutability)<!--  --> главы 3. Этот пример показывает, как использовать ключевое слово `mut` перед именем переменной для того, чтобы сделать переменную изменяемой:

```rust,ignore
let foo = 5; // неизменяемая
let mut bar = 5; // изменяемая
```

> Примечание: символы `//`  - синтаксис, обозначающий начало комментария, который размещается на одной строке до её конца. Всё, что размещено в строке после комментария игнорируется компилятором. Более подробно мы обсудим в главе 3.

Вернёмся к программе игры в угадайку. Теперь вы знаете, что `let mut guess` представляет собой изменяемую переменную с именем `guess`. С другой стороны от знака равенства (`=`) - это значение, которое `guess` примет, что является результатом вызова `String::new`, функции, которая возвращает новый экземпляр `String`. <a href="../std/string/struct.String.html" data-md-type="link">`String`</a><!--  --> - это строковый тип, предоставляемый стандартной библиотекой, который представляет собой расширяемый текст в кодировке UTF-8.

Синтаксис `::` в строке `::new` показывает что `new` является *ассоциированной функцией* для типа `String`. Ассоциированные функции реализуются в каком-либо типе, в данном случае в `String`, а не в экземпляре `String`. В некоторых языках это называется *статической функцией*.

Данная функция `new` создаёт новую пустую строку. Вы найдёте функцию `new` для многих типов, потому что это общее имя для функции, создающей какое-либо новое значение.

Подытожим: строка `let mut guess = String::new();` создаёт изменяемую переменную, которая в данный момент привязана к новому, пустому экземпляру типа `String`. Ух ты!

Напомним, что мы подключили функциональность ввода-вывода из стандартной библиотеки с помощью `use std::io;` в первой строчке программы. Теперь мы вызовем функцию `stdin` из модуля `io`:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:read}}
```

Если бы мы не добавили строку `use std::io` в начало программы, мы смогли бы вызвать эту функцию в коде как `std::io::stdin`. Функция `stdin` возвращает экземпляр [`std::io::Stdin`]<!--  -->, который является типом, предоставляющим обработку стандартного ввода из вашего терминала.

Следующая часть кода, `.read_line(&mut guess)`, вызывает метод <a><code data-md-type="codespan">read_line</code></a><!--<comment></comment>--> обработчика стандартного ввода для получения данных от пользователя. Мы передаём в функцию <code>read_line</code> один аргумент: <code>&mut guess</code>.

Работа функции `read_line` состоит в том, чтобы взять любые символы введённые пользователем из стандартного потока ввода и разместить эти данные в строку, которая была передана как её аргумент. Строковый аргумент должен быть изменяемым, чтобы метод мог изменить содержимое строки добавлением данных вводимых пользователем.

Символ `&` показывает, что аргумент является *ссылкой*, которая даёт возможность получить доступ к данным в нескольких местах кода без необходимости копировать эти данные в памяти несколько раз. Ссылки - это сложная особенность языка и одно из главных преимуществ Rust, безопасность и лёгкость использования ссылок. Вам не нужно знать массу деталей для завершения этой программы. В данный момент нужно знать, что ссылки по умолчанию являются неизменяемыми. Следовательно, необходимо написать `&mut guess`, а не `&guess`, чтобы сделать ссылку изменяемой. (Глава 4 объясняет ссылки более тщательно.)

### Обработка потенциальных ошибок с помощью типа `Result`

Мы все ещё работаем над этой строкой кода. Хотя сейчас мы обсуждаем третью строку, она по-прежнему является частью одной логической частью кода. Следующая часть - это метод:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:expect}}
```

Когда вы вызываете метод с синтаксисом `.foo()`, часто следует добавить перевод строки и пробелы, чтобы разбить длинные строки на логические части. Мы можем переписать этот код так:

```rust,ignore
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

Тем не менее, одна строка сложнее читается, поэтому лучшим решением будет разделить её: две строки для двух вызовов функций. Давайте теперь объясним, что эта строка делает.

Как упоминалось ранее, функция `read_line` помещает символы  пользовательского ввода в переменную переданную в неё, но она также имеет возвращаемый тип - в этом случае это [`io::Result`]<!--  -->. В стандартной библиотеке Rust имеется несколько типов с именем `Result`: обобщённый тип <a href="../std/result/enum.Result.html" data-md-type="link">`Result`</a><!--  -->, а также конкретные версии для под модулей, такие как `io::Result`.

Типы `Result` являются <a><em>перечисление</em></a><!--<comment></comment>-->, часто называемые *enums*. Перечисление имеет фиксированное множество возможных значений, которые называются <em>вариантами</em> перечисления. Глава 6 расскажет про перечисления более детально.

Для `Result` вариантами являются `Ok` или `Err`. Вариант `Ok` указывает, что операция прошла успешно, а внутри `Ok` находится успешно созданное значение. Вариант `Err` означает, что операция завершилась неудачно, а `Err` содержит информацию о том, как и почему это произошло.

Предназначение типа `Result` состоит в кодировании информации для обработки ошибки. Значения типа `Result`, как и значения любых типов, имеют определённые в них методы. Экземпляр `io::Result` имеет [метод `expect`]<!--  -->, который можно вызвать. Если экземпляр `io::Result` является значением `Err`, то метод `expect` вызовет сбой программы и покажет сообщение, которое Вы передали как аргумент в `expect`. Если метод `read_line` вернёт `Err`, это вероятно будет ошибка, происходящая от операционной системы. Если экземпляр `io::Result` будет `Ok`, `expect` возьмёт и вернёт значение содержащееся внутри `Ok`, чтобы его можно было использовать. В этом случае, значением будет число байт, которые пользователь ввёл в стандартный поток ввода.

Если Вы не вызовете `expect`, программа скомпилируется, но Вы получите предупреждение:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-02-without-expect/output.txt}}
```

Rust предупреждает, что вы не использовали значение `Result` возвращённое из `read_line`, указывая на то, что программа не обработала возможную ошибку.

Правильным способом убрать предупреждение будет обработать ошибку, но так как вы хотите чтобы программа завершилась, Вы можете использовать `expect`. Вы узнаете про восстановление после ошибок в главе 9.

### Вывод значений с помощью `println!`

Помимо закрывающих фигурных скобок присутствует ещё одна строка которую нужно обсудить:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print_guess}}
```

Эта строка выведет строку, в которую сохранён ввод пользователя. Фигурные скобки `{}` являются заполнителем: думайте о `{}` как о маленьком крабе, в клешнях которого находится значение. Вы можете вывести больше одного значения используя фигурные скобки: первые скобки держат первое значение перечисленное после форматируемой строки, вторые скобки держат второе значение, и так далее. Печать нескольких значений одним вызовом макроса `println!` выглядит так:

```rust
let x = 5;
let y = 10;

println!("x = {} и y = {}", x, y);
```

Этот код выведет `x = 5 и y = 10`.

### Тестирование первой части

Давайте протестирует первую часть игры. Запустите её используя `cargo run`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-01/
cargo clean
cargo run
input 6 -->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 6.44s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
6
You guessed: 6
```

На данный момент первая часть игры завершена: мы получаем ввод с клавиатуры и затем выводим его.

## Генерация секретного числа

Далее нам нужно создать секретное число, которое пользователь попробует угадать. Секретное число должно быть все время разным, так как в игру можно играть много раз. Давайте будем использовать случайное число от 1 до 100, чтобы не делать игру слишком сложной. Rust пока не включает случайную генерацию чисел в стандартную библиотеку. Тем не менее, команда Rust предоставляет <a>крейт <code>rand</code></a>.

### Использование крейта для получения дополнительных функций

Напомним, что крейт является собранием файлов исходного кода Rust. Проект который мы собираем является *бинарным крейтом*, который исполняемый. Крейт `rand` это *библиотечный крейт*, который содержит код, предназначенный для использования в других программах.

Использование внешних крейтов Cargo это то в чем он блистает. Перед тем как мы сможем написать код использующий `rand`, нужно изменить файл *Cargo.toml* для подключения крейта `rand` в качестве зависимости. Откройте этот файл и добавьте предложенную строку под заголовком секции `[dependencies]` созданную Cargo для Вас:

<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
* ch14-03-cargo-workspaces.md
-->

<span class="filename">Файл: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:9:}}
```

Все что следует за заголовком в файле *Cargo.toml* является частью раздела, продолжающегося до следующего заголовка. Секция `[dependencies]` указывает Cargo какие внешние крейты и какие их версии нужны в проекте. В данном случае мы пишем крейт `rand` с указанием версии `0.5.5`. Cargo понимает <a>семантическое версионирование</a><!--<comment></comment>--> (Semantic Versioning, иногда называемое <em>SemVer</em>), которое является стандартом для записи номеров версий. Число `0.5.5` является укороченной версией <code>^0.5.5</code>, которая подразумевает "любая версия которая имеет публичный API совместимый с версией <code>0.5.5.</code>".

Давайте теперь соберём наш проект без каких-либо правок кода, как показано в листинге 2-2.

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
cargo clean
cargo build -->

```console
$ cargo build
    Updating crates.io index
  Downloaded rand v0.5.5
  Downloaded libc v0.2.62
  Downloaded rand_core v0.2.2
  Downloaded rand_core v0.3.1
  Downloaded rand_core v0.4.2
   Compiling rand_core v0.4.2
   Compiling libc v0.2.62
   Compiling rand_core v0.3.1
   Compiling rand_core v0.2.2
   Compiling rand v0.5.5
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s
```

<span class="caption">Листинг 2-2: Результат выполнения <code>cargo build</code> после добавления крейта `rand` в качестве зависимости</span>

Вы можете увидеть разные номера версий (но все они будут совместимы с кодом, благодаря SemVer!), разные строки (в зависимости от операционной системы) и строки могут быть в разном порядке.

Теперь, когда у нас подключена внешняя зависимость, Cargo подгружает последние версии всех нужных зависимостей из *реестра*, который в свою очередь является копией данных с [Crates.io]. Crates.io - это ресурс, где разработчики выкладывают свои проекты на Rust для общего пользования.

После обновления вышеупомянутого реестра, Cargo проверяет секцию `[dependencies]` и загружает указанные в этой секции, но ещё не скачанные крейты. В нашем случае, несмотря на то, что мы указали лишь `rand` как зависимость, Cargo также загрузил `libc` и `rand_core`, поскольку работа `rand` напрямую зависит от этих крейтов. После скачивания всех крейтов, Rust компилирует их, и только после компилирует проект, внедряя уже скомпилированные зависимости.

Если вы сразу запустите `cargo build`, не внося никаких изменений, то не получите никаких выведенных данных, кроме строки `Finished`. Cargo знает, что он уже загрузил и скомпилировал зависимости и что вы ничего не изменили в своём файле *Cargo.toml*. Cargo также знает, что вы ничего не меняли в коде, поэтому он не перекомпилирует его. Если нечего делать, он просто завершает работу.

Если открыть файл *src/main.rs* и внести простое изменение, сохранить его и собрать снова, то вы увидите только две строки вывода:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
touch src/main.rs
cargo build -->

```console
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
```

Эти строки показывают, что Cargo пересобрал лишь файл *src/main.rs*, в который были внесены незначительные изменения. Зависимости в проекте никак не изменились, поэтому Cargo использует скомпилированные ранее зависимости при сборке проекта. Заново компилируется только код, который был изменён.

#### Обеспечение воспроизводимых сборок с помощью файла *Cargo.lock*

В Cargo есть механизм, гарантирующий возможность пересобрать тот же артефакт, когда вы или кто-то другой каждый раз собираете код: Cargo будет использовать только указанные вами версии зависимостей, пока не укажете другие. Например, что произойдёт, если на следующей неделе выйдет крейт `rand` с версией 0.5.6, содержащей исправление важной ошибки, но который также содержит регрессию, которая сломает ваш код?

Ответом на эту проблему является файл *Cargo.lock*, который создаётся впервые при запуске `cargo build` и потом находится в вашем каталоге *guessing_game*. Когда вы впервые собираете проект, Cargo выясняет все версии зависимостей, которые соответствуют критериям, а затем записывает их в файл *Cargo.lock*. Когда в будущем вы будете собирать проект, то Cargo увидит, что файл *Cargo.lock* существует и будет использовать указанные там версии вместо того, чтобы делать всю работу по выяснению версий снова. Это позволяет иметь воспроизводимую сборку автоматически. Другими словами, ваш проект будет оставаться на уровне версии `0.5.5` благодаря *Cargo.lock* файлу, пока не обновите версию явно.

#### Обновление крейта для получения новой версии

Когда вы *хотите* обновить крейт, Cargo предоставляет другую команду, `update`, которая проигнорирует файл *Cargo.lock* и выяснит все последние версии, которые соответствуют вашим спецификациям из *Cargo.toml* файла. Если это работает, Cargo запишет эти версии в файл *Cargo.lock*.

Но по умолчанию Cargo будет искать только версии больше `0.5.5` и меньше, чем `0.6.0`. Если `rand` крейт выпустил две новые версии, `0.5.6` и `0.6.0`, вы увидите следующее при запуске команды `cargo update`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
cargo update
assuming there is a new 0.5.x version of rand; otherwise use another update
as a guide to creating the hypothetical output shown here -->

```console
$ cargo update
    Updating crates.io index
    Updating rand v0.5.5 -> v0.5.6
```

В этот момент вы также заметите изменение в файле *Cargo.lock*, заметив что версия крейта `rand`, которую вы теперь используете является `0.5.6`.

Если вы вдруг захотите использовать `rand` версии `0.6.0` или любой другой версии в серии `0.6.x`, то в файле *Cargo.toml* нужно править следующим образом:

```toml
[dependencies]
rand = "0.6.0"
```

При следующем запуске `cargo build`, Cargo обновит реестр доступных крейтов и пересоберет проект с новыми требованиями относительно версии крейта `rand`.

Можно много рассказать про [Cargo]<!--  --> и [его экосистему]<!--  --> которые мы обсудим в главе 14, сейчас это все что вам нужно знать. Cargo позволяет очень легко повторно использовать библиотеки, поэтому Rust разработчики имеют возможность писать меньшие проекты, которые скомпонованы из многих пакетов.

### Генерация случайного числа

Теперь, после добавления крейта `rand` в *Cargo.toml*, давайте начнём использовать `rand`. Следующим шагом является обновление *src/main.rs*, как показано в листинге 2-3.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:all}}
```

<span class="caption">Листинг 2-3: Добавление кода для генерации случайного числа</span>

Сначала, добавим `use` строку вида: `use rand::Rng`. Типаж `Rng` определяет методы, которые реализуют генератор случайных чисел, и этот типаж должен быть в области видимости, чтобы использовать его методы. Глава 10 подробно расскажет об особенностях.

Далее добавляем две строки по середине. Функция `rand::thread_rng` предоставит генератор случайных чисел, который мы собираемся использовать: тот, который является локальным для текущего потока выполнения и инициализирован операционной системой. Затем мы вызываем метод `gen_range` у генератора случайного числа. Этот метод объявлен в типаже `Rng`, который мы импортировали в область действия оператором `use rand::Rng`. Метод `gen_range` принимает два числа в качестве аргументов и генерирует случайное число в их диапазоне. Он включает нижнюю границу, но исключает верхнюю границу, поэтому нам нужно указать `1` и `101`, чтобы запросить число от 1 до 100.

> Примечание: вы не будете знать, какие типажи использовать, какие методы и функции вызывать из крейта. Инструкции по использованию крейта есть в каждой документации. Ещё одна полезная особенность Cargo - то, что вы можете запустить команду `cargo doc --open`, которая соберёт локальную документацию, предоставленную всеми зависимостями и откроет её в браузере. Если вы заинтересованы в других функциях крейта `rand`, например, запустите  `cargo doc --open` и нажмите `rand` на боковой панели слева.

Вторая строка, добавленная в середину кода, печатает секретное число. Это полезно, пока мы разрабатываем программу в целях протестировать, но мы удалим её из окончательной версии. Это не совсем игра, если программа выводит ответ при запуске!

Попробуйте запустить программу несколько раз:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-03/
cargo run
4
cargo run
5
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 7
Please input your guess.
4
You guessed: 4

$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 83
Please input your guess.
5
You guessed: 5
```

Вы должны получить разные случайные числа, и все они должны быть числами между 1 и 100. Отличная работа!

## Сравнение догадки с секретным числом

Теперь, когда у нас есть пользовательский ввод и случайное число, можно сравнить их. Этот шаг показан в листинге 2-4. Обратите внимание, что этот код ещё не компилируется, мы объясним.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-04/src/main.rs:here}}
```

<span class="caption">Листинг 2-4. Обработка возможных возвращаемых значений сравнивая два числа</span>

Первый новый код здесь - это ещё один оператор `use`, импортирующий в область видимости тип с именем `std::cmp::Ordering` из стандартной библиотеки. Как `Result`, тип `Ordering` - это ещё одно перечисление, но вариантами для `Ordering` являются `Less`, `Greater` и `Equal`. Это три результата, которые возможны при сравнении двух значений.

Затем мы добавляем пять новых строк внизу, которые используют тип `Ordering`. Метод `cmp` сравнивает два значения и может быть вызван на чем угодно, что можно сравнивать. Метод принимает ссылку на то, с чем вы хотите сравнить: здесь мы сравниваем `guess` с `secret_number`. Затем он возвращает вариант `Ordering` перечисления присутствующий в области видимости благодаря `use`. Используется [`match`]<!--  --> выражение для решения, что делать дальше на основе полученного варианта `Ordering` из вызова `cmp` со значениями в `guess` и `secret_number`.

Выражение `match` состоит из *веток*. Ветка состоит из *шаблона* и кода, который должен быть выполнен, если значение заданное в начале выражения `match` соответствует образцу этой ветки. Rust берет значение, указанное для `match` и просматривает каждую ветку по очереди на совпадение. Конструкция `match` и шаблоны являются мощными функциями в Rust, которые позволяют вам выразить различные ситуации которые могут встретиться в коде и убедится, что они все обработаны. Эти особенности будут подробно рассмотрены в главе 6 и главе 18 соответственно.

Давайте разберём пример того, что случилось бы с использованным здесь выражением `match`. Скажем, пользователь угадал 50 и случайно сгенерированное секретное число на этот раз равно 38. Когда код сравнивает 50 с 38, метод `cmp` вернёт `Ordering::Greater`, потому что 50 больше 38. Выражение `match` получает значение `Ordering::Greater` и начинает проверять каждую ветку шаблонов. Оно смотрит на шаблон первой ветки, `Ordering::Less`, и видит, что значение `Ordering::Greater` не соответствует `Ordering::Less`, поэтому игнорируется код в этой ветке и переходит к следующей ветке. Образец следующей ветки, `Ordering::Greater`, шаблон *совпадает* с `Ordering::Greater`! Связанный с веткой код выполняется и напечатает `Too big!` на экран. Выражение `match`  заканчивается, потому что нет необходимости смотреть на последний рукав в этом сценарии.

Однако код в листинге 2-4 ещё не компилируется. Давайте попробуем:

```console
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-04/output.txt}}
```

Основная ошибка утверждает, что произошло *несовпадение типов*. Rust имеет строгую, статическую систему типов. Тем не менее, в нем также есть выведение типов. Когда мы написали `let mut guess = String::new()`, Rust смог вывести, что `guess` должно быть типа `String` и нам не пришлось писать тип. Но переменная `secret_number` является числовым типом. Несколько числовых типов могут иметь значение от 1 до 100: `i32` - 32-битное знаковое число; `u32` - беззнаковое 32-битное число; `i64` - знаковое 64-битное, а также другие. По умолчанию Rust определяет тип `i32`, которое является типом у `secret_number` до тех пор, пока вы где-нибудь не добавите информацию о типе, что заставить Rust вывести другой числовой тип для переменной. Причина ошибки к том, что Rust не может сравнить строковый и числовой тип.

В конечном счёте, мы хотим преобразовать считываемые программой `String` из стандартного ввода в тип действительное число, чтобы было можно сравнивать его в числовом виде с загаданным числом. Можно это сделать, добавив следующие две строки в тело функции `main`:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/src/main.rs:here}}
```

Строка такая:

```rust,ignore
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

Мы создаём переменную с именем `guess`. Но подождите, разве в программе нет уже переменной с именем `guess`? Да это так, но Rust позволяет нам *затенять* предыдущее значение `guess` с помощью нового. Эта возможность часто используется в ситуациях где вы хотите преобразовать значение из одного типа в другой тип. Затенение позволяет повторно использовать имя переменной `guess`, а не заставлять нас создавать две уникальных переменных, например вроде `guess_str` и `guess`. (Глава 3 охватывает затенение более подробно.)

Мы связываем `guess` с выражением `guess.trim().parse()`. Переменная `guess` в выражении ссылается на исходную переменную `guess` которая была типа `String` при вводе данных. Метод `trim` на экземпляре `String` удалит все пробелы в начале и конце. Хотя `u32` может содержать только числовые символы, пользователь должен нажать <span class="keystroke">Enter</span>, чтобы удовлетворить метод `read_line`. Когда пользователь нажимает <span class="keystroke">Enter</span>, символ новой строки добавляется в строку. Например, если пользователь вводит <span class="keystroke">5</span> и нажимает <span class="keystroke">Enter</span>, `guess` выглядит так: `5\n`. Символ `\n` представляет символ "новая строка" как результат нажатия <span class="keystroke">Enter</span>. Метод `trim` исключает `\n`, в результате получаем `5`.

Метод [`parse` у строк](https://doc.rust-lang.org/std/primitive.str.html#method.parse)<!--<comment> </comment>--> разбирает строку в число некоторого типа. Поскольку этот метод может анализировать различные типы чисел, то нужно указать точный тип числа, который мы хотим получить с помощью `let guess: u32`. Двоеточие (`:`) после `guess`, говорит Rust что мы аннотировали тип переменной. Rust имеет несколько встроенных числовых типов; здесь вы видите `u32` являющийся 32-битным без знаковым целым числом. Это хороший выбор по умолчанию для небольшого положительного числа. Вы узнаете о других типах чисел в главе 3. Кроме того, аннотация `u32` в этом примере программы и сравнение с `secret_number` означает, что Rust выведет что `secret_number` должен иметь тип <code>u32</code>. Так что теперь сравнение будет между двумя значениями одинакового типа!

Вызов метода `parse` может легко вызвать ошибку. Если, например, строка содержит `A👍%`, то нет никакого способа преобразовать его в число. Так как вызов может дать сбой, то метод `parse` возвращает тип `Result`, так же как `read_line` метод (обсуждался ранее в <a href="#handling-potential-failure-with-the-result-type" data-md-type="link">"Обработка потенциального сбоя с помощью `Result` типа"</a><!--  -->). Мы будем обрабатывать этот `Result` снова, используя метод `expect`. Если `parse` возвращает вариант `Err` перечисления `Result`, потому что он не может создать число из строки, то вызов `expect` приведёт к сбою игры и распечатает сообщение, которое мы передали в него. Если `parse` сможет успешно преобразовать строку в число, он вернёт вариант `Ok` перечисления `Result`, а `expect`  вернёт число, которое мы хотим от значения `Ok`.

Давайте запустим программу сейчас!

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/
cargo run
  76
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!
```

Хорошо! Несмотря на то, что были добавлены пробелы перед предположением, программа все равно вывела пользовательское предположение 76. Запустите программу несколько раз для проверки разного поведение с разными видами ввода: предположение правильное, предположение слишком велико и предположение слишком мало.

Большая часть игры сейчас работает, но пользователь может сделать только одно предположение. Давайте изменим это, добавив цикл!

## Разрешение нескольких догадок с помощью цикла

Ключевое слово `loop` создаёт бесконечный цикл. Мы добавим его сейчас, чтобы дать пользователям больше шансов угадать число:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-04-looping/src/main.rs:here}}
```

Как видите, мы переместили весь код вывода подсказки и ввода догадки в цикл. Обязательно сделайте отступ в строках внутри цикла по четыре пробела на каждой строке и снова запустите программу. Обратите внимание, что есть новая проблема, потому что программа выполняет именно то, что мы ей написали: запрашивает новое число догадки до бесконечности! Похоже, что пользователь не сможет выйти!

Пользователь всегда может прервать программу, используя сочетание клавиш <span class="keystroke">ctrl-c</span>. Но есть ещё один способ избежать этого, как упоминалось в обсуждении `parse` раздела ["Сравнение догадки с секретным номером"](#comparing-the-guess-to-the-secret-number)<!--  -->: если пользователь вводит не числовой ответ, программа завершится сбоем. Пользователь может воспользоваться этим, чтобы выйти, как показано здесь:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-04-looping/
cargo run
(too small guess)
(too big guess)
(correct guess)
quit
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.50s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 59
Please input your guess.
45
You guessed: 45
Too small!
Please input your guess.
60
You guessed: 60
Too big!
Please input your guess.
59
You guessed: 59
You win!
Please input your guess.
quit
thread 'main' panicked at 'Please type a number!: ParseIntError { kind: InvalidDigit }', src/main.rs:28:47
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Ввод `quit` в самом деле завершит игру, как и любой другой ввод не являющийся числом. Однако это не оптимально, если не сказать больше. Мы хотим, чтобы игра автоматически остановилась, когда будет угадан правильный номер.

### Выход после правильной догадки

Давайте запрограммируем игру на выход при выигрыше пользователя, добавив оператор `break`:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-05-quitting/src/main.rs:here}}
```

Добавление линии `break` после появления `You win!` заставляет программу выходить из цикла, когда пользователь угадывает секретный номер правильно. Выход из цикла также означает выход из программы, потому что цикл является последней частью `main`.

### Обработка неверного ввода

Для дальнейшего улучшения поведения игры вместо аварийного завершения программы при вводе пользователем не числовых значений, давайте заставим игру игнорировать не числовые символы, поэтому пользователь может продолжать угадывать. Мы можем сделать это, изменив строку, где `guess` преобразуется из `String` в `u32`, как показано в листинге 2-5.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-05/src/main.rs:here}}
```

<span class="caption">Листинг 2-5. Игнорирование догадки не являющейся числом и запрос другого предположения вместо сбоя программы</span>

Переключение с вызова `expect` на вызов выражения `match` как правило является способом перейти от сбоя при ошибке к обработке ошибки. Помните, что `parse` возвращает тип  `Ok`, а `Result` - это перечисление с вариантами `Ok` или `Err`. Мы здесь используем выражение `match`, как мы это делали с результатом `Ordering` метода `cmp`.

Если `parse` может успешно превратить строку в число, он вернёт значение `Ok`, которое содержит полученное число. Значение `Ok` будет соответствовать шаблону первой ветки и выражение `match` просто вернёт `num` значение, которое возвращает метод `parse` и поместит это значение внутрь `Ok` перечисления. Это число окажется в нужном месте, в новой созданной переменной `guess`.

Если метод `parse` *не способен* превратить строку в число, он вернёт значение `Err`, которое содержит более подробную информацию об ошибке. Значение `Err` не совпадает с шаблоном `Ok(num)` в первой ветке `match`, но совпадает с шаблоном `Err(_)` второй ветки. Подчёркивание `_` является вариантом для любых других случаев в этом примере, мы даёт указание что хотим обработать совпадение всех значений `Err`, не важно какая информация находится внутри. Поэтому программа будет выполнять вторую ветку кода `continue`, которая выполняет переход программы на следующую итерацию цикла `loop` и спрашивает следующий вариант для угадывания. Таким образом программа игнорирует все ошибки метода `parse`, которые могут встретится!

Теперь все в программе должно работать как положено. Давай попробуем:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-05/
cargo run
(too small guess)
(too big guess)
foo
(correct guess)
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 4.45s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 61
Please input your guess.
10
You guessed: 10
Too small!
Please input your guess.
99
You guessed: 99
Too big!
Please input your guess.
foo
Please input your guess.
61
You guessed: 61
You win!
```

Потрясающе! Одним крошечным финальным изменением мы закончим игру в угадывание. Напомним, что программа всё ещё печатает секретное число. Это хорошо сработало для тестирования, но испортило игру. `println!` который выводит секретный номер. В листинге 2-6 показан окончательный код.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-06/src/main.rs}}
```

<span class="caption">Листинг 2-6: Полный код игры угадывания числа</span>

## Итоги

На данный момент вы успешно создали игру угадай число. Поздравляем!

Этот проект был практическим способом познакомить вас со многими новыми концепциями Rust: `let`, `match`, методы, ассоциированные функции, использование внешних крейтов и другое. В следующих нескольких главах вы узнаете про эти понятия более подробно. Глава 3 охватывает концепции, которые есть у большинства языков программирования, такие как переменные, типы данных и функции, а также показывает их использование в Rust. Глава 4 исследует владение, особенность, которая отличает Rust от других языки. В главе 5 обсуждаются структуры и синтаксис методов, а в главе 6 объясняет, как работают перечисления.


[`std::io::Stdin`]: ../std/prelude/index.html
[`io::Result`]: ../std/string/struct.String.html
[метод `expect`]: ../std/io/struct.Stdin.html
[Crates.io]: ../std/io/struct.Stdin.html#method.read_line
[Cargo]: ../std/io/type.Result.html
[его экосистему]: ../std/result/enum.Result.html
[`match`]: ch06-00-enums.html