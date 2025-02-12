# Задача: Система за управление на фитнес център

## Създайте програма за управление на фитнес център с помощта на обектно-ориентирано програмиране. Програмата трябва да включва класове, които отговарят за различни роли във фитнес центъра. Всеки клас има основни характеристики и методи, както и уникални условия, които отразяват специфичните им функции.

### Основни изисквания:
Създайте клас FitnessCenter, който съхранява информация за всички потребители, служители, мениджъри и треньори.
Полета: name (име на фитнеса), users (списък с всички потребители), employees (списък със служители), trainers (списък с треньори), managers (списък с мениджъри).

Методи:

- add_user(self, user) - добавя потребител в системата.
- add_employee(self, employee) - добавя служител в системата.
- add_trainer(self, trainer) - добавя треньор в системата.
- add_manager(self, manager) - добавя мениджър в системата.
- track_daily_visits(self, user_id) - увеличава броя на дневните посещения за конкретен ден. Данните се съхраняват в поле daily_visits, структура {дата: брой влизания}.

Създайте клас Manager (Мениджър), който отговаря за управлението на фитнеса.
Полета: manager_id, fname, lname, organized_events (списък със събития).

Методи:

- organize_event(self, event_name, date) - добавя събитие към списъка с организирани събития. Ако събитие с това име вече съществува, връща грешка.
- manager_information(self) - показва информация за мениджъра.

Уникално условие: Организиране на специални събития. Ако събитие с дадено име вече съществува, се връща съобщение „Event already exists“.

Създайте клас Student (Студент), който е потребител на фитнеса със специални условия.
Полета: student_id, fname, lname, monthly_visits (брой посещения за месеца).

Методи:

- early_access_pass(self) - проверява броя на посещенията и връща статус дали студентът има право на ранен достъп.
- student_information(self) - показва информация за студента.

Уникално условие: Ако студентът има над 15 посещения за месеца, получава специален пропуск за ранно трениране.

Създайте клас RegularUser (Редовен потребител), който посещава фитнеса редовно.

Полета: user_id, fname, lname, workouts_count (брой тренировки), vip_status (по подразбиране False).

Методи:

- promote_to_vip(self) - проверява дали броят на тренировките надвишава 100. Ако условието е изпълнено, променя vip_status на True.
- user_information(self) - показва информация за потребителя.

Уникално условие: Ако потребителят е тренирал повече от 100 пъти за година, става VIP.

Създайте клас Employee (Служител), който работи във фитнеса.
Полета: employee_id, fname, lname, evaluations (списък с оценки).

Методи:

- receive_evaluation(self, manager_id, evaluation_score) - записва оценка (1-10) и изчислява средната оценка от всички оценки.
- employee_information(self) - показва информация за служителя.
Уникално условие: Служителите получават седмична оценка от мениджър.

Създайте клас PersonalTrainer (Персонален треньор), който отговаря за тренировките.
Полета: trainer_id, fname, lname, clients (списък с клиенти), group_workouts (списък с организирани групови тренировки).

Методи:
- organize_group_workout(self, workout_type, max_clients) - създава групова тренировка, ако треньорът има поне 5 клиенти.
- trainer_information(self) - показва информация за треньора.
Уникално условие: Треньорите могат да организират групови тренировки само ако имат минимум 5 клиенти.

Общи изисквания:

Данните за всички класове се въвеждат от клавиатурата.
Програмата трябва да може да добавя нови обекти към съответните списъци, като използва подходящи функции за добавяне.
При всяка операция трябва да има защита от грешки (напр. при невалиден вход).
Да се добавят коментари към програмата, които обясняват логиката на всяка функция.
Всички класове трябва да имат метод __str__, който предоставя формат за четливо изписване на обекта.
