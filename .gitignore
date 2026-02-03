#создай тут фоторедактор Easy Editor!
from PyQt5.QtCore import Qt
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton, QListWidget, QLabel, QHBoxLayout, QVBoxLayout, QFileDialog
from PyQt5.QtGui import QPixmap
from PIL import Image
from PIL import ImageOps
from PIL import ImageFilter
import os

app = QApplication([])
win = QWidget()
win.setWindowTitle('Easy Editor')
win.resize(700, 400)

button_dir = QPushButton('Папка')
list_dir = QListWidget()
photo = QLabel('Картинка')

left = QPushButton('Лево')
right = QPushButton('Право')
mirror_button = QPushButton('Зеркало')
rez = QPushButton('Резкость')
bw = QPushButton('Ч/Б')
save = QPushButton('Сохранить')
sbros = QPushButton('Сбросить фильтры')

os_h = QHBoxLayout()
row = QHBoxLayout()

dir_vert = QVBoxLayout()
button_vert = QVBoxLayout()

row.addWidget(left)
row.addWidget(right)
row.addWidget(mirror_button)
row.addWidget(rez)
row.addWidget(bw)
row.addWidget(save)
row.addWidget(sbros)

dir_vert.addWidget(button_dir)
dir_vert.addWidget(list_dir)

button_vert.addWidget(photo)
button_vert.addLayout(row)

os_h.addLayout(dir_vert)
os_h.addLayout(button_vert)

win.setLayout(os_h)

workdir = ''

def chooseWorkdir():
    global workdir
    workdir = QFileDialog.getExistingDirectory()


def filter(files, extension):
    result = list()
    for i in files:
        if i.endswith(extension):
            result.append(i)
    return result

def showFilenamesList():
    extension2 = ('png', 'jpg', 'gif', 'svg')
    chooseWorkdir()
    result = filter(os.listdir(workdir), extension2)
    list_dir.clear()
    for i in result:
        list_dir.addItem(i)

class ImageProcess():
    def __init__(self):
        self.image = None
        self.filename = None
        self.modified_dir = "Modified/"
        self.dir = None
        self.rezerve_image = None
        
    def LoadImage(self, filename, work):
        self.filename = filename
        self.dir = work
        image_dir = os.path.join(self.dir, filename)
        self.image = Image.open(image_dir)
        self.rezerve_image = self.image

    def showImage(self, image):
        photo.hide()
        pixmapImage = QPixmap(image)
        w , h = photo.width(), photo.height()
        pixmapImage = pixmapImage.scaled(w,h, Qt.KeepAspectRatio)
        photo.setPixmap(pixmapImage)
        photo.show()

    def saveImage(self):
        path = os.path.join(self.dir, self.modified_dir)
        if not(os.path.exists(path) or os.path.isdir(path)):
            os.mkdir(path)
        image_path = os.path.join(path, self.filename)
        self.image.save(image_path)

    def do_bw(self):
        self.image = self.image.convert('L')
        self.saveImage()
        image_path = os.path.join(self.dir, self.modified_dir ,self.filename)
        self.showImage(image_path)
    
    def do_mirror(self):
        self.image = self.image.transpose(Image.FLIP_LEFT_RIGHT)
        self.saveImage()
        image_path = os.path.join(self.dir, self.modified_dir ,self.filename)
        self.showImage(image_path)

    def do_Flip_left(self):
        self.image = self.image.rotate(90)
        self.saveImage()
        image_path = os.path.join(self.dir, self.modified_dir ,self.filename)
        self.showImage(image_path)

    def do_Flip_right(self):
        self.image = self.image.rotate(-90)
        self.saveImage()
        image_path = os.path.join(self.dir, self.modified_dir ,self.filename)
        self.showImage(image_path)

    def rezkos(self):
        self.image = self.image.filter(ImageFilter.SHARPEN)
        self.saveImage()
        image_path = os.path.join(self.dir, self.modified_dir ,self.filename)
        self.showImage(image_path)

    def reset(self):
       self.image = self.rezerve_image
       self.saveImage()
       image_path = os.path.join(self.dir, self.modified_dir ,self.filename)
       self.showImage(image_path)
workimage = ImageProcess()

def showChoosenImage():
    if list_dir.currentRow() >= 0:
        filename = list_dir.currentItem().text()
        workimage.LoadImage(filename, workdir)
        image_path = os.path.join(workimage.dir, workimage.filename)
        workimage.showImage(image_path)

list_dir.currentRowChanged.connect(showChoosenImage)
button_dir.clicked.connect(showFilenamesList)
bw.clicked.connect(workimage.do_bw)
save.clicked.connect(workimage.saveImage)
mirror_button.clicked.connect(workimage.do_mirror)
left.clicked.connect(workimage.do_Flip_left)
right.clicked.connect(workimage.do_Flip_right)
rez.clicked.connect(workimage.rezkos)
sbros.clicked.connect(workimage.reset)

win.show()
app.exec_()
