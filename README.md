# -Image-editor.py
import os
import shutil
PathToFirstFolder = r'C:\Downlads_py' # Указываем путь к первой директории
PathToSecondFolder = r'C:\Users\Мария\PycharmProjects\untitled' # Указывем путь к второй директории
FilesOnFirstFolder = [] # Список файлов в первой директории
FilesOnSecondFolder = [] # Список файлов во второй директории
ResultFilesOnFirstFolder = [] # Уникальные файлы в первой директории
ResultFilesOnSecondFolder= [] # Уникальные файлы во второй директории
#Формируем список файлов в первой директории
#Проверяем существование директорий:
#первой директории
if os.path.exists(PathToFirstFolder) == False:
	print ("Путь к директории",PathToFirstFolder,"не найден, проверте правильность пути")
#второй директории
elif os.path.exists(PathToSecondFolder) == False:
        print ("Путь к директории",PathToSecondFolder,"не найден, проверте правильность пути")
#Если директории существуют, то формируем списки файлов:
else:
	#Сканируем содержимое первой директории, включая поддиректории
	for root, dirs, files in os.walk(PathToFirstFolder):
		for name in files:
			#Добавляем в список относительные пути к файлам первой директории
			FilesOnFirstFolder.append(os.path.join(root, name)[len(PathToFirstFolder):])
	#Дублируем записи из списка файлов первой директории в список ункиальных файлов первой директории
	ResultFilesOnFirstFolder = FilesOnFirstFolder
	#Сканируем содержимое второй директории, включая поддиректории
	for root, dirs, files in os.walk(PathToSecondFolder):
		for name in files:
			#Добавляем в список относительные пути к файлам второй директории
			FilesOnSecondFolder.append(os.path.join(root, name)[len(PathToSecondFolder):])
	#Дублируем записи из списка файлов второй директории в список уникальных файлов второй директории
	ResultFilesOnSecondFolder = FilesOnSecondFolder
	#Удаляем записи из списка уникальных файлов первой директории, которые есть в списке файлов второй директории
	#Проходим по списку файлов второй директории
	for i in FilesOnSecondFolder:
		#Если такая запись есть в списке уникальных файлов первой директории, то
		if i in ResultFilesOnFirstFolder:
				#Удаляем её из списка уникальных файлов первой директории
				ResultFilesOnFirstFolder.remove(i)
	#Удаляем записи из списка уникальных файлов второй директории, которые есть в списке файлов первой директории
	#Проходим по списку файлов первой директории
	for i in FilesOnFirstFolder:
		#Если такая запись есть в списке уникальных файлов второй директории, то
		if i in ResultFilesOnSecondFolder:
				#Удаляем её из списка уникальных файлов второй директории
				ResultFilesOnSecondFolder.remove(i)
	#Выводим на экран список уникальных файлов в первой директории
	print ()
	print ('############################')
	print ('Список недостающих файлов в '+PathToSecondFolder+':')
	print ()
    # Проходим по списку уникальных файлов первой директории
for i in ResultFilesOnFirstFolder:
    path = os.path.realpath(PathToSecondFolder + i)
    dir = os.path.split(path)[-2]
    if not os.path.exists(dir):
        os.makedirs(dir)
    #  копируем их во вторую директорию и выводим на экран имена
    shutil.copy2(PathToFirstFolder + i, path)
    files = os.listdir(PathToFirstFolder)
    foto=files[0]
    print(foto)

#### ее, мы наконец переходим к фоторедактору непосредственно

import random
from PIL import Image, ImageFilter, ImageDraw

new_img1 = Image.open(foto)
draw = ImageDraw.Draw(new_img1) #Создаем инструмент для рисования.
width = new_img1.size[0] #Определяем ширину.
height = new_img1.size[1] #Определяем высоту.
pix = new_img1.load() #Выгружаем значения пикселей.



# второе действие - накладывает черно-белый фильтр
print('Хотите наложить черно-белый фильтр?')
ans2 = input()
ans2 = str(ans2)
new_img2 = new_img1
if ( ans2 == 'да' or ans2 == 'Да'):
        r, g, b = (new_img1.split())
        b.show()
# воспользоваться стандартным фильтром из библ Python 
print('Хотите воспользоваться фильтром?')
ans4 = input()
ans4 = str(ans4)
new_img4 = new_img2
if (ans4 == 'да' or ans4 == 'Да'):
    print('Напишите номер фильтра: ')
    print('1)BLUR,2)CONTOUR, 3)DETAIL, 4)EDGE_ENHANCE, 5)EDGE_ENHANCE_MORE, 6)EMBOSS, 7)FIND_EDGES, 8)SMOOTH, 9)SMOOTH_MORE, 10)SHARPEN')
    num = input()
    num = int(num)
    filter = {1:ImageFilter.BLUR, 2:ImageFilter.CONTOUR, 3:ImageFilter.DETAIL, 4:ImageFilter.EDGE_ENHANCE, 5:ImageFilter.EDGE_ENHANCE_MORE, 6:ImageFilter.EMBOSS, 7:ImageFilter.FIND_EDGES, 8:ImageFilter.SMOOTH, 9:ImageFilter.SMOOTH_MORE, 10:ImageFilter.SHARPEN}
    f = filter[num]
    new_img4 =new_img2.filter(f) #подстановка вабранного фильтра,  тут надо продумать!
       # если не сработает, то можно через if - ы n = 1, то  фильтр такой-то...
    new_img4.show()
else:
     new_img4.show()



print('Хотите применить фильтр "Негатив"')
ans5 = input()
ans5 = str(ans5)
new_img5 = new_img4
if (ans5 == 'да' or ans5 == 'Да'):
	factor = int(input('factor:'))
	for i in range(width):
		for j in range(height):
			rand = random.randint(-factor, factor)
			a = pix[i, j][0] + rand
			b = pix[i, j][1] + rand
			c = pix[i, j][2] + rand
			if (a < 0):
				a = 0
			if (b < 0):
				b = 0
			if (c < 0):
				c = 0
			if (a > 255):
				a = 255
			if (b > 255):
				b = 255
			if (c > 255):
				c = 255
			draw.point((i, j), (a, b, c))
new_img5.show()


# удаление файла из папки для возможности его последующего использования
os.remove(foto)
