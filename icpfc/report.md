## Вступ
Недавно пройшло чергове щорічне онлайн змагання [ICFP Contest'2018](https://icfpcontest2018.github.io/). Цього року мені все ж таки вдалося взяти в ньому участь, хоч і з труднощами. Завданням цього року було написання алгоритму для бота(ів), основною задачею якого(их) було надрукувати/розібрати задану фігуру в 3D. Максимальний розмір області друку - 256х256х256. Набір команд по руху дуже обмежений і передбачає два типи руху: S (Straight), і L (Г - подібний рух з двох частин). 
- S-рух - це рух вздовж однієї з осей координат, не більше ніж на 15 пікселів.
- L-рух - це рух, що складається з двох S з довжиною не більше 5 пікселів.
На кожен хід витрачається певна кількість енергії, що залежить від кількості ботів і розміру області друку. Є спеціальний режим, при включенні якого можна друкувати в будь-якому місці простору, не боячись, що щось "впаде" вниз. У цьому режимі кількість енергії на підтримку системи збільшується в 10 разів. Завдання - написати алгоритм для бота, який з мінімальною кількістю енергії виконає поставлене завдання. Більш детальні умови можна прочитати на [сайті організатора](https://icfpcontest2018.github.io/full/task-description.html).
Ну а тепер, власне, про те, як ми брали участь <cut />

## 0. Збір учасників

Як людина, яка багато разів брала участь у ICFP Contest, я цілком усвідомлював, що цим треба займатися заздалегідь, інакше в останній тиждень треба буде бігати по всіх і запитувати, чи не хочуть вони проміняти вихідні на 72 години адового програмування. Тому я почав задовго до початку (за кілька місяців). І набрав кістяк команди аж із 7(+/-2) осіб, включаючи мене.
![image](https://user-images.githubusercontent.com/119268/43283579-f6430650-9121-11e8-8281-6b58f088e026.png)

Мовою програмування було обрано [**Kotlin**](https://kotlinlang.org/), як мову, яка була ближча всім і однаково незручна для всіх :)

Як завжди буває, перед самим початком люди почали потроху відпадати. Відпустки, родичі, літо. Загалом, форс-мажори навколо. ¯\\_(ツ)\_/¯.  
Залишилося нас четверо, з яких двоє взяли на себе зобов'язання тільки на частину конкурсу (Я і сам стартував тільки зі наступного дня). Спасибі Даші, яка раптово змогла знайти п'ятого на два дні.

## 1. Підготовка
На момент початку завдяки спільним зусиллям був готовий репозиторій, створений базовий проект, у якому навіть можна було запускати тести, встановлений канал зв'язку через Slack. У Slack відразу були прив'язані боти, які нас сповіщали про будь-які новини з Twitter і Reddit, пов'язані з ICFPC. Дуже зручно і корисно (минулого чи позаминулого року ми навіть сканували гітхаб на предмет відкритих репозиторіїв виду icfpc, щоправда, не знадобилося :).
Зовсім не хотілося втрачати час із правами, збіркою і запуском. Мушу сказати, що цього року все пройшло більш-менш гладко. Щоправда, сильно позначалося те, що ніхто у своїй роботі не використовує Kotlin у продакшені. Якщо хтось запитає "чому ж ви тоді цю мову використовували?", то я відповім "Тому що ставка була на те, що в команді будуть люди, які з нею працюють, але не склалося. А на переправі мову не міняють :)".

## 2. Початок
// Умова  
Змагання почалося о 19:00 п'ятничного вечора, у той час як я тусив на корпоративі. Команда ж на той час відносно швидко прочитала завдання і пішла вечеряти :)

23:00 (+4 години) У нас є базові моделі даних, і нашвидкуруч написаний вирішувач, який повинен видати набір команд за заданою умовою. Усі йдуть спати. П'ятниця - робочий день, і починати програмувати після повноцінного робочого дня дуже важко (а в когось навіть був реліз (так-так, п'ятничний реліз)).

## 3. Перший день (6 годин після початку)
01:10 (+6 годин) Перед сном мені приходить в голову "геніальна ідея" про те, що нам зовсім навіть не треба нічого будувати. Адже можна просто розгорнути задачу задом наперед і "розбирати модель". На мою думку, це повинно нам сильно спростити логіку і прибрати необхідність обробляти варіанти, коли ми себе забудовуємо, або отримуємо порожнини, до яких боти потім не зможуть мати доступ. З цією ідеєю я засинаю.

Прокидатися після корпоративу було дуже важко :) І погода цьому не сприяла. Тому до колег по ICFPC я приєднався близько 10 години ранку. До цього моменту ми зібралися максимально можливим складом із 5 осіб.

10:00 (+15 годин) Ми розбираємо завдання, я дочитую умову.  

11:45 (+16 годин) Даша активно воює з форматом даних і особливостями роботи з unsigned даними в Kotlin'і. Ми вміємо читати все, що нам треба... ну майже.  

12:00 (+17 годин) Ми довго обговорюємо варіант з об'їданням і розворотом моделі. З якихось магічних причин нам здається, що не може бути все просто. І ми перевіряємо можливі кутові кейси, коли наш алгоритм не зможе працювати. До цього часу у нас виходить побудувати карту простору з розрахованими відстанями до землі. Модель мала бути закріплена до землі, і в кожен момент часу кожна точка моделі повинна мати шлях до "землі". Порахувавши мінімальну відстань кожної точки до землі, ми отримали інформацію про те, в якій послідовності можна безпечно "об'їдати модель". На кожному кроці - просто об'їдаємо найбільш віддалену від землі частину, оскільки на цій частині нічого іншого не тримається.

14:00 (+19 годин) Алгоритм начебто є і написаний, але прохід з точки А в точку Б ускладнений тим фактом, що необхідно використовувати тільки задані варіанти ходів, і в який раз ми натикаємося на те, що "а може все-таки спробувати вивчити і почати використовувати [А*](https://en.wikipedia.org/wiki/A*_search_algorithm)?". Бот промахується, дуже довго планує як йому йти в задану точку.

15:30 (+20 годин) Перший працюючий алгоритм, у якого є функція `gotoAndEat`. Для того щоб з'їдати фігуру, ми додаємо додаткову команду `Erase`, яка згодом, при розвороті набору команд перетворюється на `Fill`. У процесі ми трохи натрапляємо на те, що пропускаємо кілька команд і не завжди правильно розгортаємо всі координати. In soviet Russia порожня клітинка заповнює бота.

15:50 (+20 годин) Не можна просто так взяти і проітеруватися в потрійному вкладеному циклі `x in minx..maxx, y in miny..miny, z in minz..maxz`. Занадто багато можливостей для простої помилки у вигляді копіпаста. Вже не пам'ятаю, навіщо нам це було треба, але наш валідатор відмовлявся пропускати наші рішення
```
for (x in min.x..max.x) {
 for (y in min.x..max.x) {
  for (z in min.x..max.x) {
```
16:00 (+21 година) Ми нарешті вміємо рахувати енергію правильно. І у нас є можливість порівнювати наші рішення між собою. До цього ми експлуатували сайт організаторів, що було не дуже зручно. Тут же фіксуємо чергову багу, пов'язану з тим, що ми після кожного кроку віддаємо не повністю підтриманий `state`, через що ми намагаємося "їсти" одні й ті самі клітинки кілька разів поспіль.

17:21 (+22 години) Незважаючи на те, що ми успішно вирішуємо багато простих завдань, ми помічаємо, що наш бот підозріло довго вештається по простору для друку. Виявляємо, що замість того, щоб іти до найближчої точки з можливих, він вибирає далеку. Незважаючи на те, що на візуалізації це виглядало дуже вражаюче (робота кипить, бот бігає), даний підхід був досить неефективний.
```
- val coordsToReach = builder.coords().toMutableList()
+ val coordsToReach = builder.coords().reversed().toMutableList()
```
Розгортати команди ми теж забули
```
- return commmands
+ return commmands.reversed()
```

17:28 (+22 години) Робимо базовий солвер, і запускаємо його на всіх задачах, розподіляючи групи задач між нашими комп'ютерами.

18:02 (+23 години) Не можна просто так взяти і не забути той факт, що ми не можемо ходити крізь заповнені клітинки. Через те, що ми "об'їдали" модельку, ми не зіткнулися з цією проблемою аж до 18 задачі ¯\\_(ツ)\_/¯.

18:52 (+23 години) Ми збираємо всі можливі рішення, які змогли, і відправляємо їх на сторону сервера. Через довгий пошук шляху ми встигаємо порахувати (всього?) 33 задачі з 186. Важко оцінювати, наскільки це хороший результат, але ми точно в топ 42, тому що рівно стільки команд надіслали рішення на lighting round :). 

19:00 (+24 години) Організатори викладають нову частину рішення, у якому з'являються нові види задач: розбір моделі (ми внутрішньо хихикаємо, тому що у нас вже все з коробки готово, просто потрібно прибрати розворот команд для бота), і перескладання моделі (коли з однієї модельки потрібно зібрати іншу). Додані команди для ботів, які дозволяють одразу заповнювати/видаляти лінію, прямокутник, і паралелепіпед. Дуже зручні команди для того, щоб заповнювати/видаляти великі об'єми модельок. Забігаючи наперед, мушу сказати, що цими командами нам так і не вдалося скористатися.

21:00 (+26 годин) Через те, що Lighting Round закінчився, і є де розвернутися, ми розподіляємо свої сили. З запланованих завдань ми вибрали наступне:
- мікро-сервер, який повинен агрегувати кращі з рішень і складати їх у зручне для подальшої відправки місце
- оптимізація нашого пошуку (тут ми додали найпростіший обмежувальний куб, щоб не намагатися шукати рішення десь далеко, якщо можна це зробити в рамках напрямку на задану точку)
- тюнінг нашого пошукового алгоритму, який вибирає з можливих точок таку, з якої згодом не треба буде рухатися (до цього у нас прямо обов'язково треба було зробити хід, перш ніж побудувати/з'їсти клітинку)
- купа прапорів, що впливають на алгоритм. Ми їх агрегуємо для можливості локально перевіряти будь-які рішення, не ламаючи основних.

22:15 (+27 годин) Ми усвідомлюємо, що наш бот надмірно обережний, і може будувати лише в 6 напрямках з 18 можливих.

## 4. Другий день (~30 годин після початку)

01:28 (+30 годин) На момент +30 годин - ми 17-ті. У деяких задачах - ми перші(sic!). Ми вважаємо, що це круто, але ніч попереду, а ми вночі будемо спати.

03:21 (+32 години) Я, ігноруючи наше правило "завжди спати вночі", додаю `bounding box` для побудови шляху.

![image](https://user-images.githubusercontent.com/119268/43682457-78fbc3bc-987e-11e8-8e70-2da708d5f0dc.png)

09:24 (+38 годин) Фінальні правки для локального сервера, який тепер може обробляти рішення без зазначення завдання.

09:27 (+38 годин) Ми усвідомлюємо, що наш алгоритм вважає все дуже довго для моделей з кількістю точок близько мільйона. Простими обчисленнями ми розуміємо, що для того, щоб порахувати все вчасно, нам знадобиться машина часу, щоб запустити рішення за два дні до початку змагань.

**Random messages from chat**
> Ох, я тільки встав. Зараз душ поїм і буду виходити  
> Не їж душ.

10:00 (+39 годин) Ми вважаємо, що свої рішення ми будемо відправляти на amazon bucket, із зазначенням витраченої кількості енергії. Ми вбудовуємо цю логіку в наші вирішувачі, щоб не витрачати дорогоцінний час ЦП для перерішення завдань.

13:28 (+43 години) Ми остаточно позбавляємося локального сервера, на який витратили близько 8 людино-годин (facepalm). Заводимо Jenkins і починаємо ганяти завдання на ньому. План мінімум - до 72 годин мати рішення на всі завдання ~500. Залишається менше половини часу, а наші алгоритми повзають як черепахи. За роботу з кількома ботами ще ніхто не брався.

14:26 (+44 години) Ми повністю підвели наші алгоритми під можливість запускатися з Command-Line'a. Наш скрипт проганяє рішення на заданому завданні, рахує кількість енергії і відправляє результат на Amazon. Льоша концентрується на новому алгоритмі, ідея якого полягає в тому, щоб порахувати скелет, який можна з'їсти, не побоюючись того, що якісь частини залишаться в підвішеному стані, і тримати цей скелет актуальним у міру подальшого з'їдання. Наш Jenkins із перемінним успіхом починає рахувати завдання.

15:28 (+45 годин) Ми нарешті можемо вирішувати завдання типу Reassemble, коли на вхід дається одна модель, а на виході має вийти інша. Найшвидшим рішенням, що ми змогли придумати, було - розбираємо першу модель, збираємо другу. Це нам дозволило бути алгоритмонезалежними, хоч і не дуже ефективними.

16:15 (+46 годин) Льоша починає активно тестувати жадібний алгоритм. На тлі нашого попереднього рішення він виглядає краще за всіма задачами.

18:00 (+48 годин) Виявляємо, що наш алгоритм на великих завданнях іноді перестає знаходити можливі шляхи. Пов'язано це з тим, що ми зариваємося в модель, і наш обмежувальний куб дає занадто маленький простір для того, щоб була можливість побудувати можливий шлях.

18:35 (+48 годин) Перші мультиботи відправляються вирішувати завдання. Через те, що ми досі смутно розуміємо, як повинні працювати розподіл і злиття, ми тримаємо дві змінні bot1 і bot2 і намагаємося їх скоординувати. Після того, як боти закінчать роботу, ми шукаємо точку між ними, ведемо до неї, і робимо злиття. Для того щоб належним чином не залишати висячі шматки моделі в просторі, ми дозволяємо ботам їсти тільки в тому випадку, якщо вони не порушують цілісність конструкції моделі. У випадку, якщо боту нічого робити, він просто чекає наступного можливого випадку. Синхронізація ботів досить проста - у разі, якщо шлях бота перетинається з іншим ботом, він просто чекає. Код виглядає більш ніж жахливо, але, тим не менш, виглядає робочим.

19:00 (+49 годин) Ми заводимо вибір алгоритму під командний рядок і запускаємо мультиботів вирішувати завдання. Мультиботи очікувано рвуть у друзки одноботні рішення.

**Random messages from chat**
> build solution - #39 Failure after 6 hr 12 min 😿

19:56 (+50 годин) Ми прибираємо з усіх алгоритмів знання про те, яке завдання вони вирішують, перетворюючи всі алгоритми на алгоритм розбору. Надбудова зверху відповідає за те, щоб віддавати алгоритмам правильний стан системи і розворот. Ця ж надбудова відповідає за те, щоб розгортати команди для різних типів завдань. Код стає чистішим і потенційно безпечнішим.

23:00 (+53 години) Переписуємо алгоритм мультиботів на високорівневі команди, що відповідають за свої частини (розподіл ботів, об'єднання тощо). Мультиботи - тепер окремий випадок, і ми можемо збирати наш алгоритм із частин. Так, якщо алгоритм не знає, як розділяти ботів, ми можемо зробити це за нього і віддати йому стан, у якому він вирішить завдання, а після цього інша частина збере ботів назад у точку і відправить додому.

## 5. Третій день (~54 години після початку)

01:39 (+55 годин) Льоша прискорює свій алгоритм майже в 60(sic!) разів, пожертвувавши необхідністю непотрібного перерахунку стану. Тепер ми, по ідеї, можемо встигати вирішувати найбільші завдання.

**Random messages from chat**
> На 50 задачі кешування доступних точок 105s -> 68s  
> Відмова від State дала 4s (68 -> 64)  
> Прибрав матрицю - 64 -> 33  
> На 186 шлях вважається 1ms у 60 разів)  

8:00 (+62 години) Прокинувшись, ми спостерігаємо дивну картину. Незважаючи на те, що Льошин алгоритм працює дуже швидко, ми втрачаємо час на верифікацію рішення. Витративши 5 із половиною годин на вирішення завдання, ми витрачаємо стільки ж на верифікацію o_O

**Random messages from chat**
> Done with 74 (20622.991). Verifying...  
> Done verification in 20519 seconds. With 2481175342640 energy

8:29 (+62 години) Ми виявляємо, що наші доблесні будівельники підкидають нам диво інженерної думки, не рахуючись із законами фізики. Крім цього сьогодні - понеділок, і нас залишається троє. Троє невиспних зомбі. У Льоші, до того ж, увечері поїзд.

![image](https://user-images.githubusercontent.com/119268/43754761-128b6cac-9a15-11e8-9f61-2f15f36d42ef.png)

Додаємо очікування, щоб боти чекали один одного перед переміщенням на наступний рівень
> Reached 973 (3114) of 3198 of 36  
> Reached 972 (3111) of 3198 of 36  
> Bot 1 Will wait for another bot while moving to the next ground  

10:38 (+64 години) Наш Jenkins вмирає. Ми усвідомлюємо, що 8 задач на вирішення встигли з'їсти по 8 гігабайт+ кожна. Рятує тільки жорсткий ресет, який ускладнюється тим, що всю пам'ять ми теж вичерпали. Погасили сервер і стартували заново. Як виявилося, причиною були мультиботи, які, перебуваючи в сусідніх клітинках, не могли розійтися миром і чекали, поки кожен із них закінчить операцію. На додачу до всього ці джентльмени не забували повідомляти про свої дії в консоль, що й вилилося в багато гігабайт логів про те, як два надзвичайно ввічливі боти вирішують почекати, поки інший виконає свою роботу.

| Bot1 |  Bot2 |  
|:-----------------------:|:-----------------------:|  
| ![](https://media.giphy.com/media/5AcR8w022Gk4E/giphy.gif) | ![](https://media.giphy.com/media/l0Iy8I6QBKzt6dDyg/giphy.gif) |

12:48 (+66 годин) Ми майже до нуля урізаємо наш верифікатор для Льошиного алгоритму, залишаючи тільки найважливіше. Запускаємо на вирішення всі нерозв'язані завдання. Протягом наступної години ми отримуємо відповіді майже на всі завдання.

13:00 (+67 годин) Ми всіма правдами і неправдами намагаємося прикрутити мультиботів до Льошиного алгоритму. Відсутність повного верифікатора дуже сильно б'є по нас, і ми не розуміємо, чому наші рішення відмовляються проходити. 

15:00 (+69 годин) Не можна просто так взяти і прикрутити алгоритм із кількох частин так, щоб він працював правильно. Десь забули розгорнути команди, десь два рази ботів повертали в початкову точку.

16:45 (+70 годин) Ми обклалися купою додаткових даних, логів і трейсів, але 100% вірогідності у вдалості нашого рішення все ще не можемо дати.

17:38 (+71 година) За трохи більше ніж годину до закінчення у нас залишаються нерозв'язаними 13 задач із 488. Ми з Льошею, який уже сидить у поїзді, намагаємося скоординувати наші дії з виявлення причини, через яку наші боти носяться по всій карті, стикаючись один з одним.

18:25 (+72 години) Знаючи, що під кінець змагання можуть бути великі проблеми з відправкою результатів, ми відправляємо проміжний результат наших рішень. Залишаються нерозв'язаними 5 задач.

18:45 (+72 години). Ми нарешті знаходимо багу, у якій ми неправильно перетинали два множини точок
> A ∩ B ∩ C ≠ A ∩ (B ∪ C)
 ```
 val commonPoints = affectedPoints.intersect(matrix.filledCoordinates).intersect(volatile)`
 vs
 val commonPoints = affectedPoints.intersect(matrix.filledCoordinates.plus(volatile))`
 ```
 Ми натравлюємо наших мультиботів на початкові невеликі завдання.
 
 18:58 (+72 години) Відправляємо наші фінальні рішення. Залишаються нерозв'язаними 3 задачі.
 
 ## 6. Після закінчення змагання
 
 19:05. Ми на 57 місці (за 6 годин до закінчення).  
 20:08. Дораховуються залишені 3 задачі.  
 23:24. Льоша надсилає нам подивитися, як наші мультиботи всіма розбираються з першою задачкою.  
 ![7-bots-solution](https://user-images.githubusercontent.com/119268/43815783-011e3312-9ada-11e8-84de-7b9b3ddfcaa8.gif)
 ??:??. Ми усвідомлюємо, що наші мультиботи зламалися на задачах "зібрати-розібрати", так що у фінальному результаті у нас близько 23 неправильних рішень. ¯\\_(ツ)\_/¯  
 
## 7. Післяслово, Висновки та Подяки.
 
**Команда**: Forth Major  
**Місце в Lightigh Round**: ~17 за 6 годин до закінчення
**Місце в Main Round**: ~54 за 6 годин до закінчення
**Склад-людей-які-все-таки-змогли-хай-і-не-всі-72-години**:  
- Дарина Чернишова (@sunstream)
- Павло Чернишов (@cronos)
- Павло Тайкало (@tt.kilew)
- Костя Бичков (@xNekOIx)
- Олексій Демедикій (@daloog)

**Окрема Подяка**:
- Сергій Зенченко (@zen)

**Що спрацювало**  
- Заздалегідь підготовлена структура проєкту та акаунти на gitlab (на момент початку в усіх були доступи)
- Командна робота та розподіл за фічами.
- Тести

**Що наступного разу можна зробити краще**
- Jenkins + Command Line одразу. Нехай краще не знадобиться, ніж будемо на нього витрачати час
- Трохи підтягнути алгоритми
- Трохи більше перевірок, особливо на фінальні результати
- Братися за алгоритми, які принесуть нам у N разів більше (мультиботи, групи) раніше, а не намагатися вижати максимум із базового алгоритму.
- Оцінювати швидкість рішень і переробляти, якщо ми явно не встигаємо.
- Змушувати підписуватися кров'ю учасників, щоб не шукати їх в останній момент :) Все одно не спрацює, я знаю.
- Почитати і спробувати мову програмування, якою будемо програмувати трохи заздалегідь.
- Навчитися робити швидкі, перевірені рішення )

P.S. Хочу сказати велике спасибі всім учасникам, що змогли виділити такий великий проміжок часу для загального блага :). Чи могли б ми зробити більше? Могли %) Але, як кажуть, людина людині - вовк, а невиспана людина - зомбі зомбі зомбі.
Удачі. До зустрічі наступного року.
