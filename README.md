# Polimorfizm
1 - задание
class Mentor:
    def __init__(self, name, surname):
        self.name=name
        self.surname=surname
        self.courses_attached = []
class Lecturer(Mentor):
    pass
class Reviewer(Mentor):
    pass
lecturer = Lecturer('Иван', 'Иванов')
reviewer = Reviewer('Пётр', 'Петров')
print(isinstance(lecturer, Mentor)) # True
print(isinstance(reviewer, Mentor)) # True
print(lecturer.courses_attached)    # []
print(reviewer.courses_attached)    # []

2-задание
class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}
    def rate_lecture(self, lecturer, course, grade):
        if not isinstance(lecturer, Lecturer):
           return 'Ошибка'
        if course not in self.courses_in_progress:
            return 'Ошибка'
        if course not in lecturer.courses_attached:
            return 'Ошибка'
        if not isinstance(grade, (int, float)) or grade < 1 or grade > 10:
           return 'Ошибка'
        if course in lecturer.grades:
            lecturer.grades[course].append(grade)
        else:
            lecturer.grades[course] = [grade]
        return None
class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []
class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name,surname)
        self.grades = {}

class Reviewer(Mentor):
     def rate_hw(self, student, course,grade):
         if isinstance(student, Student) and \
            course in self.courses_attached and \
            course in student.courses_in_progress:
             if course in student.grades:
                 student.grades[course] += [grade]
             else:
                 student.grades[course] = [grade]
             return 'Ошибка'
         else:
              return None

lecturer = Lecturer('Иван', 'Иванов')
reviewer = Reviewer('Пётр', 'Петров')
student = Student('Алёхина', 'Ольга', 'Ж')

student.courses_in_progress += ['Python', 'Java']
lecturer.courses_attached += ['Python', 'C++']
reviewer.courses_attached += ['Python', 'C++']

print(student.rate_lecture(lecturer, 'Python', 7))   # None
print(student.rate_lecture(lecturer, 'Java', 8))     # Ошибка
print(student.rate_lecture(lecturer, 'С++', 8))      # Ошибка
print(student.rate_lecture(reviewer, 'Python', 6))   # Ошибка

print(lecturer.grades)  # {'Python': [7]}





3-задание
class Reviewer:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
    def __str__(self):
        return f"Имя: {self.name}\n Фамилия: {self.surname}\n\n"
some_reviewer=Reviewer("Some","Buddy")


class Lecturer:
    def __init__(self, name, surname, avarage_lecture_grade):
        self.name = name
        self.surname = surname
        self.avarage_lecture_grade = avarage_lecture_grade
    def __str__(self):
        return (f"Имя: {self.name}\n "
                f"Фамилия: {self.surname}\n "
                f"Средняя оценка за лекции: {self.avarage_lecture_grade}\n")
    def __gt__(self, other):
        if not isinstance(other, Lecturer):
            return NotImplemented
        return self.avarage_lecture_grade > other.avarage_lecture_grade

    def __eq__(self, other):
        if not isinstance(other, Lecturer):
            return NotImplemented
        return self.avarage_lecture_grade == self.avarage_lecture_grade


class Student:
    def __init__(self, name, surname, avarage_rate, courses, finished_courses):
        self.name = name
        self.surname = surname
        self.avarage_rate = avarage_rate
        self.courses = courses
        self.finished_courses=finished_courses
    def __str__(self):
        return (f"Имя: {self.name}\n "
                f"Фамилия: {self.surname}\n"
                f" Средняя оценка за домашние задания: {self.avarage_rate}\n"
                f" Курсы в процессе изучения: {self.courses}\n "
                f"Завершенные курсы: {self.finished_courses}\n")
def __gt__(self, other):
        if not isinstance(other, Student):
          return NotImplemented
        return self.avarage_rate > other.avarage_rate
def __eq__(self, other):
        if not isinstance(other, Student):
          return NotImplemented
        return self.avarage_rate == other.avarage_rate
some_student = Student("Ruoy","Eman", "9.9", "Python, Git", "Введение в программирование")
another_student = Student("Ron", "Potter", "8.5", "Python", "Введение в программирование")
some_lecturer=Lecturer("Some","Buddy", "9.9" )
another_lecturer=Lecturer("Sam", "Baddy", "7.5")
some_lecturer1=9.9
another_lecturer1=7.5
some_student1=9.9
another_student1=8.5

print(some_reviewer)
print(some_student)
print(some_lecturer1 > another_lecturer1)
print(another_lecturer1 > some_lecturer1)
print(some_student1 > another_student1)
print(another_student1 > some_student1)




4-задание
class Student:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_in_progress = []
        self.finished_courses = []
        self.grades = {}

    def __str__(self):
        avg = self.average_grade()
        courses = ", ".join(self.courses_in_progress) or "нет"
        finished = ", ".join(self.finished_courses) or "нет"
        return (f"Имя: {self.name}\n"
               f"Фамилия: {self.surname}\n"
               f"Средняя оценка за домашние задания : {avg}\n"
               f"Курсы в процессе изучения: {courses}\n"
               f"Завершенные курсы:{finished}")

    def average_grade(self):
        all_grades = []
        for grades_list in self.grades.values():
            all_grades.extend(grades_list)
        return round(sum(all_grades) / len(all_grades), 2) if all_grades else 0

    def rate_lecturer(self, lecturer, course, grade):
        if isinstance(lecturer, Lecturer) and course in self.courses_in_progress and course in lecturer.courses_attached:
            lecturer.grades.setdefault(course, []).append(grade)
        else:
            print(f"Ошибка: оценка для лектора {lecturer.name} {lecturer.surname} не добавлена. Проверьте курс или тип объекта.")

class Lecturer:
     def __init__(self, name, surname):
         self.name = name
         self.surname = surname
         self.courses_attached = []
         self.grades = {}

     def __str__(self):
         avg = self.average_grade()
         return (f"Имя: {self.name}\n"
                 f"Фамилия: {self.surname}\n"  
                 f"Средняя оценка за лекции: {avg}")


     def average_grade(self):
         all_grades = []
         for grades_list in self.grades.values():
             all_grades.extend(grades_list)
         return round(sum(all_grades) / len(all_grades), 2) if all_grades else 0


class Reviewer:
      def __init__(self, name, surname):
          self.name = name
          self.surname = surname
          self.courses_attached = []

      def __str__(self):
          return (f"Имя: {self.name}\n"
                 f"Фамилия: {self.surname}")

      def rate_hw(self, student, course, grade):
          if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
             student.grades.setdefault(course, []).append(grade)
          else:
             print(f"Ошибка: оценка для студента {student.name} {student.surname} не добавлена. Проверьте курс или тип объекта.")


def avg_grade_students(students, course):
    grades = []
    for student in students:
        if course in student.grades:
            grades.extend(student.grades[course])
    return round(sum(grades) / len(grades), 2) if grades else 0

def avg_grade_lecturers(lecturers, course):
     grades = []
     for lecturer in lecturers:
         if course in lecturer.grades:
             grades.extend(lecturer.grades[course])
     return round(sum(grades) / len(grades), 2) if grades else 0

student1 = Student("Никита", "Иванов")
student1.courses_in_progress = ["Python", "Git"]
student1.finished_courses = ["Введение в программирование"]

student2 = Student("Лана", "Иванова")
student2.courses_in_progress = ["Python", "Java"]
student2.finished_courses = ["Введение в программирование"]

lecturer1 = Lecturer("Иван", "Петров")
lecturer1.courses_attached = ["Python", "Git"]

lecturer2 = Lecturer("Сергей", "Крюков")
lecturer2.courses_attached = ["Python", "Java"]

reviewer1 = Reviewer("Александр", "Васильев")
reviewer1.courses_attached = ["Python", "Git"]

reviewer2 = Reviewer("Людмила", "Денисова")
reviewer2.courses_attached = ["Python", "Java"]

reviewer1.rate_hw(student1, "Python", 10)
reviewer1.rate_hw(student1, "Python", 9)
reviewer1.rate_hw(student1, "Git", 8)

reviewer2.rate_hw(student2, "Python", 9)
reviewer2.rate_hw(student2, "Python", 8)
reviewer2.rate_hw(student2, "Java", 10)

student1.rate_lecturer(lecturer1, "Python", 10)
student1.rate_lecturer(lecturer1, "Git", 9)

student2.rate_lecturer(lecturer1, "Python", 9)
student2.rate_lecturer(lecturer2, "Python", 8)
student2.rate_lecturer(lecturer2, "Java", 9)
print(student1)
print(lecturer1)
print(f"Средняя оценка студентов по Python: {avg_grade_students([student1, student2], 'Python')}")
print(f"Средняя оценка лекторов по Python: {avg_grade_lecturers([lecturer1, lecturer2], 'Python')}")






