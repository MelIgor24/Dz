# Домашние задания 1-4

    class Student:
    student_list = []

    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}
        self.average_grades = 0
        Student.student_list.append(self)

    def add_courses(self, course_name):
        self.finished_courses.append(course_name)

    def rate_lecturer(self, lecturer, course, grade):
        if isinstance(lecturer, Lecturer) and course in lecturer.courses_attached and course in self.courses_in_progress and 0 <= grade <= 10:
            if course in lecturer.grades:
                lecturer.grades[course] += [grade]
            else:
                lecturer.grades[course] = [grade]
        else:
            return 'Ошибка'

    def average_rating(self):
        if sum([len(i) for i in self.grades.values()]) > 0:
            result = (sum([sum(i) for i in self.grades.values()]) /
                      sum([len(i) for i in self.grades.values()]))
            self.average_grades = result
            return result

    def __str__(self):
        result = f'Имя: {self.name}\nФамилия: {self.surname}\n' \
                 f'Средняя оценка за домашние задания: {self.average_grades}\n' \
                 f'Курсы в процессе изучения: {", ".join(self.courses_in_progress)}\n' \
                 f'Завершенные курсы: {", ".join(self.finished_courses)}'
        return result

    def __gt__(self, other):
        if isinstance(other, Student):
            return self.average_rating() > other.average_rating()
        print('Ошибка')
        return

    def __eq__(self, other):
        if isinstance(other, Student):
            return self.average_rating() == other.average_rating()
        print('Ошибка')
        return



    class Mentor:
        def __init__(self, name, surname):
            self.name = name
            self.surname = surname
            self.courses_attached = []

        def add_coursese(self, course_names):
            self.courses_attached.append(course_names)


    class Lecturer(Mentor):
        lecturer_list = []

        def __init__(self, name, surname):
            super().__init__(name, surname)
            self.grades = {}
            self.average_grades_lecturer = 0
            Lecturer.lecturer_list.append(self)

        def average_rating(self):
            if sum([len(i) for i in self.grades.values()]) > 0:
                result = (sum([sum(i) for i in self.grades.values()]) /
                        sum([len(i) for i in self.grades.values()]))
                self.average_grades_lecturer = result
                return result

        def __str__(self):
            res = f'Имя: {self.name}\nФамилия: {self.surname}\n' \
                f'Средняя оценка за лекции:  {self.average_grades_lecturer}'
            return res

        def __gt__(self, other):
            if isinstance(other, Lecturer):
                return self.average_rating() > other.average_rating()
            print('Ошибка')
            return

        def __eq__(self, other):
            if isinstance(other, Lecturer):
                return self.average_rating() == other.average_rating()
            print('Ошибка')
            return


    class Reviewer(Mentor):
        reviewer_list = []

        def __init__(self, name, surname):
            super().__init__(name, surname)
            Reviewer.reviewer_list.append(self)

        def rate_hw(self, student, course, grade):
            if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
                if course in student.grades:
                    student.grades[course] += [grade]
                else:
                    student.grades[course] = [grade]
            else:
                return 'Ошибка'

        def __str__(self):
            res = f'Имя: {self.name}\nФамилия: {self.surname}'
            return res




    one_student = Student('Александр', 'Петров', 'муж')
    one_student.courses_in_progress += ['Java', 'Python']
    two_student = Student('Елена', 'Круглова', 'жен')
    two_student.courses_in_progress += ['Python', 'Java']

    one_reviewer = Reviewer('Владимир', 'Иванов')
    one_reviewer.courses_attached += ['Java', 'Python']
    two_reviewer = Reviewer('Юлия', 'Васильева')
    two_reviewer.courses_attached += ['Java', 'Python']
    one_lecturer = Lecturer('Константин', 'Белобородов')
    one_lecturer.courses_attached += ['Java', 'Python']
    two_lecturer = Lecturer('Анна', 'Донскова')
    two_lecturer.courses_attached += ['Java', 'Python']

    one_reviewer.rate_hw(one_student, 'Python', 9)
    one_reviewer.rate_hw(one_student, 'Python', 8)
    one_reviewer.rate_hw(two_student, 'Python', 6)
    one_reviewer.rate_hw(two_student, 'Python', 8)
    two_reviewer.rate_hw(one_student, 'Java', 10)
    two_reviewer.rate_hw(one_student, 'Java', 10)
    two_reviewer.rate_hw(two_student, 'Java', 8)
    two_reviewer.rate_hw(two_student, 'Java', 8)

    one_student.rate_lecturer(one_lecturer, 'Python', 9)
    one_student.rate_lecturer(one_lecturer, 'Python', 8)
    one_student.rate_lecturer(two_lecturer, 'Python', 8)
    one_student.rate_lecturer(two_lecturer, 'Python', 9)
    two_student.rate_lecturer(two_lecturer, 'Java', 9)
    two_student.rate_lecturer(two_lecturer, 'Java', 8)
    two_student.rate_lecturer(one_lecturer, 'Java', 7)
    two_student.rate_lecturer(one_lecturer, 'Java', 7)

    student_list = [one_student, two_student]
    reviewer_list = [one_reviewer, two_reviewer]
    lecturer_list = [one_lecturer, two_lecturer]

    [instance.average_rating() for instance in Student.student_list]
    [instance.average_rating() for instance in Lecturer.lecturer_list]

    one_student.add_courses('C++')
    two_student.add_courses('C++')


    print(one_student)
    print()
    print(two_student)
    print()
    print(one_lecturer)
    print()
    print(two_lecturer)
    print()
    print(one_reviewer)
    print()
    print(two_reviewer)
    print()
    print(f'Результат сравнения студентов (по средним оценкам за ДЗ):'f'{one_student.name} {one_student.surname} > {two_student.name} {two_student.surname}')
    print(one_student.__gt__(two_student))
    print()
    print(f'Результат сравнения лекторов (по средним оценкам за лекции): 'f'{one_lecturer.name} {one_lecturer.surname} > {two_lecturer.name} {two_lecturer.surname}')
    print(one_lecturer.__gt__(two_lecturer))
    print()


    def average_grade_in_class(class_name, course):

        list_instances = [_ for _ in globals().values() if isinstance(_, class_name)]
        list_grades = [_.grades[course] for _ in list_instances if course in _.grades.keys()]
        average_grade = sum([sum(i) for i in list_grades]) / sum([len(i) for i in list_grades])
        return f'Средняя оценка среди {class_name.__name__} по предмету {course} - {average_grade}'

    print(average_grade_in_class(Lecturer, 'Java'))
    print(average_grade_in_class(Student, 'Python'))
    print(average_grade_in_class(Lecturer, 'Python'))
    print(average_grade_in_class(Student, 'Java'))