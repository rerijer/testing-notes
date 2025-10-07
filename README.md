# testing-notes
Notes and articles about software testing, QA theory, and practical cases for beginners.
Every beginner in QA will face writing their first test case. To do this, you need to understand what a test case is, what its structure is, where you write it, and what example to use.

Let’s start from the very beginning. Since we are just learning, we don’t have a specific assignment, so let’s make one up ourselves. We need to check if something works as expected. The easiest option, in my opinion, is a registration form. Here, you can check every field and write both positive and negative test cases. I chose the Cisco registration form. That’s just the first thing I remembered :)

Okay, let’s come up with some tasks. For beginners, it is enough to pick one important function and write two types of checks — positive for correct data and actions, and negative for incorrect data and mistakes. To choose a topic, start with an obvious goal from requirements or a user story: what does the user want to do, and what result counts as success?

Now we need to come up with a set of checks. Let’s find equivalence classes, define boundary values, check them on both sides (on the boundary — positive, just outside — negative), and add negative scenarios for wrong formats, empty fields, too-long values, and wrong steps order.

Let’s break it all down step by step. Equivalence classes are groups of input values that the system handles in the same way; it’s enough to check just one example from each group to cover the whole group. This saves test time and helps balance positive and negative checks. For example, if you sort apples by size S, M, L, you don’t have to check every apple — you check one S, one M, one L, because all apples of the same size class are supposed to be the same. In testing, equivalence classes work exactly the same way: one value represents a whole group.

How to find these classes:

Use a rule from the requirements: format, range, mandatory, allowed symbols.

Split values into valid and invalid groups, where the system reacts differently.

Split further only if the system reacts differently (different message, different screen).

Example for email: valid addresses. Invalid subtypes — missing “@”; two “@”; empty; no domain; spaces inside; too long; forbidden characters; invalid Unicode. Take one example for each subtype.

Example for age with a rule 18–65: classes — less than 18; 18 to 65; more than 65. Representatives: 17 (negative), 30 (positive), 66 (negative).

Boundaries are the “edges” between classes, where there are often bugs: for age, this is 17/18 and 65/66. For positive tests, use values exactly at the boundary (18, 65), for negative — just outside (17, 66). This gets you the most critical bugs with the fewest tests.

Here are a few tips:

One case — one group, one representative, to keep results clear.

Choose “typical” representatives, not just the extremes — use the middle value in valid cases.

For invalids, make separate subtypes if error messages or handling are different.

To start, just to get what a test case is, we’ll use:

Successful registration with correct values (positive)

Password show/hide toggle check (positive)

Error for empty required fields (negative)

Error for wrong email format (negative)

So, we have two positive and two negative test cases. Here’s what they are:
A test case is a step-by-step scenario for a specific requirement or function with clear inputs and an expected result. Thanks to this “instruction,” anyone can repeat the test and get an obvious Pass or Fail.

Let’s talk about positive and negative types. A positive test case checks that the system works as required with valid data. Negative test cases check stability and correct error handling when something is wrong or the user does something wrong. Both are needed: the first confirms things are fine, the second confirms the system breaks safely. Here is a small table for clarity:

Criterion|Positive Test Case|	|Negative Test Case

Goal	   |Confirm proper work|	|Confirm proper error handling

Data	   |Valid	             | |Invalid/prohibited


Result	 |Success	           | |Error/block, no crash

Use	     |Common, main paths | |Edge, error, hacking


Typical mistakes:

Mixing several checks into one case instead of atomic (one case, one check).

Unclear steps or expectations, missing specific test data (everything should be clear for anyone).

Dependence on other test case results and fragile preconditions (each case should be independent).

That’s enough to start. Once you’ve picked a topic and understand the idea, let’s write the case.

Where do you actually write these test cases? The choice depends on need for traceability, planning, launch and reporting, Jira integration, bug trackers, and code reviews. If you’re just starting, Jira+Xray or Zephyr is practical; for classic test management — TestRail; for trying AI suggestions and test generation — modern tools like Qase or Testsigma, etc.

But don’t worry about that for now! We’ll go through it later. For now, a regular notepad is enough. Start simple.

Structure. Here’s a template for a simple, basic test case:

ID: <unique identifier>

Title: <short test goal>

Link: <requirement/story/design ID>

Preconditions: <what must be ready before start>

Environment: <browser/platform/locale/build>

Test data: <test values and accounts>

Steps:
  1) ...
  2) ...

Expected result: <clear outcome>

Actual result: <filled in after you run the test>

Status: Not Run / Pass / Fail / Blocked

Priority: High / Medium / Low

Notes: <logs, screenshots, version, artifacts>

—

Let’s start. First test case: Successful registration with correct values.
First, we write the ID: TC-CISCO-REG-001. Then title. Next, the preconditions (registration page open, EN locale). Environment is browser. Next, the data we’ll enter for each field. Then steps — clear and easy to follow. Expected result — after our check, registration is successful, and we go to the next step.

Why don’t I have the other fields in my test case examples? Because these are attributes filled during test execution in the test management system, not at the time of case writing. In study cases, they’re often skipped to focus on the structure and expected results. Actual result is often filled in a bug report if a test fails, so these fields may be missing in basic examples.

First positive case:

ID: TC-CISCO-REG-001

Title: Successful registration with valid data

Preconditions: Registration page is open; locale = EN

Environment: Web

Test data: Email=test.user+qa@example.com, Password=strongPass@123, First name=Ann, Last name=Ivanova, Country=Russia

Steps: 1) Fill all required fields with valid data; 2) Click "Register".

Expected result: Registration is accepted and the user is moved to the next step (email verification)


Second positive case:

ID:TC-CISCO-REG-002

Title: Password visibility toggle works as expected

Preconditions: Registration page is open

Environment: Web

Test data: Password=strongPass@123

Steps: 1)Enter a password; 2)Click the eye icon to show; 3)Verify characters become visible; 4)Click again to hide.

Expected result: Toggling switches visibility without clearing or corrupting the value.


First negative case:

ID: TC-CISCO-REG-101

Title: Required fields empty – form is not submitted

Preconditions: Registration page is open; locale = EN

Steps: 1) Leave Email, Password, First name, Last name empty and do not select Country/Region; 2) Click "Register".

Expected result: Submission is blocked; each required field shows a clear English error message ("We found some errors. Please review the form and make corrections.")


Second negative case:

ID: TC-CISCO-REG-102

Title: Email format validation

Preconditions: Registration page is open

Test data: Email=user@@example; other fields valid

Steps: 1) Enter an invalid email format; 2) Fill other required fields; 3) Click "Register".

Expected result: A clear error near the Email field ("This value is not a valid email address")


—

Summary – some reminders for every beginner:

One test case = one check. It’s much easier to keep things together, find bugs, and learn logic step by step. Write every step so clearly that anyone can understand what you mean.

Show all test data (email, password, etc.) in the case – always give a real example, never just “something correct”.

Each case must have just one clear expected result. This way, you know quickly if the test passed or failed, and spend no time on confusion.

Write positive and negative tests separately. Never mix both in one case; it’s much easier to track what went wrong.

Statuses like “Pass”, “Fail”, “Blocked” and “Actual result” are filled only after running the test. Before launch, they stay empty or say “Not Run”.

Set priorities (High/Medium/Low) before testing — first, do the most important and risky scenarios, those that affect the customer or the company’s money.

—

Mini-FAQ:

Why do you need negative tests?

They help find bugs before the user does. Negative tests check that the system responds kindly to wrong or strange data, not crashing or losing information.


How many tests do you need for one field?

Easiest: one for every “different value type” (equivalence class). Ex: one for a proper email, one for empty, one for too long, one with odd characters, one with Cyrillic, etc.


Why are there so few cases in study examples?

Examples just show the basics. In real life, there are usually more test cases, especially when there are clear requirements, specs, and real bugs.


Now you’re ready to go further in learning QA.
-------

Каждый начинающий специалист столкнется с первым тест-кейсом. Чтобы его написать, нужно понимать, как он хотя бы выглядит, какая у него структура, где его писать и что брать за пример.
Начнем с самого начала. Так как мы еще только учимся, у нас нет конкретного задания, поэтому мы придумает его сами. Нам нужно что-то проверить на его работоспособность. Самым простым и понятным, на мой взгляд, является окно регистрации. Здесь можно проверить каждое поле, составить негативные и позитивные тесты. Я выбрала окно регистрации Cisco. Это было первое, что я вспомнила).

Так, хорошо. Теперь придумаем себе задачи. Начинающим достаточно выбрать важную функцию и описать два типа проверок — позитивные для правильных данных и действий, и негативные для неправильных данных и ошибок. Чтобы выбрать тему, начнем с явной цели из требований или пользовательской истории: что пользователь хочет сделать и какой результат считается успешным. 
Теперь нужно собрать набор проверок. Выделим классы эквивалентности, определим граничные значения и проверим их с двух сторон (на границе (позитив) и за границей (негатив)), добавим негативные сценарии для неверных форматов, пустых полей, слишком длинных значений и нарушений порядка действий.

Давай все потихоньку разбирать. Эквивалентные классы — это группы входных значений, которые система обрабатывает одинаково; достаточно проверить по одному представителю из каждой такой группы, чтобы сделать вывод о всей группе. Это сокращает число тестов без потери смысла и помогает сбалансировать позитивные и негативные проверки. Если сортировать яблоки по размеру S, M, L, то для контроля качества не требуется проверять каждое яблоко; достаточно взять по одному из S, M, L, потому что все внутри класса ожидаются одинаковыми по правилам сортировки. В тестировании эквивалентные классы работают точно так же: один представитель заменяет множество схожих значений. Как выделять эти классы:
1.	Взять правило из требований: формат, диапазон, обязательность, состав символов.
2.	Разделить на валидные и невалидные группы, где результат или ответ системы различается.
3.	Дальше дробить только там, где поведение действительно разное: другой алгоритм, другой экран, другое сообщение об ошибке.

Например Email: валидные адреса. Невалидные подтипы — без «@», две «@», пусто, без домена, пробелы внутри, длина сверх лимита, запрещённые символы, неподдерживаемый Unicode. В каждом подтипе берётся один представитель. Возраст с правилом 18–65: классы — меньше 18; от 18 до 65; больше 65. Представители: 17 (негатив), 30 (позитив), 66 (негатив).
Границы — это «стыки» между классами, где чаще всего возникают ошибки: для возраста это 17/18 и 65/66. Для позитивных тестов берутся значения на допустимой границе (18, 65), для негативных — сразу за ней (17, 66). Такое сочетание даёт малым числом тестов высокий шанс поймать дефекты.

Чтобы все сделать правильно, вот пару советов:
1.	Один кейс — одна группа и один представитель, чтобы результаты были однозначными.
2.	Представители выбирать «типичными»: не только экстремумы, но и середину валидного класса.
3.	Для невалидных значений создавать отдельные подтипы, если сообщения об ошибке или обработка различаются.

Для начала, чтобы хотя бы понять, что из себя представляет тест-кейс, выберем такие: 
1.	Успешная регистрация с правильными значениями;
2.	Проверка значка показать/скрыть пароль;
3.	Ошибка для пустых обязательных полей;
4.	Ошибка при вводе некорректного email.
Мы выбрали два позитивных и два негативных кейса. Немного введу в курс дела, что это такое. 

Сам тест-кейс – это пошаговый сценарий конкретного требования или функции с четкими входными данными и ожидаемым результатом. Благодаря этой «инструкции», любой специалист может воспроизвести тест и получить однозначный вывод Pass/Fail.

Теперь поговорим о позитивных и негативных видах. Первый нам нужен для проверки того, что система корректно выполняет требуемый сценарий при валидных данных. Негативные тест-кейсы проверяют устойчивость и корректную обработку ошибок при невалидных данных или некорректных действиях. Они оба необходимы: первые подтверждают, что все ок, работает как задуманно, а вторые – безопасно и корректно ломается. В табличке я представила их различия для большего понимания.

Критерий:	Позитивный тест-кейс ||	Негативный тест-кейс

Цель:	Подтвердить корректную работу	|| Подтвердить корректную обработку ошибки

Данные:	Валидные ||	Невалидные/запрещённые

Результат:	Успех операции ||	Ошибка/блокировка без аварий

Применение:	Основные, частые пути ||	Краевые, ошибочные, злоумышленные


Чтобы все сделать правильно, давай разберем типичные ошибки:
1.	Смешение нескольких проверок в одном кейсе вместо атомарности (один тест для одной проверки). 
2.	Неоднозначные шаги и ожидания, отсутствие конкретных тест данных (все должно быть четенько, чтобы любой специалист смог понять).
3.	Зависимость от результата других кейсов и хрупкие предусловия (каждый тест независим друг от друга).

Для самого-самого начала думаю хватит. После того, как мы определились с темой и поняли, что это такое, переходим к написанию.

Где вообще пишут эти ваши тест-кейсы. Выбор определяется потребностью в трассируемости, планировании и запусках, отчётности/дашбордах и интеграциях с Jira, баг‑трекером и CI, а также возможностями параметризации/ревью кейсов. Если стартовать без инфраструктуры, практичен Jira+Xray или Zephyr; для классического управления — TestRail; для экспериментов с ИИ‑подсказками и автогенерацией — современные TMS вроде Qase/Testsigma и др.. 

Но давай пока об этом не будем думать. Мы это разберем в дальнейшем. А пока просто блокнот. Начнем с простого.
Итак, структура. Шаблон для простенького, самого обычного тест-кейса выглядит вот так:

ID: <уникальный идентификатор>

Название: <краткая цель проверки>

Связь: <ID требования/юзер-стори/макета>

Предусловия: <что должно быть готово до старта>

Окружение: <браузер/платформа/локаль/сборка>

Данные: <тестовые значения и учётные записи>

Шаги:
  1) ...
  2) ...
     
Ожидаемый результат: <чёткий результат проверки>

Фактический результат: <заполняется по итогам прогона>

Статус: Not Run / Pass / Fail / Blocked

Приоритет: High / Medium / Low

Примечания: <логи, скриншоты, версии, артефакты>


Так, приступим. Первый тест-кейс: Успешная регистрация с правильными значениями.

Сначала, напишем ID. Я придумала: TC-CISCO-REG-001. Теперь название, с которым мы ранее с тобой определились. Далее мы пишем предусловие. До старта у нас открыта страница для регистрации и локализация на EN. Окружение у нас – это браузер. Теперь пишем, какие данные мы вводим для каждого окошка. Далее шаги, которые мы предпринимаем. Они должны быть конкретные и понятные. Ожидаемый результат. После нашей проверки, мы ожидаем, что регистрация прошла успешно, и нас перекинуло на следующий этап. 

Объясню, почему нет других строк в моих примерах тест-кейсов. Эти строки отсутствуют, потому что это атрибуты, которые заполняются при исполнении теста и в системе управления тестированием, а не при написании самих шагов. В учебных примерах их часто опускают, чтобы сфокусироваться на структуре шагов и ожидаемых результатах. Более того, фактический результат нередко фиксируется не в самом кейсе, а в баг репорте при статусе Failed, поэтому поля могут не показывать в базовых примерах. 

First positive case:

ID: TC-CISCO-REG-001

Title: Successful registration with valid data

Preconditions: Registration page is open; locale = EN

Environment: Web

Test data: Email=test.user+qa@example.com, Password=strongPass@123, First name=Ann, Last name=Ivanova, Country=Russia

Steps: 1) Fill all required fields with valid data; 2) Click "Register".

Expected result: Registration is accepted and the user is moved to the next step (email verification)


Second positive case:

ID:TC-CISCO-REG-002

Title: Password visibility toggle works as expected

Preconditions: Registration page is open

Environment: Web

Test data: Password=strongPass@123

Steps: 1)Enter a password; 2)Click the eye icon to show; 3)Verify characters become visible; 4)Click again to hide.

Expected result: Toggling switches visibility without clearing or corrupting the value.


First negative case:

ID: TC-CISCO-REG-101

Title: Required fields empty – form is not submitted

Preconditions: Registration page is open; locale = EN

Steps: 1) Leave Email, Password, First name, Last name empty and do not select Country/Region; 2) Click "Register".

Expected result: Submission is blocked; each required field shows a clear English error message ("We found some errors. Please review the form and make corrections.")


Second negative case:

ID: TC-CISCO-REG-102

Title: Email format validation

Preconditions: Registration page is open

Test data: Email=user@@example; other fields valid

Steps: 1) Enter an invalid email format; 2) Fill other required fields; 3) Click "Register".

Expected result: A clear error near the Email field ("This value is not a valid email address")

Подведем итоги. Памятка для каждого, кто только начинает:

Один тест-кейс — это всегда одна проверка. Так проще поддерживать свои тесты, искать баги и учиться разбирать логику шаг за шагом. Старайся делать описание шагов настолько конкретным и простым, чтобы любой человек (не только ты) понял, что от него ждётся.

Все тестовые данные — будь то email, пароль или любое другое поле — показывай в тест-кейсе явно, не пиши “что-то корректное”, всегда указывай конкретный пример.

У каждого кейса должен быть только один чёткий ожидаемый результат. Так ты сможешь быстро определить, прошёл тест или провалился, и не тратить время на разбор двусмысленностей.

Позитивные и негативные тесты всегда пиши отдельно. Не смешивай их в одном кейсе, иначе потом будет сложно понять, что именно не сработало.

Статусы вроде “Pass”, “Fail”, “Blocked”, а также фактический результат пишутся только после прогона теста. До запуска эти поля остаются пустыми или стоят как “Not Run”.

Приоритеты (High/Medium/Low) выставляются до запуска — сначала берутся самые важные и критичные сценарии, которые влияют на клиента или деньги компании.

Mini-FAQ:

Зачем нужны негативные тесты?

Они помогают находить баги до того, как их встретит клиент. Негативные сценарии проверяют, что система дружелюбно реагирует на неправильные или странные данные, а не ломается или теряет данные.

Сколько делать тестов на одно поле?

Проще всего: на каждый “разный тип значения” (эквивалентный класс) — один тест. Например: один для корректного email, один для пустого, один для слишком длинного, один с нестандартными знаками, один с кириллицей.

Почему в учебных примерах всегда мало кейсов?

В примерах показывают только базу. В жизни тест-кейсов обычно больше, особенно когда появляются точные требования, спецификации и реальные баги.

Теперь мы готовы двигаться дальше в изучении QA.


