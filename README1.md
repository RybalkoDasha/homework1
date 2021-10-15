# Homework Python 'OOP'

```python
class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []  # какие курсы студент уже прошел
        self.courses_in_progress = []  # о списке курсов, которые сейчас изучаются
        self.grades = {}  # информация студентов об оценках

    def add_courses(self, course_name):  # метод для добавления пройденных курсов
        self.finished_courses.append(course_name)

    def rate_lectures(self, lecturer, course, grade):
        if isinstance(lecturer,
                      Lecturer) and course in lecturer.courses_attached and (course in self.courses_in_progress
                                                                             or course in self.finished_courses):
            if course in lecturer.grades:
                lecturer.grades[course] += [grade]
            else:
                lecturer.grades[course] = [grade]
        else:
            return 'Ошибка'

    def count_grades_student(self):
        average_grade = 0
        count_list = 0
        for key, value in self.grades.items():
            for el in value:
                average_grade += el
            count_list += len(value)
        res = 0
        if count_list != 0:
            res = average_grade / count_list
        return res

    def __str__(self):
        self.count_grades_student()
        res = f"""Имя: {self.name}\nФамилия: {self.surname} \
              \nСредняя оценка за домашние задания:{self.count_grades_student()} \
              \nКурсы в процессе изучения:{",".join(self.courses_in_progress)}\
              \nЗавершённые курсы:{",".join(self.finished_courses)}"""
        return res

    def __lt__(self, other):
        if not isinstance(other, Student):
            print('Not a Student')
            return
        return self.count_grades_student() < other.count_grades_student()


class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []  # есть закрепленный за ними список курсов


class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}  # словарь для полуения оценок за лекции от студентов

    def count_grades_lecturer(self):
        average_grade = 0
        count_list = 0
        for key, value in self.grades.items():
            for el in value:
                average_grade += el
            count_list += len(value)
            res = 0
            if count_list != 0:
                res = average_grade / count_list
        return res

    def __str__(self):
        self.count_grades_lecturer()
        res = f"Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за лекции:{self.count_grades_lecturer()}"
        return res

    def __lt__(self, other):
        if not isinstance(other, Lecturer):
            print('Not a Lecturer')
            return
        return self.count_grades_lecturer() < other.count_grades_lecturer()


class Reviewer(Mentor):
    # дальше реализуем взаимодействие классов на основе выставления ревьюеров оценок студентам:

    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and (course in student.courses_in_progress
                                             or course in student.finished_courses):
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

    def __str__(self):
        res = f'Имя: {self.name}\nФамилия: {self.surname}'
        return res


student1 = Student('Darya', 'Rybalko', 'women')
student1.finished_courses.append('Git')
student1.courses_in_progress += ['Python']


student2 = Student('Ivan', 'Rybalko', 'man')
student2.finished_courses.append('Git')
student2.courses_in_progress += ['Python']

lectur1 = Lecturer('Oleg', 'Buligun')
lectur1.courses_attached += ['Python']
lectur1.courses_attached += ['Git']

lectur2 = Lecturer('Maksim', 'Nikitin')
lectur2.courses_attached += ['Git']
lectur2.courses_attached += ['Python']

reviewer1 = Reviewer('Andrew', 'Ershov')
reviewer1.rate_hw(student1, 'Python', 10)
reviewer1.rate_hw(student1, 'Git', 10)
reviewer1.rate_hw(student1, 'Python', 8)
reviewer1.rate_hw(student1, 'Git', 7)
reviewer1.rate_hw(student2, 'Python', 10)
reviewer1.rate_hw(student2, 'Git', 10)
reviewer1.rate_hw(student2, 'Python', 10)
reviewer1.rate_hw(student2, 'Git', 9)
print(reviewer1)

reviewer2 = Reviewer('Alexander', 'Ionov')
reviewer2.rate_hw(student1, 'Python', 9)
reviewer2.rate_hw(student1, 'Git', 9)
reviewer2.rate_hw(student1, 'Python', 10)
reviewer2.rate_hw(student1, 'Git', 8)
reviewer2.rate_hw(student2, 'Python', 8)
reviewer2.rate_hw(student2, 'Git', 10)
reviewer2.rate_hw(student2, 'Python', 9)
reviewer2.rate_hw(student2, 'Git', 9)
print(reviewer2)

student1.rate_lectures(lectur1, 'Python', 10)
student1.rate_lectures(lectur1, 'Git', 9)
student1.rate_lectures(lectur1, 'Python', 9)
student1.rate_lectures(lectur1, 'Git', 9)
student1.rate_lectures(lectur2, 'Python', 9)
student1.rate_lectures(lectur2, 'Git', 9)
student1.rate_lectures(lectur2, 'Python', 9)
student1.rate_lectures(lectur2, 'Git', 7)

student2.rate_lectures(lectur1, 'Python', 10)
student2.rate_lectures(lectur1, 'Git', 8)
student2.rate_lectures(lectur1, 'Python', 9)
student2.rate_lectures(lectur1, 'Git', 9)
student2.rate_lectures(lectur2, 'Python', 8)
student2.rate_lectures(lectur2, 'Git', 9)
student2.rate_lectures(lectur2, 'Python', 9)
student2.rate_lectures(lectur2, 'Git', 9)

student1.count_grades_student()
print(student1)
student2.count_grades_student()
print(student2)
print(student1 < student2)
print(lectur1)
print(lectur2)
print(lectur1 < lectur2)


def average_rating(students, course):
    total_assessment = 0    #общая сумма оценки
    number_ratings = 0      #общая колво оценок

    for student in students:
        for key, value in student.grades.items():
            if key == course:
                number_ratings += len(value)
                for el in value:
                    total_assessment += el

    res = 0
    if number_ratings != 0:
        res= total_assessment / number_ratings

    return res


mas_student = [student1, student2]
name_curs = 'Python'
average_rating_st = average_rating(mas_student, name_curs)
print(f"средняя оценка всех студентов по курсу 'Python': {average_rating_st}")

mas_student = [student1, student2]
name_curs = 'Git'
average_rating_st = average_rating(mas_student, name_curs)
print(f"средняя оценка всех студентов по курсу 'Git': {average_rating_st}")


def average_rating_lecturer(lecturer, course):
    total_assessment = 0  # общая сумма оценки
    number_ratings = 0  # общая кол-во оценок
    for lecturs in lecturer:
        for key, value in lecturs.grades.items():
            if key == course:
                number_ratings += len(value)
                for el in value:
                    total_assessment += el

    res = 0
    if number_ratings != 0:
        res = total_assessment / number_ratings

    return res


list_lecturer = [lectur1, lectur2]
name_curs = 'Python'
average_rating_l = average_rating_lecturer(list_lecturer, name_curs)
print(f"средняя оценка всех лекторов по курсу 'Python': {average_rating_l}")

list_lecturer = [lectur1, lectur2]
name_curs = 'Git'
average_rating_l = average_rating_lecturer(list_lecturer, name_curs)
print(f"средняя оценка всех лекторов по курсу 'Git': {average_rating_l}")
```