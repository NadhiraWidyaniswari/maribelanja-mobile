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


