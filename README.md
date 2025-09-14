Nama: Muhammad Aidil Fadilah\
Nim: 2509116032\
Kelas: Sistem Informasi (A) 2025

**1. PENJELASAN PROGRAM**

# Modul tkinter diimpor untuk membuat antarmuka pengguna grafis (GUI).
from tkinter import *

# Modul random diimpor untuk menghasilkan posisi acak untuk buah.
import random

# Sekumpulan variabel global didefinisikan untuk mengonfigurasi properti permainan.
GAME_WIDTH = 700\
GAME_HEIGHT = 700\
SPEED = 100\
SPACE_SIZE = 25\
BODY_PARTS = 3\
SNAKE_COLOR = "blue"\
FOOD_COLOR = "red"\
BACKGROUND_COLOR = "green"

# Kelas Snake didefinisikan. Ini merepresentasikan ular dalam permainan.
class Snake:

# Metode __init__ adalah konstruktor untuk kelas Snake.
def __init__(self):
    # Mengatur ukuran awal tubuh ular.
    self.body_size = BODY_PARTS
    # Sebuah list untuk menyimpan koordinat (x, y) dari setiap bagian tubuh.
    self.coordinates = []
    # Sebuah list untuk menyimpan objek persegi grafis untuk setiap bagian tubuh.
    self.squares = []

    # Sebuah loop untuk menginisialisasi koordinat ular ke [0, 0] untuk semua bagian tubuh.
    for i in range(0, BODY_PARTS):
        self.coordinates.append([0, 0])

    # Sebuah loop untuk membuat persegi panjang grafis untuk setiap bagian tubuh di kanvas.
    for x, y in self.coordinates:
        # Membuat persegi panjang biru di kanvas dengan ukuran tertentu dan memberinya tag "snake".
        square = canvas.create_rectangle(x, y, x + SPACE_SIZE, y + SPACE_SIZE, fill=SNAKE_COLOR, tag = "snake")
        # Menambahkan objek persegi yang dibuat ke list squares.
        self.squares.append(square)
        
# Kelas Food didefinisikan. Ini merepresentasikan makanan yang dimakan ular.
class Food:

# Metode __init__ adalah konstruktor untuk kelas Food.
def __init__(self):
    
    # Menghasilkan koordinat x acak untuk makanan, disesuaikan dengan grid.
    x = random.randint(0, (GAME_WIDTH // SPACE_SIZE)-1) * SPACE_SIZE
    # Menghasilkan koordinat y acak untuk makanan, disesuaikan dengan grid.
    y = random.randint(0, (GAME_HEIGHT // SPACE_SIZE)-1) * SPACE_SIZE

    # Menyimpan koordinat yang dihasilkan dalam sebuah list.
    self.coordinates = [x, y]

    # Membuat objek oval merah di kanvas untuk merepresentasikan makanan.
    canvas.create_oval(x, y, x + SPACE_SIZE, y + SPACE_SIZE, fill=FOOD_COLOR, tags="food")
    
# Fungsi next_turn menangani logika permainan untuk setiap giliran.
def next_turn(snake, food):

# Mengambil koordinat kepala ular.
x, y = snake.coordinates[0]

# Memperbarui koordinat kepala ular berdasarkan arah saat ini.
if direction == "up":\
    y -= SPACE_SIZE\
elif direction == "down":\
    y += SPACE_SIZE\
elif direction == "left":\
    x -= SPACE_SIZE\
elif direction == "right":\
    x += SPACE_SIZE

# Menyisipkan koordinat kepala baru di awal list.
snake.coordinates.insert(0, (x, y))

# Membuat persegi baru untuk kepala ular yang baru.
square = canvas.create_rectangle(x, y, x + SPACE_SIZE, y + SPACE_SIZE, fill=SNAKE_COLOR)

# Menyisipkan objek persegi baru di awal list squares.
snake.squares.insert(0, square)

# Memeriksa apakah kepala ular telah bertabrakan dengan makanan.
if x == food.coordinates[0] and y == food.coordinates[1]:

    # Mendeklarasikan variabel global score.
    global score

    # Menambah nilai skor.
    score += 1

    # Memperbarui label skor di GUI.
    label.config(text="Score:{}".format(score))

    # Menghapus makanan lama dari kanvas.
    canvas.delete("food")

    # Membuat objek makanan baru di lokasi acak.
    food = Food()

# Jika ular tidak makan makanan, blok ini dieksekusi.
else:
    
    # Menghapus koordinat terakhir dari list coordinates, membuat ular terlihat bergerak.
    del snake.coordinates[-1]

    # Menghapus persegi grafis terakhir dari kanvas.
    canvas.delete(snake.squares[-1])

    # Menghapus objek persegi terakhir dari list squares.
    del snake.squares[-1]

# Memeriksa tabrakan yang mengakhiri permainan.
if check_collision(snake):
    # Memanggil fungsi game_over jika tabrakan terdeteksi.
    game_over()

# Jika tidak ada tabrakan, menjadwalkan giliran berikutnya untuk terjadi setelah waktu tertentu (SPEED).
else:/
    window.after(SPEED, next_turn, snake, food)
    
# Fungsi change_direction menangani perubahan arah ular berdasarkan tombol yang ditekan.
def change_direction(new_direction):

# Mendeklarasikan variabel global direction.
global direction

# Mencegah ular membalikkan arahnya dengan memeriksa arah saat ini.
if new_direction == 'left':\
    if direction != 'right':\
        direction = new_direction\
elif new_direction == 'right':\
    if direction != 'left':\
        direction = new_direction\
elif new_direction == 'up':\
    if direction != 'down':\
        direction = new_direction\
elif new_direction == 'down':\
    if direction != 'up':\
        direction = new_direction
        
# Fungsi check_collision mendeteksi apakah ular menabrak batas window atau dirinya sendiri.
def check_collision(snake):

# Mengambil koordinat kepala ular.
x, y = snake.coordinates[0]

# Memeriksa apakah kepala ular berada di luar batas permainan (menabrak window).
if x < 0 or x >= GAME_WIDTH:\
    print("GAME OVER")\
    # Mengembalikan True jika tabrakan dengan dinding terdeteksi.\
    return True

# Memeriksa tabrakan dengan batas window atas atau bawah.
elif y < 0 or y >= GAME_HEIGHT:\
    print("GAME OVER")\
    # Mengembalikan True jika tabrakan dengan dinding terdeteksi.\
    return True

# Memeriksa apakah kepala ular telah bertabrakan dengan salah satu bagian tubuhnya sendiri.
for body_part in snake.coordinates[1:]:\
    if x == body_part[0] and y == body_part[1]:\
        print("GAME OVER")\
        # Mengembalikan True jika tabrakan dengan tubuh ular terdeteksi.\
        return True
    
# Mengembalikan False jika tidak ada tabrakan yang terdeteksi.
return False\
Fungsi game_over menampilkan pesan "GAME OVER".\
def game_over():\

# Menghapus semua item dari kanvas.
canvas.delete(ALL)
# Membuat objek teks dengan "GAME OVER" di tengah layar.
canvas.create_text(canvas.winfo_width()/2, canvas.winfo_height()/2, font=('consolas', 70), text="GAME OVER", fill="black", tag="gameover")\
Membuat jendela utama untuk GUI.\
window = Tk()

# Mengatur judul window.
window.title("Snake Game")

# Mencegah user yang tidak bersangkutan untuk mengubah ukuran window.
window.resizable(False, False)

# Menginisialisasi variabel global score menjadi 0.
score = 0

# Menginisialisasi variabel global direction menjadi 'down' dan agar saat permainan di mulai ular akan berjalan kearah bawah.
direction = 'down'

# Membuat widget label untuk menampilkan skor.
label = Label(window, text="Score:{}".format(score), font=('consolas', 40))

# Mengemas label ke dalam window, membuatnya terlihat.
label.pack()

# Membuat widget kanvas untuk menggambar elemen permainan.
canvas = Canvas(window, bg=BACKGROUND_COLOR, height=GAME_HEIGHT, width=GAME_WIDTH)

# Mengemas kanvas ke dalam window.
canvas.pack()

# Memaksa window untuk diperbarui, yang diperlukan untuk mendapatkan dimensi yang benar.
window.update()

# Mendapatkan lebar dan tinggi jendela.
window_width = window.winfo_width()

window_height = window.winfo_height()

# Mendapatkan lebar dan tinggi layar pengguna.
screen_widht = window.winfo_screenwidth()

screen_height = window.winfo_screenheight()

# Menghitung koordinat x dan y untuk memusatkan window di layar.
x = int((screen_widht/2) - (window_width/2))

y = int((screen_height/2) - (window_height/2))

# Mengatur geometri window untuk memusatkannya.
window.geometry(f"{window_width}x{window_height}+{x}+{y}")

# Mengikat penekanan tombol panah ke fungsi change_direction.
window.bind('', lambda event: change_direction('left'))\
window.bind('', lambda event: change_direction('right'))\
window.bind('', lambda event: change_direction('up'))\
window.bind('', lambda event: change_direction('down'))\

# Membuat instance dari kelas Snake.
snake = Snake()

# Membuat instance dari kelas Food.
food = Food()

# Memulai loop permainan dengan memanggil fungsi next_turn untuk pertama kalinya.
next_turn(snake, food)

# Memulai loop acara utama, yang mendengarkan input pengguna dan memperbarui GUI.
window.mainloop()

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Hasil dari menggunakan program CRUD.**\
Pertama Create, create beefungsi untuk menciptakan badan ular dan buah.

Kedua Read, program akan memperlihatkan skor yang dapat dilihat user.

Ketiga Update, Update digunakan untuk melakukan pembaruan terhadap penambahan pada badan ular dan nilai skor.

Keempat Delete, Delete digunakan untuk menghapus buah yang telah di makan dan menghapus tubuh ular agar tidak terus terusan memenuhi kolom dan baris yang dilewati.

**Penggunaan List dan Tupple**\
Pertama List, penggunaan list disini berfungsi untuk mengatur kordinat pada tubuh ular karena setiap ular bergerak maka satu kordinat baru akan di tambahkan di depan list dan juga Ketika ular memakan makanan, list ini tumbuh, menambahkan satu segmen tubuh baru. Sifatnya yang dinamis membuat list sangat cocok untuk tugas ini.

Kedua Tupple, penggunaan tupple disini berfungsi untuk mengatur nilai tetap pada nilai x dan y dan juga untuk merepresentasikan nilai tetap seperti koordinat.

<img width="1920" height="1080" alt="Screenshot 2025-09-14 112058" src="https://github.com/user-attachments/assets/fbb16900-8404-44e1-acd6-dce89cb133ca" />

<img width="1920" height="1080" alt="Screenshot 2025-09-14 112147" src="https://github.com/user-attachments/assets/ab04573f-c5db-4227-aac8-c48308ac78b5" />

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**2. PENJELASAN ALUR FLOWCHART**

<img width="524" height="684" alt="Screenshot 2025-09-14 132150" src="https://github.com/user-attachments/assets/c34914a2-c67a-4aa9-8215-8eb0a9379503" />

**Pada bagian pertama flowchart kita bisa memulai program yang telah kita rangkai.**

**Setelah kita bisa langsung memasukan input seperti tombol kenan, kiri, atas, dan bawah.**

**Program akan merespon input dan membuat ular bergerak arah sesuai dengan input yang kita masukan.**

**Pada decision pertama akan dihadapkan dengan pilihan memakan buah atau tidak.**

**Jika pengguna memilih iya makan akan mendapatkan +1 skor dan +1 anggota tubuh ular.**

**Setelah buah di makan maka buah akan terdelete dan muncul kembali secara random di indeks acak.**

**Namun jika tidak maka ular masih akan tetap berjalan.**

**Lalu setelah itu akan dihadapkan dengan decision kedua yaitu, apakah pemain menabrak badan ular atau batas window.**

**jika tidak maka user akan dapat terus mengulang program permainan ular.**

**Namun jika user menabrak batas window ataupun badan ular maka akan mengeluarkan output bertuliskan "GAME OVER".**

**Setelah itu program akan langsung otomatis tertutup**





