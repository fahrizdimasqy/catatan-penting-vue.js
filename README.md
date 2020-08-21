# catatan-penting-vue.js

## Siklus Objek Vue

### Create

1. beforeCreate yaitu hook sesaat setelah objek Vue dan komponennya diinisialisasi. Properti data belum
dapat diakses atau digunakan pada hook ini.
2. created yaitu hook ketika objek Vue telah selesai diciptakan. Pada hook ini, sifat reactivity pada properti
data juga sudah didefinisikan sehingga kita sudah diizinkan untuk mengakses dan memanipulasi data.
Properti computed yang digunakan untuk memonitor perubahan data juga sudah berjalan. Jika aplikasi
membutuhkan request data dari server maka hook ini adalah hook yang tepat untuk melakukannya.
Berikut ini contoh kode untuk menggunakan kedua hook ini yaitu dengan method beforeCreate dan created.

### mount
3. beforeMount yaitu hook ketika template dicompile.
4. mounted yaitu hook ketika elemen (properti el) telah diinisialisasi, data telah dimuat dan view telah
dirender.

### update
5. beforeUpdate yaitu hook yang terjadi setelah mounted dan hanya terjadi jika ada perubahan data yang
mengakibatkan render ulang. Tepatnya, hook ini terjadi sebelum view dirender ulang.
6. updated yaitu hook yang terjadi setelah beforeUpdate yaitu setelah view dirender ulang.

### destroy
7. beforeDestroy yaitu hook yang terjadi sebelum component dihapus.
8. destroyed yaitu hook yang terjadi setelah objek Vue dihapus.

```javascript
var vm = new Vue({
el: '#app',
data: {
message: 'Hello world!',
},
beforeCreate () {
console.log('before create: '+
'message = ' + this.message)
},
created () {
console.log('created: '+
'message = ' + this.message)
},
});
```
Pada gambar di atas terlihat bahwa hook created bisa mengakses variabel message sebaliknya beforeCreate
tidak bisa.
Apa yang terjadi pada properti data dapat kita lihat juga dengan cara tambahkan kode hook created menjadi
sebagai berikut.


### Properti Method
Properti methods digunakan untuk fungsi yang bisa dipanggil melalui suatu event

### Properti Computed
computed digunakan sebagai variabel bayangan yang nilainya bergantung pada variabel data,

### Properti Filters
filters digunakan untuk memanipulasi tampilan dari suatu teks.

## Mengenal Directive

### v-html
Merupakan directive yang digunakan untuk menampilkan data berupa kode HTML
```javascript
var vm = new Vue({
el: '#app',
data: {
message: "<span style='color:red'>Hello World!</a>",
},
})
```

```html
<p v-html="message"></p>
```

### v-once
Merupakan directive yang digunakan agar nilai variabel pada template tidak bisa diubah-ubah lagi.

```html
<p v-once>{{ message }}</p>
```

### v-text
Merupakan directive yang digunakan untuk menampilkan string biasa, fungsinya sama dengan mustache
atau double kurung kurawal.

```html
<p v-text="message"></p>
<!-- sama dengan -->
<p>{{ message }}</p>
```

### v-show
Merupakan directive yang digunakan untuk menampilkan atau menyembunyikan suatu elemen DOM.
Directive ini membutuhkan variabel bertipe boolean.

```html
  <p v-show="displayMessage">{{ message }}</p>
```
Ketika variabel displayMessage bernilai true maka teks message akan terlihat di browser, sebaliknya jika
jika variabel displayMessage bernilai false maka teks message tidak akan terlihat di browser. Proses on/off
pada directive ini menggunakan properti display pada CSS.

### V-if
```javascript
<div id="app">
  <div v-if="nilai === 'A'">
  Sempurna
  </div>
  <div v-else-if="nilai === 'B'">
    Bagus
  </div>
  <div v-else-if="nilai === 'C'">
    Cukup
  </div>
  <div v-else>
    Kurang
  </div>
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      nilai: "B",
    },
  })
</script>
````

### v-on
Merupakan directive yang berperan sebagai sebuah event listener pada elemen HTML/komponen Vue.
Directive ini bertugas memantau aktifitas (aksi) yang dilakukan terhadap suatu elemen HTML/komponen Vue.

```javascript
<button v-on:click="info('halo')">
  Informasi
</button>
```

> **Catatan: info() adalah method yang harus kita deklarasikan dalam Vue, lihat pembahasan berikutnya.** 
> **Catatan: penulisan directive v-on: dapat disingkat menjadi @, contoh: **

```html
<button @click="info('halo')">Info</button>
```
### v-bind
Directive ini berfungsi untuk mem-binding atribut HTML atau komponen agar nilainya terupdate secara
reactive sesuai dengan datanya, kebalikan dari v-on.

```javascript
<div id="app">
  <img v-bind:src="imageSrc">
</div>
<script>
  var vm = new Vue({
  el: '#app',
    data: {
    imageSrc: 'logo-vue.png',
  }
 })
  setTimeout(()=>{
  vm.imageSrc = 'flowers.jpg'
 },3000);
</script>
```

Pada contoh di atas, attribut src mem-binding variabel imageSrc, sehingga nilai dari atribut src tersebut
mengikuti nilai dari variabel imageSrc. Demikian juga ketika nilai variabel imageSrc diganti secara runtime
maka image yang ditampilkanpun juga akan berubah karena variabel imageSrc diubah.

> **Pada contoh di atas, attribut src mem-binding variabel imageSrc, sehingga nilai dari atribut src tersebut
mengikuti nilai dari variabel imageSrc. Demikian juga ketika nilai variabel imageSrc diganti secara runtime
maka image yang ditampilkanpun juga akan berubah karena variabel imageSrc diubah.**

```html
<a :href="url"> Website VueJS </a>
```

## Dynamic Argument

```html
<a v-bind:[nama_atribut]="url"> ... </a>
```
Nah pada contoh di atas, variabel nama_atribut akan secara dinamis dievaluasi sebagai JS expression, dan
hasil dari evaluasi itu akan digunakan sebagai nilai akhir dari argumen direktif tersebut. Misalnya pada Vue
kita set data nama_atribut bernilai "href", maka binding directive ini akan sama dengan kode v-bind:href.

## Fungsi fungsi built in javascript
* push
Fungsi push digunakan untuk menambahkan data elemen baru pada suatu array pada posisi index terakhir.
contoh: vm.books.push('Mastering PHP 7')

* pop
fungsi pop untuk menghapus elemen terakhir dari suatu array.
contoh: vm.books.pop()

* unshift
Fungsi unshift digunakan untuk menambahkan data elemen baru pada suatu array pada posisi index pertama
(0).
contoh: vm.books.unshift('Mastering PHP 7')

* shift
fungsi shift untuk menghapus elemen pertama dari suatu array.
contoh: vm.books.shift()

* sort 
Fungsi sort digunakan untuk mengurutkan data elemen pada suatu array secara ascending
contoh: vm.books.sort()

* reverse 
Fungsi reverse melakukan sebaliknya
contoh: vm.books.reverse()

* splice
Fungsi splice ini multi fungsi, bisa digunakan untuk menambahkan data elemen baru pada suatu array.
contoh: vm.books.splice(2,0,'Mastering PHP 7')
Perintah tersebut akan menambahkan data elemen baru yaitu 'Mastering PHP 7' pada index ke dua dari
array.
  
  - parameter pertama menujukkan posisi index dari data yang akan ditambahkan.
  - adapun parameter kedua menunjukkan jumlah elemen pada array yang akan dihapus.
