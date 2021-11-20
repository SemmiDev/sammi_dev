+++
author = "Sammi"
title = "Rust ~ Ownership, Borrowing & Lifetimes"
date = "2021-11-13"
description = "Membahas Ownership, Borrowing & Lifetimes di Bahasa Pemrograman Rust"
tags = [
    "Rust",
    "Ownership",
    "Rustacean",
]
+++

### Preface

*Ownership, Borrowing & Lifetimes* adalah *The most of distinct and compelling features in Rust* artinya ketiga hal tersebut adalah hal paling mendasar & penting untuk mulai mempelajari Bahasa Pemrograman Rust.

Rust adalah bahasa pemrograman yang fokus pada keamanan (*safety*) dan kecepatan (*speed*) dengan fokus tersebut, Rust dapat memenuhi tujuan *zero-cost abstractions* artinya bahwa dengan bahasa Rust kita dapat membuat aplikasi dengan biaya yang sedikit mungkin, biaya yang dimaksudkan adalah biaya alokasi memori. 

*`Ownership`* adalah contoh utama dari *zero-cost abstractions*. Untuk mempelajari Rust memang harus lebih disiplin dan jika anda mempunyai pengalaman dalam bahasa pemrograman lainnya maka anda perlu belajar kembali tentang kedisiplinan penulisan maupun penyusunan kode program karena dalam hal ini Rust adalah bahasa yang jauh berbeda dengan bahasa lainnya.

### Ownership

Rust hanya memperbolehkan satu *variable* hanya memiliki satu pemilik saja atau apapun yang di ikatkan (*binding*) dengan *variable* tersebut, ini berarti ketika *binding* tersebut keluar dari ruang lingkupnya (*scope*) maka, dengan otomatis Rust akan mengosongkan sumber daya terkait (*will free the bound of resources*), contohnya gini:

```Rust
fn hello() {
    let msg = "Hello Universe";
}
```
Ketika *function* **hello()** di panggil dan disitu terdapat variable *msg* maka string baru akan di buat kedalam *stack*, dan akan di alokasikan ruang memori kedalam **heap** untuk variable *msg*. Tapi ketika function **hello()** sudah selesai melaksanakan tugasnya dan variable *msg* selesai dieksekusi maka Rust akan membersihkan ruang yang tadinya dipakai oleh *variable msg* dan semua yang berhubungan dengan alokasi ruang yang dipakai dalam *function* tersebut. Perlu diketahui bahwa ruang lingkup (*scope*) diawali dengan **{** dan di akhiri **}**.

Rust selalu memastikan bahwa hanya ada satu pemilik untuk satu *binding* (*variable*) untuk semua *resource* yang diberikan. Contoh:

```Rust
let students = vec!["Sammi", "Faren", "Chiko"];
let students2 = students;
println!("The students {}", students[0]);
```

Kita mempunyai satu *variable students* dan *variable* tersebut sudah di berikan ke *variable* lainnya yaitu *students2*, jika kita panggil kembali variable *students* maka kita akan mendapatan *error* seperti ini :

```
use of moved value: `students`
let students2 = students;
    ----- value moved here
println!("The students {}", students[0]);
                           ^^^^ value used here after move
```

Itu artinya, kita tidak boleh menggunakan *variable* yang sudah di pindahkan ke *variable* atau *binding* lainnya.

Ketika kita mempunyai kode seperti ini :

```Rust
let age = 20;
```

Maka rust akan mengalokasikan memori dengan tipe data *integer i32* ke dalam *stack* lalu menyalin alamat memori (*bit pattern*) dan menghubungkan ke variable *age* tadi.

Mari kita bahas lebih dalam lagi tentang *Ownership*. Jika kita mempunyai kode seperti ini:

```Rust
let students = vec!["Sammi", "Faren", "Chiko"];
let mut students2 = students;
```
Pada baris pertama, Rust mengalokasikan memori untuk objek vektor **students** kedalam *stack*, tapi selain itu Rust juga mengalokasikan beberapa memori di *heap* untuk data aktual `(["Sammi", "Faren", "Chiko"])` lalu Rust menyalin alamat memori di *heap* tadi kedalam *pointer internal* yang merupakan bagian dari objek vektor tadi yang ditempatkan di *stack* (ini disebut data *pointer*). Kedua bagian dari vektor (satu di *stack* dan satu lagi di *heap*) panjang (*length*), *capacity* (kapasitas) dan lain-lain adalah sama nilainya.

Ketika kita memindahkan **students** ke **students2**, Sebenarnya Rust menyalin *bitwise* dari objek vektor **students** ke dalam alokasi *stack* untuk **students2** tapi salinan ini tidak membuat kopian dari alokasi di *heap* yang berisi data aktual. Artinya bahwa akan ada dua pointer untuk kedua objek vector yang mengarah ke *heap* yang sama.

Lalu bagaimana jika kedua objek tersebut diakses secara bersamaan? *For example*, jika kita menghapus elemen ke tiga dari objek **students2** :

```Rust
students2.truncate(2);
```

Sedangkan objek **students** masih diakses maka kita akan mendapatkan objek vektor yang tidak lagi valid karena objek **students** tidak akan tahu bahwa salah satu elemen telah dihapuskan. Sekarang objek **students** pada stack tidak lagi sama dengan di *heap*, ini adalah kesalahan yang *risk*-an, karena menyebabkan kesalahan segmentasi atau lebih buruknya lagi memunginkan pengguna yang tidak sah untuk membaca alamat memori yang sudah tidak lagi memiliki akses. Inilah sebabnya mengapa Rust melarang untuk menggunakan objek yang sebelumnya sudah dipindahkan/dipergunakan.

#### *Copy types*

Rust sudah menetapkan bahwa ketika *ownership* sudah di transfer ke *binding* yang lain maka kita tidak dapat lagi menggunakan *binding* yang sebelumnya (asli). Namun kita bisa menggunakan fitur dari *trait* untuk menangani masalah ini yaitu **Copy**, Contohnya gini:

```Rust
let a = 10;
let aa = a;
println!("A is: {}", a);
```

*In this case*, tipe a adalah **i32**, yang mengimplementasikan **trait Copy**. Ketika kita meng-assign a ke aa maka duplikat dari a akan dibuat tapi tidak dipindahkan dan kita tetap dapat mengakses a. Karena **i32** memiliki *pointer* ke data yang lain.

Semua *Primitive Types* adalah implementasi dari **trait Copy** dan kepemilikan nya tidak dipindahkan seperti yang diasumsikan mengikuti `aturan ownership`. Untuk mengilustrasikannya, 2 potongan kode dibawah ini hanya mengkompilasi tipe data **i32** dan **boolean**.

```Rust
fn main() {
    let var_x = 5;
    let _y = double(var_x);
    println!("{}", var_x);
}
fn double(x: i32) -> i32 {
    x * 2
}
```

```Rust
fn main() {
    let var_x = true;
    let _y = change_truth(var_x);
    println!("{}", var_x);
}
fn change_truth(x: bool) -> bool {
    !x
}
```
Jika kita menggunakan tipe data yang tidak mengimplementasikan **trait Copy**, maka pada saat kompilasi kita akan mendapatkan pesan *error* karena kita mencoba untuk memindahkan nilai dari *variable* diatas.

```
error: use of moved value: `var_x`
println!("{}", var_x);
               ^
```

### Borrowing
Rust adalah bahasa dengan *safety code* dimana *object* diatur oleh bahasa pemograman tersebut dari awal hingga akhir. *Developer* tidak perlu lagi melakukan *pointer arithmatic* dan manajemen *memory* seperti yang kita lakukan dalam bahasa C dan C++.

Kode program dibawah ini pada dasarnya untuk menjumlahkan dua buah vector :

```Rust
fn main() {
    fn sum_vec(v: &Vec<i32>) -> i32 {
        return v.iter().fold(0, |a, &b| a + b);
    }
    fn foo(v1: &Vec<i32>, v2: &Vec<i32>) -> i32 {
        let s1 = sum_vec(v1);
        let s2 = sum_vec(v2);
        s1 + s2
    }
    let v1 = vec![1, 2, 3];
    let v2 = vec![4, 5, 6];
    let answer = foo(&v1, &v2);
    println!("{}", answer);
}
```

Alih-alih kita menggunakan **Vec<i32>** sebagai *argument* pada *function*, kita menggunakan *references*: **&Vec<i32>** untuk melakukan pereferensian, dan kita tidak menggunakan **v1** dan **v2** secara langsung melainkan kita menggunakn **&v1** dan **&v2**. Kita gunakan `“references”` yaitu **&T** sebagai cara untuk membawa objek tapi tidak sekaligus dengan pemiliknya. *Binding* yang di-*borrow* tidak akan dihapuskan alokasi memorinya sehingga ketika kita memanggil *binding* tersebut diluar ruang lingkupnya, kita masih bisa menggunakan binding yang aslinya.

Referensi tetap bersifat *“immutable”* seperti *binding*, yang artinya jika terdapat *binding* didalam *function foo()* maka tidak akan bisa diubah atau dibawa keluar dari *“scope”* :

```Rust
fn main() {
  fn foo(v: &Vec<i32>) {
    v.push(5);
  }
  let v = vec![];
  foo(&v);
}
```

Jika program diatas kita *running*, maka akan muncul error seperti ini 

```
error: cannot borrow immutable borrowed content `*v` as mutable
fn foo(v: &Vec<i32>) {
          --------- use `&mut Vec<i32>` here to make mutable
v.push(5);
^
```

karena kita mencoba untuk memasukkan secara paksa sebuah nilai kedalam vector, dan itu tidak akan bisa kita lalukan.

#### References
Jenis kedua dari references yaitu: **&mut T**. Adalah sebuah *“mutable references”* yang mana kita diperbolehkan untuk merubah *resource* dari yang kita *borrow*. For example:

```Rust
fn main() {
  let mut x = 5;
  {
    let y = &mut x;
    *y += 1;
  }
  println!("{}", x);
}
```

Program diatas akan menghasilkan angka 6. Kita buat *mutable reference* untuk **x (&mut x)** lalu kita tambahkan nilai 1 ke y. Karena sebelumnya kita memanggil x dengan *mutable references* maka kita bisa mengolah x tadi, jika tidak seperti itu maka kita tidak akan bisa mengolahnya

Ketika kita ingin mengakes pereferensian dari **&mut** maka kita membutuhkan asterisk `(*)` sebelum y karena y adalah **&mut** dari pe-referensian. Selain itu kita tambahkan pula **{** dan **}** yang menandakan bahwa pengolahan tersebut dalam ruang lingkup tersendiri, coba kita ubah kode diatas menjadi seperti ini:

```Rust
fn main() {
  let mut x = 5;
  let y = &mut x;
  *y += 1;
  println!("{}", x);
}
```
maka kita akan mendapatkan error seperti ini:

```
error: cannot borrow `x` as immutable because it is also borrowed as mutable
let y = &mut x;
             - mutable borrow occurs here
*y += 1;
println!("{}", x);
               ^ immutable borrow occurs here
- mutable borrow ends here
```

#### Borrowing Rules
* Semua borrow harus dalam ruanglingkup (scope) yang tidak lebih dari binding aslinya.
* Boleh memakai satu atau dua dari kedua jenis borrow, tapi tidak memakai kedua jenis borrow secara bersamaan:
    - satu atau lebih references **(&T)** ke resource.
    - tepat satu mutable reference **(&mut T)**.


Dengan *references*, kemungkinan kita akan memiliki banyak *references* yang akan kita buat. Namun kita hanya dapat memiliki **&mut** dalam satu waktu, dengan demikian kita tidak akan bisa memiliki data *race* jika kita menggunakan **&mut** secara bersamaan. Inilah mengapa Rust mencegah data *races* pada saat kompilasi dan kita akan mendapatkan *error* ketika kita melanggar aturan yang ada.

Okehh, *let’s consider our example again*.
```Rust
fn main() {
  let mut x = 5;
  let y = &mut x;
  *y += 1;
  println!("{}", x);
}
```

```
error: cannot borrow `x` as immutable because it is also borrowed as mutable
let y = &mut x;
             - mutable borrow occurs here
*y += 1;
println!("{}", x);
               ^ immutable borrow occurs here
- mutable borrow ends here
```

Error tersebut karena kita telah melanggar aturan: **&mut T** yang di pointing ke x, dan kita tidak diperbolehkan untuk membuat **&T**


```Rust
fn main() {
  let mut x = 5;
  let y = &mut x;    // -+ &mut borrow `x` dimulai disini.
  *y += 1;           //  |
  println!("{}", x); // -+ - mencoba mem-borrow kembali `x` disini.
}  
```

Scope nya akan konflik karena kita tidak bisa membuat **&x** selama y masih dalam scope yang sama dengan *binding* yang aslinya. Dan ketika kita membuat *scope* untuk *borrowing* diatas kita tandai dengan **{** dan **}**:

```Rust
let mut x = 5;
{
    let y = &mut x; // -+ &mut borrow dimulai disini.
    *y += 1;        //  |
}                   // -+ ... borrow berakhir disini.
println!("{}", x);  // <- `x` di-borrow kembali disini.
```

Kode diatas tidak akan mendapatkan masalah karena kita mem-borrow dalam scope yang berbeda.

### Lifetimes
Ketika kita sudah selesai berinteraksi dengan object maka proses untuk dis-alokasi memory akan dilakukan secara otomatis.  Sehingga kita tidak perlu secara manual untuk membersihkan memory yang sudah digunakan.

Pada *unmanaged* code seperti C++ cukup sulit untuk membuat sebuah program yang benar dan terbebas dari *bug*, sehingga seringkali menimbulkan celah yang bisa *dicompromise*. Ancaman yang paling besar adalah *buffer overflow attack* yaitu ketika seseorang bisa mengakses informasi melebihi dari informasi yang disediakan pada alokasi memory space untuk program, Artinya seseorang bisa melakukan modifikasi di level bawah pada lokasi memori yang berada diluar jangkauan oleh program.

Oke Let’s get started with Lifetimes.

Melakukan references ke *resources* yang dimiliki *binding* lain dapat menjadi rumit. Misalnya, bayangkan jika terdapat operasi :

* Menangani beberapa jenis resource.
* Meminjamkan references ke resource.
* Memutuskan hubungan ke resource dan mendisalokasikan memori namun masih memiliki references.
* Memutuskan untuk menggunakan resource.

Dan boom! *Your reference is pointing to an invalid resource*. Ini yang disebut `“dangling pointer”` atau `“use after free”`. Contoh sederhana untuk situasi diatas adalah :

```Rust
let r;              // Kita buat binding : `r`.
{
    let i = 1;      // Kita buat scoped value untuk : `i`.
    r = &i;         // Assing reference dari `i` ke `r`.
}                   // `i` keluar dari scope.
println!("{}", r);  // tapi `r` masih me-refer ke `i`.
```

Untuk menghindarai kesalahan diatas, maka kita perlu memastikan bahwa pereferensian tetap dalam *scope* atau ruang lingkupnya.

Ketika kita membuat suatu function yang mengambil argumen dari references maka situasinya akan menjadi lebih kompleks. Perhatikan contoh berikut :


```Rust
fn skip_prefix(line: &str, prefix: &str) -> &str {
  // ...
}
let line = "lang:en=Hello World!";
let lang = "en";
let v;
{
  let p = format!("lang:{}=", lang);  // -+ `p` didalam scope
  v = skip_prefix(line, p.as_str());  //  |
}                                       // -+ `p` keluar dari scope.
println!("{}", v);
```

Di sini kita memiliki fungsi `skip_prefix` yang mengambil dua referensi **&str** sebagai parameter dan mengembalikan referensi tunggal **&str**. Kita panggil dengan cara mem-passing ke dalam *references* pada p: dua variable dengan lifetimes yang berbeda.

[Referensi](https://doc.rust-lang.org/stable/book)
