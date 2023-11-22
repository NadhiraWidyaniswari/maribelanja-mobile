# **Tugas Pemrograman Berbasis Platform**
Nadhira Widyaniswari - 2206811884 - E
---
## Tugas 7

### Apa perbedaan utama antara stateless dan stateful widget dalam konteks pengembangan aplikasi Flutter?
Menurut saya state itu seperti data yang digunakan untuk menampilkan kondisi dari sebuah widget dalam waktu tertentu. Dimana hal tersebut bisa saja berubah apabila seseorang menekan button, menambah produk ataupun lainnya. Terdapat dua jenis dasar dalam flutter yaitu
- Stateless widget 
    - Widget ini memiliki tampilan yang statis dan konstan (tidak berubah selama berjalannya aplikasi). Saya mengerti mengapa tampilan utama halaman sebaiknya menggunakan stateless widget karena ikon dari nama aplikasi, button berupa cards memiliki sifat yang tidak berubah. Tidak memiliki state internal
- Statefull Widget
    - Widget ini memliki state internal yang dapat berubah selama berjalannya widget. Tipe state ini cocok digunakan untuk halaman web yang dapat berubah ketika dimasukkan sesuatu, seperti form, maupun perubahan TextField. Memiliki sifat dinamis.


### Sebutkan seluruh widget yang kamu gunakan untuk menyelesaikan tugas ini dan jelaskan fungsinya masing-masing.
Beberapa widget yang digunakan dalam menu.dart dan main.dart adalah
- MaterialApp = digunakan untuk inisialisasi aplikasi flutter yang memuat konfigurasi global
- Scaffold = merupakan kerangka dasar aplikasi yang menjadi dua bagian yaitu navigasi bar dan halaman utama pada web
- MyHomePage(stateless Widget) = merupakan tampilan halaman utama aplikasi
- ColorScheme = merupakan pengaturan untuk warna-warna tampilan dalam aplikasi
- runApp = digunakan untuk menjalankan aplikasi flutter dengan widget myApp sebagai root aplikasi
- List<ShopItem> items = daftar item dimana pada kasus ini berisi informasi mengenai nama button, icon, dan color
- AppBar = navigasi bar
- SingleChildScrollView = widget yang memungkinkan untuk scrolling apabila halaman utama tidak cukup dalam layar
- Padding = menambah jarak dari tepi widget yang dimaksud
- Column = menampilkan widget secara mendatar
    - Text = menampilkan text
    - GridView.count = menampilkan bentuk button dimana mengambil items dari list items
- ShopItem = seperti class pada java yang menyimpan atribut button
- Shopcard = widget untuk menampilkan Shop Item dan memberikan notifikasi Snackbar apabila button dipencet.

### Jelaskan bagaimana cara kamu mengimplementasikan checklist di atas secara step-by-step (bukan hanya sekadar mengikuti tutorial)
- Langkah pertama yang saya lakukan adalah membuat program baru flutter dengan `flutter create maribelanja` yang merupakan nama aplikasi yang saya kembangkan sebelumnya dan melakukan sinkronisasi dengan `git init`dengan repository baru bernama `maribelanja-mobile`. Setelah itu saya coba melakukan perintah `flutter run` pada root aplikasi dan muncul tampilan default yang perlu dirubah dengan membuka pada folder lib yaitu `main.dart`
- Setelah itu saya merapihkan struktur proyek dengan cara memisahkan kerangka utama dengan konten halaman aplikasi dengan memecah file `main.dart` dan `menu.dart`. Pada menu dart kita pindahkan isi konten dari `main.dart`
- Kustomisasi tema pada main.dart, disini saya menambahkan warna menjadi indigo. Setelah itu buka file `menu.dart` dan rubah widget halaman utama dari stateful menjadi stateless dengan menghapus fungsi state bawaan dari flutter dan merubah kode `({super.key, required this.title})` menjadi `({Key? key}) : super(key: key);`. Hingga kode menjadi,
```dart
class MyHomePage extends StatelessWidget {
    MyHomePage({Key? key}) : super(key: key);

    @override
    Widget build(BuildContext context) {
        return Scaffold(
            ...
        );
    }
}
```
- Lalu saya membuat class ShopItem yang saya tambahkan atribut color agar setiap button memiliki warna yang berbeda. Dan saya buat sebuat struktur data yang dapat menampung item-item button dan saya tambahkan kerangka aplikasi dengan widget Scafforld
```dart
return Scaffold(
      appBar: AppBar(
        title: const Text(
          'Shopping List',
        ),
      ),
      body: SingleChildScrollView(
        // Widget wrapper yang dapat discroll
        child: Padding(
          padding: const EdgeInsets.all(10.0), // Set padding dari halaman
          child: Column(
            // Widget untuk menampilkan children secara vertikal
            children: <Widget>[
              const Padding(
                padding: EdgeInsets.only(top: 10.0, bottom: 10.0),
                // Widget Text untuk menampilkan tulisan dengan alignment center dan style yang sesuai
                child: Text(
                  'PBP Shop', // Text yang menandakan toko
                  textAlign: TextAlign.center,
                  style: TextStyle(
                    fontSize: 30,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
              // Grid layout
              GridView.count(
                // Container pada card kita.
                primary: true,
                padding: const EdgeInsets.all(20),
                crossAxisSpacing: 10,
                mainAxisSpacing: 10,
                crossAxisCount: 3,
                shrinkWrap: true,
                children: items.map((ShopItem item) {
                  // Iterasi untuk setiap item
                  return ShopCard(item);
                }).toList(),
              ),
            ],
          ),
        ),
      ),
    );
```
lalu menambahkan widget stateless untuk menampilkan button dalam card
```dart
class ShopCard extends StatelessWidget {
  final ShopItem item;

  const ShopCard(this.item, {super.key}); // Constructor

  @override
  Widget build(BuildContext context) {
    return Material(
      color: Colors.indigo,
      child: InkWell(
        // Area responsive terhadap sentuhan
        onTap: () {
          // Memunculkan SnackBar ketika diklik
          ScaffoldMessenger.of(context)
            ..hideCurrentSnackBar()
            ..showSnackBar(SnackBar(
                content: Text("Kamu telah menekan tombol ${item.name}!")));
        },
        child: Container(
          // Container untuk menyimpan Icon dan Text
          padding: const EdgeInsets.all(8),
          child: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Icon(
                  item.icon,
                  color: Colors.white,
                  size: 30.0,
                ),
                const Padding(padding: EdgeInsets.all(3)),
                Text(
                  item.name,
                  textAlign: TextAlign.center,
                  style: const TextStyle(color: Colors.white),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```
- Setelah melakukan perubahan pada kedua file, saya melakukan `flutter run` dan melihat apa yang berubah dari tampilan halaman browser saya.


## Tugas 8
### Jelaskan perbedaan antara Navigator.push() dan Navigator.pushReplacement(), disertai dengan contoh mengenai penggunaan kedua metode tersebut yang tepat!
Widget Navigator berfungsi untuk melakukan perpindahan dari halaman satu ke halaman lainnya sesuai dengan instruksi kode yang ditulis. Widget ini menampilkan halaman tersebut sebagai halaman baru yang ditumpuk. Mirip dengan implementasi stack, karena terdapat perintah push(), pop(), dan lain-lain. Saat kita menambahkan perintah push, halaman terbaru akan ada di paling atas stack dan apabila back,yaitu di pop bisa kembali ke halaman sebelumnya. Terdapat dua method yang sekilas mirip namun berbeda yaitu `Navigator.push()` dan `Navigator.pushReplacement()` mereka memiliki kesamaan untuk navigasi halaman, namun proses pada stacknya dibedakan. Perbedaan tersebut adalah,
Navigator.push() | Navigator.pushReplacement()
-----|----|
Pada stack, halaman baru (B) akan dipush ke atas tumpukan halaman saat ini (A), dimana apabila pengguna menekan tombol back, pengguna akan dapat kembali pada halaman (A). | Pada stack halaman baru (B) akan dipush mengantikan halaman saat ini (A). Hal ini dapat berguna apabila pengguna tidak boleh mengakses halaman sebelumnya dan terus lanjut pada navigasi selanjutnya

Penulisan kode keduanya juga mirip, hanya berbeda nama method saja, namun apabila mengimplementasikannya kurang tepat dapat menjadi kesalahan yang cukup fatal. Salah satu penerapan `Navigator.push()` dalam kehidupan sehari-hari pada saat berbelanja online, kita dapat mengecek harga belanja dan tidak jadi beli, kita dapat memencet tombol back ke halaman keranjang yang sebelumnya benar-benar kita akses. Lalu, salah satu contoh penerapan `Navigator.pushReplacement()` adalah setelah kita melakukan pembelian dan melajutkan pembayaran produk, ketika kita memencet tombol kembali, akan diarahkan pada halaman utama platform belanja online, bukan ke keranjang.

### Jelaskan masing-masing layout widget pada Flutter dan konteks penggunaannya masing-masing!
Dalam flutter, terdapat banyak widget untuk mengatur tata letak (layout) dalam aplikasi. Diantaranya adalah
- Container
  - dapat digunakan sebagai tempat dalam mengatur properti seperti padding, margin, warna latar, dan masih banyak lainnya dengan hubungan child.
  ``` dart
  Container(
    margin: EdgeInsets.all(16.0),
    padding: EdgeInsets.symmetric(vertical: 8.0, horizontal: 16.0),
    color: Colors.blue,
    child: Text('Hello World!'),
  )
  ```
- Column dan Row
  - Pada hal ini dapat digunakan pada pengaturan card yang memiliki pengaturan vertikal
  ```dart
  Column(
    children: [
      Text('Item 1'),
      Text('Item 2'),
      Text('Item 3'),
    ],
  )
  ```

- ListView dan GridView
  - ListView dapat digunakan untuk menampilkan daftar dalam list yang ingin tampilkan, pada kode saya, pada saat membuat drawer mengimplementasikan tampilan ini.
  ```dart
  ListView(
    children: [
      ListTile(title: Text('Item 1')),
      ListTile(title: Text('Item 2')),
      ListTile(title: Text('Item 3')),
    ],
  )
  ```
- AppBar
  - Digunakan untuk header halaman utama pada kode
  ```dart
  AppBar(
    title: Text('Mari Belanja'),
    actions: [
      IconButton(
        icon: Icon(Icons.search),
        onPressed: () {
        },
      ),
    ],
  )
  ```

Semua layout memiliki kelebihan dan kekurangannya tersendiri, tergantung dengan implementasi kode masing-masing. Seperti contoh dalam menampilkan halaman yang membutuhkan setting secara vertikal dapat menggunakan listview, tidak perlu column dan row.

### Sebutkan apa saja elemen input pada form yang kamu pakai pada tugas kali ini dan jelaskan mengapa kamu menggunakan elemen input tersebut!

Semua input dalam form yang saya terapkan menggunakan elemen `TextFormField` dimana akan menerima input berupa string untuk nama dan deskripsi produk, sementara untuk harga dan jumlah akan menerima input berupa integer yang juga dihandle apabila field kosong menggunakan validators.

```dart
TextFormField(
  decoration: InputDecoration(
    labelText: 'Nama',
    hintText: 'Masukkan nama Anda',
  ),
)
```
kita dapat mengatur label dan hint pada decoration dari TextFormField dalam form terdapat button yang dapat save product saat dibentuk

### Bagaimana penerapan clean architecture pada aplikasi Flutter?
clean architecture merupakan konsep yang penting dalam mengembangkan sebuah aplikasi dimana memisahkan logika aplikasi menjadi berlapis-lapis terisolasi dan independen satu dengan lainnya. Tujuan dari pemisahan tersebut tidak lain untuk memudahkan pengerjaan bersamaan sebuah aplikasi dan agar tidak tercampur satu dengan lainnya. Pada tugas 8, pemisahan tersebut dipisahkan menjadi tampilan halaman form dan menu pada folder screens. Lalu terdapat widget yang mengatur tombol-tombol pada folder wigdets untuk shop_card dan left drawer navigasi.

Dalam flutter, penerapan tersebut membantu untuk memisahkan bagian menjadi business logic, UI, state management, dan sumber eksternal. Dan kode kita lebih dapat mudah di uji coba

- Lapisan paling luar aadalah lapisan data maupun user interface. Basis data juga terletak dalam kode ini.
- Lapisan presentation merupakan repositary yang menghubungkan data dengan tampilan aplikasi
- Lapisan domain merupakan lapisan paling dalam dimana terdapat logika aplikasi seperti entities dan use cases yang menjadi tempat bergantung seluruh logika lainnya.

### Jelaskan bagaimana cara kamu mengimplementasikan checklist di atas secara step-by-step! (bukan hanya sekadar mengikuti tutorial)

- Langkah pertama yang saya lakukan adalah membuat form untuk menambah produk pada aplikasi saya. Saya membuat maribelanja_form.dart dan membuat state dan membuat widget build sebagai kerangka dasar halaman aplikasi form. Lalu ditambahkan warna untuk form. Saya juga menambahkan _formkey ke dalam atribut key untuk handler dari form state

``` dart
class _ShopFormPageState extends State<ShopFormPage> {
    final _formKey = GlobalKey<FormState>();
}
```

- dan saya menambahkan pada body form key tersebut. Saya menambahkan string kosong untuk nama dan deskripsi produk dan saya setting default harga dan jumlah produk adalah 0

``` dart
class _ShopFormPageState extends State<ShopFormPage> {
    final _formKey = GlobalKey<FormState>();
    String _name = "";
    int _price = 0;
    int _amount = 0;
    String _description = "";
}
```

- Lalu saya menambahkan "TextFromField" yang telah diatur paddingnya untuk meminta output keempat elemen diatas dan saya jadikan child dari column serta setting rata teksnya. Terdapat validator untuk mengecek apakah ada bagian yang tidak diisi oleh pengguna. Sehingga dalam file tersebut berisi elemen-elemen untuk form
``` dart
child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Padding(
                  padding: const EdgeInsets.all(8.0),
                  child: TextFormField(
                    decoration: InputDecoration(
                      hintText: "Nama Produk",
                      labelText: "Nama Produk",
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(5.0),
                      ),
                    ),
                    onChanged: (String? value) {
                      setState(() {
                        _name = value!;
                      });
                    },
                    validator: (String? value) {
                      if (value == null || value.isEmpty) {
                        return "Nama tidak boleh kosong!";
                      }
                      return null;
                  },
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: TextFormField(
                  decoration: InputDecoration(
                    hintText: "Harga",
                    labelText: "Harga",
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(5.0),
                    ),
                  ),
                  onChanged: (String? value) {
                    setState(() {
                      _price = int.parse(value!);
                    });
                  },
                  validator: (String? value) {
                    if (value == null || value.isEmpty) {
                      return "Harga tidak boleh kosong!";
                    }
                    if (int.tryParse(value) == null) {
                      return "Harga harus berupa angka!";
                    }
                    return null;
                  },
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: TextFormField(
                  decoration: InputDecoration(
                    hintText: "Jumlah",
                    labelText: "Jumlah",
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(5.0),
                    ),
                  ),
                  onChanged: (String? value) {
                    setState(() {
                      _amount = int.parse(value!);
                    });
                  },
                  validator: (String? value) {
                    if (value == null || value.isEmpty) {
                      return "Jumlah tidak boleh kosong!";
                    }
                    if (int.tryParse(value) == null) {
                      return "Jumlah harus berupa angka!";
                    }
                    return null;
                  },
                ),
              ),
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: TextFormField(
                  decoration: InputDecoration(
                    hintText: "Deskripsi",
                    labelText: "Deskripsi",
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(5.0),
                    ),
                  ),
                  onChanged: (String? value) {
                    setState(() {
                      _description = value!;
                    });
                  },
                  validator: (String? value) {
                    if (value == null || value.isEmpty) {
                      return "Deskripsi tidak boleh kosong!";
                    }
                    return null;
                  },
                ),
              ),
```

- Saya juga menambah elevated button agar produk baru akan disimpan secara pop up datanya (belum disimpan pada data base). Perlu fungsi `showDialog()` pada bagian `onPressed()` untuk memunculkan informasi dari produk yang coba untuk disimpan. Lalu bagaimana untuk navigasi pop sebagai perintah untuk kembali ke halaman selanjutnya, dan melakukan form reset dengan kode `_formKey.currentState!.reset()`

- Lalu pindah pada menu.dart untuk mengatur navigasi tombol "tambah produl" agar redirect ke halaman form dengan cara menambahkan Navigator.push
```dart
if (item.name == "Tambah Produk") {
  Navigator.push(
    context,
    MaterialPageRoute(
      builder: (context) => const ShopFormPage(),
    ));
          }

```
- Setelah itu saya akan menambahkan shortcut pada navigasi yang telah saya buat pada tugas 7. Pertama saya membuat folder baru bernama `widgets` dengan nama `left_drawer.dart` lalu saya tambahkan kode,
```dart
import 'package:flutter/material.dart';
import 'package:maribelanja/screens/menu.dart';
import 'package:maribelanja/screens/maribelanja_form.dart';


class LeftDrawer extends StatelessWidget {
  const LeftDrawer({super.key});

  @override
  Widget build(BuildContext context) {
    return Drawer(
      child: ListView(
        children: [
          const DrawerHeader(
            decoration: BoxDecoration(
              color: Colors.indigo,
            ),
            child: Column(
              children: [
                Text(
                  'Mari Belanja',
                  textAlign: TextAlign.center,
                  style: TextStyle(
                    fontSize: 30,
                    fontWeight: FontWeight.bold,
                    color: Colors.white,
                  ),
                ),
                Padding(padding: EdgeInsets.all(10)),
                Text("Catat seluruh keperluan belanjamu di sini!",
                    textAlign: TextAlign.center,
                    style: TextStyle(
                      fontSize: 15,
                      fontWeight: FontWeight.normal,
                      color: Colors.white,
                    )
                  ),
              ],
            ),
          ),
          ListTile(
            leading: const Icon(Icons.home_outlined),
            title: const Text('Halaman Utama'),
            // Bagian redirection ke MyHomePage
            onTap: () {
              Navigator.pushReplacement(
                  context,
                  MaterialPageRoute(
                    builder: (context) => MyHomePage(),
                  ));
            },
          ),
          ListTile(
            leading: const Icon(Icons.add_shopping_cart),
            title: const Text('Tambah Produk'),
            // Bagian redirection ke ShopFormPage
            onTap: () {
              Navigator.pushReplacement(
                  context,
                  MaterialPageRoute(
                    builder: (context) => const ShopFormPage(),
                  ));
            },
          ),
        ],
      ),
    );
  }
}
```
- yang akan membuat seperti navigasi shortcut dan pada tugas ini, saya baru setting untuk direct ke halaman form tambah produk apabila buttonnya ditekan akan pindah halaman. Lalu saya juga menambahkan dekorasi seperti warna dan tulisan tambahan untuk mempercantik tampilan. Setelah itu, tambahkan drawer sebagain child dari AppBar pada menu.dart dengan cara import path dan tambahkan kode `drawer: const LeftDrawer(),`

- Setelah itu saya melakukan refactoring file dengan memindahkan kode untuk halaman pada aplikasi yaitu `menu.dart` dan  `maribelanja_form` menjadi satu folder pada screens dan widget pada aplikasi pada folder baru  `widgets` setelah itu lakukan drag `shop_card.dart, menu.dart, maribelanja.dart` pada folder yang telah dijelaskan diatas. Lalu pastikan pada `flutter run` kode dapat berjalan dengan semua checklist tugas terpenuhi dan lancar.


### Referensi
https://aditya-rohman.medium.com/mengembangkan-aplikasi-flutter-dengan-proses-test-driven-development-tdd-dan-mengadopsi-clean-29d29bb0702b

## Tugas 9
- Apakah bisa kita melakukan pengambilan data JSON tanpa membuat model terlebih dahulu? Jika iya, apakah hal tersebut lebih baik daripada membuat model sebelum melakukan pengambilan data JSON?
Hal tersebut dapat dilakukan, yaitu pengambilan data JSOn tanpa pembuatan model. Kita bisa mengambilnya secara langsung namun belum terstruktur rapih. Jika sudah mengonversinya sebagai model, hal tersebut lebih dapat mudah dibaca karena kita dapat mengetahui sebuah data memiliki entitas apa saja. Lalu juga kelemahan dari tidak menggunakan model adalah sulit untuk dikelola dan di modifikasi

- Jelaskan fungsi dari CookieRequest dan jelaskan mengapa instance CookieRequest perlu untuk dibagikan ke semua komponen di aplikasi Flutter. 
Cookie berguna untuk menyimpan data mengenai sesi yang dilakukan oleh pengguna. Dan fungsi dari cookie request adalah mengelola cookies saat autentikasi. Hal ini juga berguna untuk track data kapan terakhir pengguna login

- Jelaskan mekanisme pengambilan data dari JSON hingga dapat ditampilkan pada Flutter.
  - Langkah pertama yang dilakukan adalah mengambil dari HTTP, disini kita gunakan data dari web deploymen django untuk data tersebut. (menggunakan package http)
  - Lalu data yang sudah diambil dlam format JSON akan dikonversi dalam object dalam bahasa dart. Hal ini dijadikan Map seperti konversi model data dari Django ke dart
  - Lalu akan dilakukan pemrosesan dan penuimpanan data yang akan ditampilkan sesuai dengan kode program kita dimana terdapat grid pada kode ini


- Jelaskan mekanisme autentikasi dari input data akun pada Flutter ke Django hingga selesainya proses autentikasi oleh Django dan tampilnya menu pada Flutter.
  Pada tugas ini, beberapa langkan autentikasi pengguna yaitu (apabila berhasil mengambil data dari web pbpo)
  = Pengguna akan ditampilkan halaman login untuk mengisi nama dan password, saya belum melakukan proses registrasi. Dan dilakukan pengiriman data dengan method POST pada backend
  - Pengecekkan akan dilakukan dengan mengecek nama dan password yang sudah pernah masuk ke dalam aplikasi. Setelah proses berlangsung, flutter akan menangkap respon tersebut, pada tugas ini juga terdapat cookie request. Dan pengguna dapat mengakses menu di flutter.

- Sebutkan seluruh widget yang kamu pakai pada tugas ini dan jelaskan fungsinya masing-masing.
  - ListTile = menambah button dari dalam list untuk ditekan
  - Drawer = menggunakan konsep navigation bar
  - Future Builder = melakukan proses apabila terdapat perintah aync ke JSON dimana hal tersebut bisa saja belum terjadi
  - AlertDialog = menampilkan pesan apabila pengguna menekan atau melakukan sesuatu


- Jelaskan bagaimana cara kamu 
mengimplementasikan checklist di atas secara step-by-step! (bukan hanya sekadar mengikuti tutorial).

  - Pada tugas 9 ini, saya belum bisa berhasil login karena deployment django saya bermasalah
  - Saya menambahkan pada proyek saya terdahulu aplikasi autentikasi yang bisa saya gunakan pada tugas ini serta mengatur views.py dan urls.py yang digunakan. Saya juga menambahkannya pada installed apps. Setelah itu pada proyek flutter saya, saya menambahkan login dart yang berisi halaman untuk login pada proyek saya dan merubah rute saat pertama kali akses aplikasi saya harus login, baru bisa mengakses tampilan pada tugas 8
  - Saya juga memindahkan model dari django saya yang dikonversi menjadi bahasa dart agar data saya dapat terstruktur rapih.
  - Saya juga mengambil data dari aplikasi django saya dengan fetch menggunakan package http. Yang harus di tambah dulu serta menginstal beberapa package pada django sebelumnya.
  - Lalu saya membuat list_product.dart yang dapat digunakan untuk melihat produk apa saja yang ada dalam aplikasi saya untuk lihat produk dan mengatur rute jalannya aplikasi ketika menekan button
  - Saya juga membuat cookie dan fungsi untuk logout namun belum bisa menjalankan hal tersebut