# Электронный дневник школы

Этот сайт - интерфейс для учеников школы. Здесь можно посмотреть оценки, расписание и прочую открытую информацию. Учителя заполняют базу данных через другой сайт. Ставят там оценки и т.д.
## Запуск сервера

- Скачайте код
- Установите зависимости командой `pip install -r requirements.txt`
- Создайте БД командой `python3 manage.py migrate`
- Запустите сервер командой `python3 manage.py runserver`
## Запуск команды
- `python manage.py shell`
- `from datacenter.models import Mark,Schoolkid,Chastisement,Lesson,Commendation`
- `from django.core.exceptions import ObjectDoesNotExist,MultipleObjectsReturned`
- `import random`
- `arguments=["Молодец!","Отлично!","Хорошо!","Гораздо лучше, чем я ожидал!","Ты сегодня прыгнул выше головы!","С каждым разом у тебя получается всё лучше!","Я поражен!","Потрясающе!","Замечательно","Здорово!"]`

## Функции
### **Предупреждения имя и урок должно быть в " "**
### Изменения оценок:
```python
def fix_marks(name):
    try:
        kid = Schoolkid.objects.get(full_name__contains=name)
        bat_marks = Mark.objects.filter(schoolkid=kid,points__in = [2,3,4])
        for bat_mark in bat_marks:
             bat_mark.points = 5
             bat_mark.save()
    except DoesNotExist:
        print(f"Кря,ученик по имени {name} не найден проверьте имя!")
    except .MultipleObjectsReturned:
        print(f"Кря,уточните имя ученика")
```
Запуск функции:
`fix_marks("имя")`
### Удаление жалоб:
```python
def remove_chastisements(name):
    try:
        kid = Schoolkid.objects.get(full_name__contains=name)
        comments = Chastisement.objects.filter(schoolkid=kid)
        for comment in comments:
            comment.delete()
    except DoesNotExist:
        print(f"Кря,ученик по имени {name} не найден проверьте имя!")
    except MultipleObjectsReturned:
        print(f"Кря,уточните имя ученика")
```
Запуск функции:
`remove_chastisements("имя")`
### Создание похвали от учителя:
```python
def create_commendation(name,subject):
    try:
        kid = Schoolkid.objects.get(full_name__contains=name)
        lessons = Lesson.objects.filter(group_letter=kid.group_letter,year_of_study=kid.year_of_study,subject__title=subject)
        Commendation.objects.all().create(created=lessons[random.randint(0,len(lessons))].date,schoolkid=kid,teacher=lessons[0].teacher,subject=lessons[0].subject,text=random.choice(arguments))
    except ObjectDoesNotExist:
            print(f"Кря,ученик по имени {name} или урок не найден проверьте имя!")
    except MultipleObjectsReturned:
            print(f"Кря,уточните имя ученика")
```
Запуск функции:
`create_commendation("имя","урок")`
## Туториалы
[Туториалы](https://www.youtube.com/watch?v=34Rp6KVGIEM&list=PLDyJYA6aTY1lPWXBPk0gw6gR8fEtPDGKa)
## Цели проекта

Код написан в учебных целях — это урок в курсе по Python и веб-разработке на сайте [Devman](https://dvmn.org).

