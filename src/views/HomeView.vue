<script setup>

</script>
<script setup>

</script>

<template>
    <main>
        <div :class="['home-view', currentTheme]">

            <div v-if="loading" class="falling-quotes"></div>
            <div v-if="loading" class="loading-spinner"></div>
            <div v-else class="quote-container">
                <h1>Книга на {{ formattedDate }}</h1>
                <div class="quote-box">
                    <div class="quote-int">
                        <p>{{ currentBook }}</p>
                    </div>

                    <div class="btt">
                        <button id="moreC" @click="fetchNewBook">Показать еще книгу</button>
                    </div>

                    <div class="btt">
                        <button @click="shareWall">Отправить на стену</button>
                        <button @click="shareBook">Поделиться сервисом</button>
                    </div>

                </div>
            </div>
        </div>
    </main>
</template>

<script>
import bridge from '@vkontakte/vk-bridge';

export default {
    data() {
        return {
            books: [
                "«Война и мир» — Лев Толстой",
                "«Преступление и наказание» — Фёдор Достоевский",
                "«Мастер и Маргарита» — Михаил Булгаков",
                "«Анна Каренина» — Лев Толстой",
                "«Идиот» — Фёдор Достоевский",
                "«Доктор Живаго» — Борис Пастернак",
                "«Братья Карамазовы» — Фёдор Достоевский",
                "«Отцы и дети» — Иван Тургенев",
                "«Человек-невидимка» — Герберт Уэллс",
                "«1984» — Джордж Оруэлл",
                "«Над пропастью во ржи» — Джером Сэлинджер",
                "«Три товарища» — Эрих Мария Ремарк",
                "«Сто лет одиночества» — Габриэль Гарсиа Маркес",
                "«Унесённые ветром» — Маргарет Митчелл",
                "«Великий Гэтсби» — Фрэнсис Скотт Фицджеральд",
                "«Улисс» — Джеймс Джойс",
                "«Маленький принц» — Антуан де Сент-Экзюпери",
                "«По ком звонит колокол» — Эрнест Хемингуэй",
                "«Лолита» — Владимир Набоков",
                "«Шум и ярость» — Уильям Фолкнер",
                "«Прощай, оружие!» — Эрнест Хемингуэй",
                "«Грозовой перевал» — Эмили Бронте",
                "«Гамлет» — Уильям Шекспир",
                "«Джейн Эйр» — Шарлотта Бронте",
                "«Гордость и предубеждение» — Джейн Остин",
                "«Франкенштейн» — Мэри Шелли",
                "«Портрет Дориана Грея» — Оскар Уайльд",
                "«Моби Дик» — Герман Мелвилл",
                "«Дон Кихот» — Мигель де Сервантес",
                "«Парижская коммуна» — Вера Засулич",
                "«Записки из подполья» — Фёдор Достоевский",
                "«Чума» — Альбер Камю",
                "«Метаморфозы» — Овидий",
                "«Великий Гэтсби» — Фрэнсис Скотт Фицджеральд",
                "«Над пропастью во ржи» — Джером Сэлинджер",
                "«О дивный новый мир» — Олдос Хаксли",
                "«Фауcт» — Иоганн Вольфганг Гёте",
                "«Замок» — Франц Кафка",
                "«Американская трагедия» — Теодор Драйзер",
                "«Поющие в терновнике» — Колин Маккалоу",
                "«Тихий Дон» — Михаил Шолохов",
                "«Мидлмарч» — Джордж Элиот",
                "«Смерть в Венеции» — Томас Манн",
                "«Возвращение в Брайдсхед» — Ивлин Во",
                "«Робинзон Крузо» — Даниель Дефо",
                "«Фаренгейт 451» — Рэй Брэдбери",
                "«Белая гвардия» — Михаил Булгаков",
                "«Заводной апельсин» — Энтони Бёрджесс",
                "«Превращение» — Франц Кафка",
                "«Путешествия Гулливера» — Джонатан Свифт",
                "«Дэвид Копперфильд» — Чарльз Диккенс",
                "«Повелитель мух» — Уильям Голдинг",
                "«Крестный отец» — Марио Пьюзо",
                "«Гарри Поттер и философский камень» — Джоан Роулинг",
                "«Скотный двор» — Джордж Оруэлл",
                "«Жажда жизни» — Ирвинг Стоун",
                "«Зеленая миля» — Стивен Кинг",
                "«Старик и море» — Эрнест Хемингуэй",
                "«Пиноккио» — Карло Коллоди",
                "«Автостопом по Галактике» — Дуглас Адамс",
                "«Алхимик» — Пауло Коэльо",
                "«Бегущий за ветром» — Халед Хоссейни",
                "«Дюна» — Фрэнк Герберт",
                "«451 градус по Фаренгейту» — Рэй Брэдбери",
                "«Три мушкетера» — Александр Дюма",
                "«К югу от границы, на запад от солнца» — Харуки Мураками",
                "«Невыносимая легкость бытия» — Милан Кундера",
                "«Пляска смерти» — Стивен Кинг",
                "«Собор Парижской Богоматери» — Виктор Гюго",
                "«Сила привычки» — Чарльз Дахигг",
                "«Братья Львиное сердце» — Астрид Линдгрен",
                "«Девушка с татуировкой дракона» — Стиг Ларссон",
                "«Имя розы» — Умберто Эко",
                "«Моби Дик» — Герман Мелвилл",
                "«Престиж» — Кристофер Прист",
                "«Темная башня» — Стивен Кинг",
                "«Женщина в белом» — Уилки Коллинз",
                "«Дракула» — Брэм Стокер",
                "«Кентерберийские рассказы» — Джеффри Чосер",
                "«Коллекционер» — Джон Фаулз",
                "«Мост в Терабитию» — Кэтрин Патерсон",
                "«Гроздья гнева» — Джон Стейнбек",
                "«Унесенные ветром» — Маргарет Митчелл",
                "«На дороге» — Джек Керуак",
                "«Лавка древностей» — Чарльз Диккенс",
                "«Тёмные аллеи» — Иван Бунин",
                "«Жюль Верн» — Пять недель на воздушном шаре",
                "«Портрет художника в юности» — Джеймс Джойс",
                "«Женщина в песках» — Кобо Абэ",
                "«Слепота» — Жозе Сарамаго",
                "«Человек, который смеется» — Виктор Гюго",
                "«Цветы для Элджернона» — Дэниел Киз",
                "«Жизнь Пи» — Янн Мартел",
                "«Золотая ветвь» — Джеймс Джордж Фрэзер",
                "«Сильмариллион» — Джон Рональд Руэл Толкин",
                "«Атлант расправил плечи» — Айн Рэнд",
                "«Вино из одуванчиков» — Рэй Брэдбери",
                "«Маленькие женщины» — Луиза Мэй Олкотт",
                "«Повелитель времени» — Вернон Виндж",
                "«Колодец одиночества» — Рэдклифф Холл",
                "«Черный обелиск» — Эрих Мария Ремарк",
                "«Степной волк» — Герман Гессе",
                "«Гаргантюа и Пантагрюэль» — Франсуа Рабле",
                "«Яма» — Александр Куприн",
                "«Сияние» — Стивен Кинг",
                "«Завтрак у Тиффани» — Трумен Капоте",
                "«Один день Ивана Денисовича» — Александр Солженицын",
                "«Игры престолов» — Джордж Р. Р. Мартин",
                "«Час быка» — Иван Ефремов",
                "«Пикник на обочине» — Аркадий и Борис Стругацкие",
                "«Город и звезды» — Артур Кларк",
                "«Алиса в Стране чудес» — Льюис Кэрролл",
                "«О мышах и людях» — Джон Стейнбек",
                "«Ведьмак» — Анджей Сапковский",
                "«Замок Броуди» — Арчибальд Кронин"
            ],
            currentBook: "",
            currentDate: new Date(),
            loading: true,
            shownBooks: new Set(),
            currentTheme: 'light' // Default theme
        };
    },
    computed: {
        formattedDate() {
            const options = { day: 'numeric', month: 'long' };
            return this.currentDate.toLocaleDateString('ru-RU', options);
        }
    },
    methods: {
        getBookIndexForDate() {
            const day = this.currentDate.getDate();
            const month = this.currentDate.getMonth(); // January is 0, December is 11
            return (day + month) % this.books.length;
        },
        fetchNewBook() {
            this.showAdd();
            this.loading = true;
            setTimeout(() => {
                if (this.shownBooks.size === this.books.length) {
                    this.shownBooks.clear();
                }
                let newBook;
                do {
                    const randomIndex = Math.floor(Math.random() * this.books.length);
                    newBook = this.books[randomIndex];
                } while (this.shownBooks.has(newBook));

                this.shownBooks.add(newBook);
                this.currentBook = newBook;
                this.loading = false;
            }, 5000);
        },

        shareWall() {
            bridge.send('VKWebAppShowWallPostBox', {
                message: 'Моя книга на сегодня: \n' + this.currentBook + '.\nПолучите свою книгу на сегодня!',
                attachment: 'https://vk.com/app52236926',
            })
                .then((data) => {
                    console.log("Идентификатор записи: ${data.post_id}");

                })
                .catch((e) => {
                    console.log("Ошибка!", e);
                });
        },
        shareBook() {
            bridge.send('VKWebAppShare', {
                link: 'https://vk.com/app52236926'
            })
                .then((data) => {
                    if (data.result) {
                        // Запись размещена
                    }
                })
                .catch((error) => {
                    console.log(error);
                });
        },

        showAdd() {
            const now = Date.now();
            if (now - this.lastAdTime < 30000) {
                return;
            }

            bridge.send('VKWebAppShowNativeAds', {
                ad_format: 'interstitial' /* Тип рекламы */
            })
                .then((data) => {
                    if (data.result) {
                        this.lastAdTime = Date.now(); // Update the last ad time
                    } else {
                        console.log('Ad error');
                    }
                })
                .catch((error) => { console.log(error); });
        },
    },
    mounted() {
        setTimeout(() => {
            const initialBookIndex = this.getBookIndexForDate();
            this.currentBook = this.books[initialBookIndex];
            this.loading = false;
        }, 2000);
    }
};
</script>


<style scoped>
.home-view {
    position: relative;
    text-align: center;
    font-family: 'Georgia', serif;
    /* Классический шрифт для книжной тематики */
    padding: 20px;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #faf8ef;
    /* Мягкий светлый фон, напоминающий бумагу */
    overflow: hidden;
}

.quote-container {
    background-color: #fffdf5;
    /* Цвет бумаги */
    padding: 30px;
    border-radius: 5px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    /* Мягкая тень */
    max-width: 600px;
    width: 90%;
    user-select: none;
    border: 1px solid #eae0c8;
    /* Легкий контур, напоминающий страницу книги */
}

h1 {
    font-size: 22px;
    margin-bottom: 20px;
    color: #333;
    /* Темно-серый текст */
}

p {
    font-size: 18px;
    color: #555;
    /* Немного мягче для приятного чтения */
    line-height: 1.6;
    /* Легкость восприятия текста */
}

button {
    display: inline-block;
    margin-top: 20px;
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    background-color: #6c5b7b;
    /* Темно-фиолетовый цвет для кнопок */
    color: white;
    border: none;
    border-radius: 3px;
    transition: background-color 0.3s ease;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    /* Легкая тень для объема */
}

button:hover {
    background-color: #483d59;
    /* Более темный оттенок при наведении */
}

.btt button {
    margin: 10px;

}


.loading-spinner {
    border: 4px solid #f3f3f3;
    border-top: 4px solid #6c5b7b;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    animation: spin 1s linear infinite;
    margin: auto;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}

/* Мобильная версия */
@media (max-width: 600px) {
    .quote-container {
        width: 95%;
        padding: 20px;
    }

    h1 {
        font-size: 18px;
    }

    p {
        font-size: 16px;
    }

    button {
        font-size: 14px;
        padding: 8px 15px;
    }
}
</style>
