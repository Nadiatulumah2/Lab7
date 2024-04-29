# lab7_php_ci


![ss1](https://github.com/Nadiatulumah2/Lab7/assets/129835302/b91e0634-860e-437c-adc5-dc34ab1543e0)

![ss2](https://github.com/Nadiatulumah2/Lab7/assets/129835302/2d7521df-2256-4221-b58d-fcc13deda395)


![ss3](https://github.com/Nadiatulumah2/Lab7/assets/129835302/ab8daa38-6916-41b6-9584-b3426ed9ee8d)



Penjelasan :
### Routes.php

```
use CodeIgniter\Router\RouteCollection;
```
Baris ini mengimpor class RouteCollection dari namespace CodeIgniter\Router, yang digunakan untuk mendefinisikan rute dalam aplikasi.

```
$routes->get('/', 'Home::index');
$routes->get('/about', 'Page::about');
$routes->get('/contact', 'Page::contact');
$routes->get('/faqs', 'Page::faqs');
$routes->setAutoRoute(true);
```
1. Rute pertama mengarahkan permintaan HTTP GET ke root website (/) ke metode index pada controller Home.
2. Rute berikutnya mendefinisikan halaman seperti /about, /contact, dan /faqs, yang masing-masing diarahkan ke metode about, contact, dan faqs pada controller Page.
3. setAutoRoute(true) mengizinkan CodeIgniter untuk secara otomatis menemukan controller dan metode yang sesuai berdasarkan URL, jika tidak didefinisikan secara eksplisit dalam konfigurasi rute.

```
$routes->get('artikel', 'Artikel::index');
$routes->get('/artikel/(:any)', 'Artikel::view/$1');
```
Rute artikel mengarahkan ke metode index pada controller Artikel untuk menampilkan daftar artikel.
Rute /artikel/(:any) mendefinisikan parameter dinamis ((:any)) yang mengizinkan segala karakter dan mengarahkan ke metode view pada controller Artikel, di mana $1 mewakili segmen URL yang cocok dengan (:any).

```
$routes->group('admin', function($routes) {
    $routes->get('artikel', 'Artikel::admin_index');
    $routes->add('artikel/add', 'Artikel::add');
    $routes->add('artikel/edit/(:any)', 'Artikel::edit/$1');
    $routes->get('artikel/delete/(:any)', 'Artikel::delete/$1');
});
```
Blok ini mendefinisikan grup rute dengan prefix admin. Semua URL dalam grup ini akan diawali dengan /admin.
artikel dalam grup admin mengarah ke metode admin_index untuk menampilkan dashboard admin artikel.
Rute artikel/add, artikel/edit/(:any), dan artikel/delete/(:any) digunakan untuk menambah, mengedit, dan menghapus artikel. (:any) kembali digunakan sebagai parameter dinamis untuk edit dan delete, masing-masing diikuti oleh metode yang relevan di controller Artikel.

### Artikel.php

```
public function index()
```
Membuat instance dari ArtikelModel.
Mengambil semua artikel yang tersedia menggunakan method findAll().
Mengembalikan sebuah view yang bernama artikel/index, dengan data artikel dan judul halaman.

```
public function view($slug)
```
Metode ini menampilkan detail dari satu artikel berdasarkan slug.
Membuat instance dari ArtikelModel.
Mencari artikel berdasarkan slug dengan menggunakan method where() yang diikuti oleh first() untuk mengambil artikel pertama yang cocok.
Jika artikel tidak ditemukan, akan melempar PageNotFoundException.
Jika artikel ditemukan, mengembalikan view artikel/detail dengan artikel dan judul yang sesuai.

```
public function admin_index()
```
Mirip dengan index(), tetapi untuk tampilan admin.
Mendapatkan semua artikel dan menampilkan dalam konteks admin dengan view artikel/admin_index.

```
public function add()
```
Menggunakan service validation dari CodeIgniter untuk memvalidasi input.
Jika validasi gagal, kembali ke halaman sebelumnya dengan data input dan pesan error.
Jika validasi berhasil, data artikel baru dimasukkan ke database.
Setelah artikel ditambahkan, pengguna diarahkan kembali ke daftar artikel admin.

```
public function edit($id)
```
Metode ini menangani pengeditan artikel yang ada.
Menerima id artikel sebagai parameter.
Melakukan validasi input.
Jika validasi berhasil, mengupdate artikel berdasarkan id.
Pengguna diarahkan kembali ke daftar artikel admin setelah pengeditan.
Jika validasi gagal, mengambil data artikel saat ini dan menampilkannya di form untuk diedit.

```
public function delete($id)
```
Metode ini menangani penghapusan artikel.
Menerima id artikel sebagai parameter.
Menghapus artikel dari database menggunakan method delete().
Setelah penghapusan, mengarahkan pengguna kembali ke daftar artikel admin.
