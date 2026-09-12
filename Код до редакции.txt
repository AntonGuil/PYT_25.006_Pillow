import tkinter as tk
from tkinter import ttk
# import customtkinter as ctk
from PIL import Image, ImageTk

# метод растягивает изображение по размеру холста
# при этом пропорции исходной картинки нарушаются
def stretch_image(event):
    # измененное изображение объявим глобальной переменной
    global resized_tk

    # получаем размеры холста
    width = event.width
    height = event.height
    # получим изображение с новыми размерами
    resized_image = imgRaccoon_orig.resize((width, height))
    resized_tk = ImageTk.PhotoImage(resized_image)
    # помещаем его на холст
    canvas.create_image(0, 0, image = resized_tk, anchor = 'nw')


# метод выводит картинку в исходной пропорции, при этом обрезает
# те части картикни, которые выходят за габариты холста
def fill_image(event):
    # измененное изображение объявим глобальной переменной
    global resized_tk
    # определим соотношение окна холста
    canvas_ratio = event.width / event.height

    # получим размеры для правильной растяжки
    if canvas_ratio > imgRaccoon_ratio: # холст шире чем картинка
        width = int(event.width)
        height = int(width / imgRaccoon_ratio)
    else: # холст уже чем картинка
        height = int(event.height)
        width = int(height * imgRaccoon_ratio)

    # получим изображение с новыми размерами
    resized_image = imgRaccoon_orig.resize((width, height))
    resized_tk = ImageTk.PhotoImage(resized_image)
    # помещаем его на холст по центру
    canvas.create_image(int(event.width / 2), int(event.height / 2),
                        image = resized_tk, anchor = 'center')

# метод выводит картинку в полном объеме и исходной пропорции
# при этом на холсте остается незаполненное пространство
def show_full_image(event):
    # измененное изображение объявим глобальной переменной
    global resized_tk
    # определим соотношение окна холста
    canvas_ratio = event.width / event.height

    # получим размеры для правильной растяжки
    if canvas_ratio > imgRaccoon_ratio: # холст шире чем картинка
        height = int(event.height)
        width = int(height * imgRaccoon_ratio)
    else: # холст уже чем картинка
        width = int(event.width)
        height = int(width / imgRaccoon_ratio)

    # получим изображение с новыми размерами
    resized_image = imgRaccoon_orig.resize((width, height))
    resized_tk = ImageTk.PhotoImage(resized_image)
    # помещаем его на холст по центру
    canvas.create_image(int(event.width / 2), int(event.height / 2),
                        image = resized_tk, anchor = 'center')

# стартовые настройки окна
window = tk.Tk()
window.geometry('600x400')
window.title('Images')

# создаем стандартное изображение
imgRaccoon_orig = Image.open('Raccoon.jpg')
imgRaccoon_ratio = imgRaccoon_orig.size[0] / imgRaccoon_orig.size[1]
imgRaccoon_tk = ImageTk.PhotoImage(imgRaccoon_orig)

imgPythonDark_orig = Image.open('Python_black.png').resize((30,30))
imgPythonDark_tk = ImageTk.PhotoImage(imgPythonDark_orig)

# создаем кастомизируемое изображение
# в этом случае нужны 2 одинаковых по размеру изображения
# img_ctk = ctk.CTkImage(
#     light_image = Image.open('Python_black.png'),
#     dark_image = Image.open('Python_white.png'))

# создаем 'решетку' для виджетов
window.columnconfigure((0,1,2,3), weight = 1, uniform = 'a')
window.rowconfigure(0, weight = 1)

# Рисуем виджеты кнопок и помещаем изображение

# создаем рамку для кнопок
button_frame = ttk.Frame(window)

button_01 = ttk.Button(button_frame, text = 'Кнопка  ', image = imgPythonDark_tk, compound = 'left')
button_01.pack(pady = 10)

# button_02 = ctk.CTkButton(button_frame, text = 'Кнопка  ', image = img_ctk, compound = 'left')
# button_02.pack(pady = 10)

button_frame.grid(column = 0, row = 0, sticky = 'nsew')

# создадим холст для картинок и встроим его в решетку
canvas = tk.Canvas(window,
            background = 'green', bd = 0, highlightthickness = 0, relief = 'ridge')

canvas.grid(column = 1, columnspan = 3, row = 0, sticky = 'nsew')

# создаем изображение на холсте, оно должно быть в формате tk!
# canvas.create_image(0, 0, image = imgRaccoon_tk, anchor = 'nw')

# повесим на холст функцию изменения размера изображения
# canvas.bind('<Configure>', stretch_image)
# canvas.bind('<Configure>', fill_image)
canvas.bind('<Configure>', show_full_image)

# Рабочий цикл окна
window.mainloop()

# Тест работы системы Git