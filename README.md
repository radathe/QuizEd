# QuizEd
import tkinter as tk
from tkinter import ttk, messagebox
import time

#Расширенная база данных вопросов (8 предметов по 5 вопросов)
questions_db = {
    "Математика": [
        {"question": "Сколько будет 2 + 2 * 2?", "answers": ["6", "8", "4"], "correct": 0, "comment": "Сначала умножение: 2*2=4, затем 2+4=6."},
        {"question": "Чему равен √144?", "answers": ["12", "14", "16"], "correct": 0, "comment": "Корень из 144 равен 12."},
        {"question": "Решите уравнение: 3x - 7 = 14", "answers": ["x = 7", "x = 9", "x = 5"], "correct": 0, "comment": "3x = 21 → x = 7"},
        {"question": "Чему равен cos(π)?", "answers": ["-1", "0", "1"], "correct": 0, "comment": "Косинус π радиан равен -1"},
        {"question": "Площадь круга с радиусом 3 равна:", "answers": ["9π", "6π", "3π"], "correct": 0, "comment": "Формула: S = πr²"}
    ],
    "История": [
        {"question": "В каком году началась Вторая мировая война?", "answers": ["1939", "1941", "1945"], "correct": 0, "comment": "1 сентября 1939 года"},
        {"question": "Кто был первым президентом России?", "answers": ["Борис Ельцин", "Михаил Горбачёв", "Владимир Путин"], "correct": 0, "comment": "Ельцин избран в 1991 году"},
        {"question": "Столица Древней Руси?", "answers": ["Киев", "Москва", "Новгород"], "correct": 0, "comment": "Киев - столица с IX века"},
        {"question": "Год крещения Руси?", "answers": ["988", "998", "1015"], "correct": 0, "comment": "Крещение при князе Владимире"},
        {"question": "Кто написал 'Слово о полку Игореве'?", "answers": ["Неизвестный автор", "Нестор Летописец", "Александр Пушкин"], "correct": 0, "comment": "Авторство не установлено"}
    ],
    "Русский язык": [
        {"question": "Сколько гласных букв в русском алфавите?", "answers": ["10", "6", "33"], "correct": 0, "comment": "А, Е, Ё, И, О, У, Ы, Э, Ю, Я"},
        {"question": "Какое слово является наречием?", "answers": ["быстро", "бег", "бегать"], "correct": 0, "comment": "Наречие отвечает на вопрос 'как?'"},
        {"question": "Сколько падежей в русском языке?", "answers": ["6", "5", "7"], "correct": 0, "comment": "Именительный, Родительный и др."},
        {"question": "Какое предложение сложное?", "answers": ["Небо потемнело, и пошёл дождь.", "Светит солнце.", "Тёплый летний день"], "correct": 0, "comment": "Содержит две грамматические основы"},
        {"question": "В каком слове пишется 'нн'?", "answers": ["стеклянный", "ветреный", "оловяный"], "correct": 0, "comment": "Суффиксы -анн-, -янн-"}
    ],
    "Информатика": [
        {"question": "Основоположник теории алгоритмов?", "answers": ["Алан Тьюринг", "Билл Гейтс", "Стив Джобс"], "correct": 0, "comment": "Тьюринг ввел понятие алгоритма"},
        {"question": "Какой язык программирования самый старый?", "answers": ["Фортран", "Python", "Java"], "correct": 0, "comment": "Фортран создан в 1957 году"},
        {"question": "Что измеряется в битах?", "answers": ["Количество информации", "Скорость интернета", "Частота процессора"], "correct": 0, "comment": "Бит - минимальная единица информации"},
        {"question": "Что такое ОЗУ?", "answers": ["Оперативная память", "Жёсткий диск", "Процессор"], "correct": 0, "comment": "Random Access Memory"},
        {"question": "Какой тег создаёт ссылку в HTML?", "answers": ["<a>", "<link>", "<href>"], "correct": 0, "comment": "<a href='...'>...</a>"}
    ],
    "Обществознание": [
        {"question": "Основной закон государства?", "answers": ["Конституция", "Уголовный кодекс", "Гражданский кодекс"], "correct": 0, "comment": "Конституция - основной закон"},
        {"question": "Кто является главой государства в РФ?", "answers": ["Президент", "Премьер-министр", "Председатель ГД"], "correct": 0, "comment": "Согласно Конституции РФ"},
        {"question": "Что такое инфляция?", "answers": ["Рост цен", "Падение курса валюты", "Увеличение зарплат"], "correct": 0, "comment": "Устойчивое повышение общего уровня цен"},
        {"question": "Сколько ветвей власти в РФ?", "answers": ["3", "2", "4"], "correct": 0, "comment": "Законодательная, исполнительная, судебная"},
        {"question": "Что не относится к правам человека?", "answers": ["Право на жильё", "Право на труд", "Право нарушать законы"], "correct": 2, "comment": "Права не могут нарушать законы"}
    ],
    "Химия": [
        {"question": "Формула воды?", "answers": ["H₂O", "CO₂", "H₂SO₄"], "correct": 0, "comment": "Два атома водорода и один кислорода"},
        {"question": "Какой газ самый лёгкий?", "answers": ["Водород", "Гелий", "Азот"], "correct": 0, "comment": "Атомный вес водорода = 1"},
        {"question": "Что показывает pH?", "answers": ["Кислотность", "Плотность", "Температуру"], "correct": 0, "comment": "Шкала от 0 (кислота) до 14 (щелочь)"},
        {"question": "Какой элемент обозначается как Au?", "answers": ["Золото", "Серебро", "Алюминий"], "correct": 0, "comment": "От латинского 'Aurum'"},
        {"question": "Что такое окисление?", "answers": ["Отдача электронов", "Приём электронов", "Разложение вещества"], "correct": 0, "comment": "Реакция с кислородом"}
    ],
    "Физика": [
        {"question": "Единица измерения силы?", "answers": ["Ньютон", "Джоуль", "Ватт"], "correct": 0, "comment": "1 Н = 1 кг·м/с²"},
        {"question": "Кто открыл закон всемирного тяготения?", "answers": ["Ньютон", "Эйнштейн", "Галилей"], "correct": 0, "comment": "Яблоко и F = G(m₁m₂)/r²"},
        {"question": "Что измеряется в герцах?", "answers": ["Частота", "Напряжение", "Сопротивление"], "correct": 0, "comment": "1 Гц = 1 колебание в секунду"},
        {"question": "Какой закон описывает силу Архимеда?", "answers": ["F = ρgV", "F = ma", "U = RI"], "correct": 0, "comment": "Сила равна весу вытесненной жидкости"},
        {"question": "Что такое инерция?", "answers": ["Свойство сохранять скорость", "Сопротивление движению", "Энергия движения"], "correct": 0, "comment": "Первый закон Ньютона"}
    ],
    "Биология": [
        {"question": "Сколько хромосом у человека?", "answers": ["46", "23", "64"], "correct": 0, "comment": "23 пары хромосом"},
        {"question": "Кто открыл пенициллин?", "answers": ["Флеминг", "Пастер", "Мечников"], "correct": 0, "comment": "Александр Флеминг в 1928 году"},
        {"question": "Где происходит фотосинтез?", "answers": ["В хлоропластах", "В митохондриях", "В ядре"], "correct": 0, "comment": "Хлоропласты содержат хлорофилл"},
        {"question": "Что не является кровеносной клеткой?", "answers": ["Нейрон", "Эритроцит", "Лейкоцит"], "correct": 0, "comment": "Нейрон - нервная клетка"},
        {"question": "Какой витамин вырабатывается на солнце?", "answers": ["D", "C", "A"], "correct": 0, "comment": "Витамин D синтезируется под UV-лучами"}
    ]
}

class QuizApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Образовательный Quiz")
        self.root.geometry("600x500")
        
        #Переменные состояния
        self.subject = None
        self.current_question = 0
        self.score = 0
        self.time_left = 0
        self.timer_id = None
        
        #Стили
        self.style = ttk.Style()
        self.style.configure("TButton", font=("Arial", 12))
        self.style.configure("TLabel", font=("Arial", 12))
        
        self.show_subject_selection()
    
    def show_subject_selection(self):
        """Окно выбора предмета"""
        self.clear_window()
        self.stop_timer()
        
        ttk.Label(self.root, text="Выберите предмет:", font=("Arial", 16, "bold")).pack(pady=20)
        
        # Сетка кнопок для предметов
        frame = ttk.Frame(self.root)
        frame.pack(pady=10, padx=20, fill="both", expand=True)
        
        subjects = list(questions_db.keys())
        for i, subject in enumerate(subjects):
            btn = ttk.Button(
                frame, 
                text=subject, 
                command=lambda s=subject: self.start_quiz(s),
                width=20
            )
            btn.grid(row=i//2, column=i%2, padx=10, pady=5)
    
    def start_quiz(self, subject):
        """Начало тестирования"""
        self.subject = subject
        self.questions = questions_db[subject]
        self.current_question = 0
        self.score = 0
        self.show_question()
    
    def show_question(self):
        """Отображение вопроса с таймером"""
        self.clear_window()
        self.stop_timer()
        
        if self.current_question >= len(self.questions):
            self.show_results()
            return
        
        question_data = self.questions[self.current_question]
        
        # Заголовок с прогрессом
        header = ttk.Frame(self.root)
        header.pack(fill="x", padx=20, pady=10)
        
        ttk.Label(
            header, 
            text=f"Вопрос {self.current_question + 1}/{len(self.questions)}",
            font=("Arial", 12, "bold")
        ).pack(side="left")
        
        # Таймер (30 секунд)
        self.time_left = 30
        self.timer_label = ttk.Label(header, text=f"⏱️ {self.time_left} сек", font=("Arial", 12))
        self.timer_label.pack(side="right")
        self.start_timer()
        
        # Сам вопрос
        ttk.Label(
            self.root, 
            text=question_data["question"],
            font=("Arial", 14),
            wraplength=550,
            justify="center"
        ).pack(pady=20)
        
        # Варианты ответов
        self.answer_var = tk.IntVar(value=-1)
        
        for i, answer in enumerate(question_data["answers"]):
            ttk.Radiobutton(
                self.root,
                text=answer,
                variable=self.answer_var,
                value=i,
                style="TRadiobutton"
            ).pack(anchor="w", padx=50, pady=5)
        
        # Кнопка ответа
        ttk.Button(
            self.root,
            text="Ответить",
            command=self.check_answer
        ).pack(pady=20)
    
    def start_timer(self):
        """Запуск обратного отсчёта"""
        if self.time_left > 0:
            self.timer_label.config(text=f"⏱️ {self.time_left} сек")
            self.time_left -= 1
            self.timer_id = self.root.after(1000, self.start_timer)
        else:
            self.timeout()
    
    def stop_timer(self):
        """Остановка таймера"""
        if self.timer_id:
            self.root.after_cancel(self.timer_id)
            self.timer_id = None
    
    def timeout(self):
        """Действия при истечении времени"""
        self.stop_timer()
        messagebox.showwarning("Время вышло!", "Вы не успели ответить на вопрос")
        self.answer_var.set(-2)  # Специальное значение для "время вышло"
        self.check_answer()
    
    def check_answer(self):
        """Проверка ответа пользователя"""
        self.stop_timer()
        
        user_answer = self.answer_var.get()
        if user_answer == -1:
            messagebox.showwarning("Внимание", "Выберите вариант ответа!")
            self.start_timer()
            return
        
        question_data = self.questions[self.current_question]
        is_correct = (user_answer == question_data["correct"])
        
        if is_correct:
            self.score += 1
        
        # Окно с результатом
        self.clear_window()
        
        # Цвет результата
        result_color = "#2E8B57" if is_correct else "#DC143C"
        result_text = "Правильно! ✅" if is_correct else "Неправильно! ❌"
        
        ttk.Label(
            self.root, 
            text=result_text,
            font=("Arial", 16, "bold"),
            foreground=result_color
        ).pack(pady=15)
        
        # Правильный ответ
        correct_answer = question_data["answers"][question_data["correct"]]
        ttk.Label(
            self.root,
            text=f"Правильный ответ: {correct_answer}",
            font=("Arial", 12)
        ).pack(pady=5)
        
        # Комментарий
        ttk.Label(
            self.root, 
            text=f"Объяснение: {question_data['comment']}",
            font=("Arial", 11),
            wraplength=550,
            justify="center"
        ).pack(pady=10)
        
        # Прогресс
        ttk.Label(
            self.root,
            text=f"Ваш счёт: {self.score}/{len(self.questions)}",
            font=("Arial", 12, "bold")
        ).pack(pady=10)
        
        # Кнопка продолжения
        btn_text = "Далее" if self.current_question < len(self.questions) - 1 else "Завершить"
        ttk.Button(
            self.root,
            text=btn_text,
            command=self.next_question
        ).pack(pady=20)
    
    def next_question(self):
        """Переход к следующему вопросу"""
        self.current_question += 1
        self.show_question()
    
    def show_results(self):
        """Показ итоговых результатов"""
        self.clear_window()
        
        ttk.Label(
            self.root, 
            text="Тест завершён!",
            font=("Arial", 18, "bold")
        ).pack(pady=20)
        
        # Результаты
        result_frame = ttk.Frame(self.root)
        result_frame.pack(pady=10)
        
        ttk.Label(
            result_frame,
            text=f"Предмет: {self.subject}\nПравильных ответов: {self.score}/{len(self.questions)}",
            font=("Arial", 14),
            justify="center"
        ).pack()
        
        # Процент правильных ответов
        percentage = (self.score / len(self.questions)) * 100
        ttk.Label(
            result_frame,
            text=f"Успешность: {percentage:.1f}%",
            font=("Arial", 14, "bold"),
            foreground="#2E8B57" if percentage >= 60 else "#DC143C"
        ).pack(pady=10)
        
        # Кнопки
        btn_frame = ttk.Frame(self.root)
        btn_frame.pack(pady=20)
        
        ttk.Button(
            btn_frame,
            text="Выбрать другой предмет",
            command=self.show_subject_selection
        ).pack(side="left", padx=10)
        
        ttk.Button(
            btn_frame,
            text="Выход",
            command=self.root.quit
        ).pack(side="right", padx=10)
    
    def clear_window(self):
        """Очистка окна"""
        for widget in self.root.winfo_children():
            widget.destroy()

#Запуск приложения
if __name__ == "__main__":
    root = tk.Tk()
    app = QuizApp(root)
    root.mainloop()

