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
  
Fungsi splice juga bisa digunakan untuk menghapus elemen dari suatu array pada index tertentu. 
Misalnya: ```javascript vm.books.splice(2,1)```
Perintah tersebut akan menghapus elemen array pada index ke-dua sebanyak satu elemen.

Perintah splice pun bisa digunakan untuk mengubah elemen dari suatu array pada index tertentu. 
Misalnya: ```javascript vm.books.splice(1,1,'Mastering Hacking')```
Perintah tersebut akan menghapus elemen array index ke-satu sekaligus menambahkan elemen baru pada
index ke-satu juga. (ingat: index array dimulai dari 0)

* Filter
fungsi filter digunakan untuk mem-filter elemen dari array berdasarkan kriteria tertentu. Misalnya
mengambil judul buku (dari books) yang diawali dengan huruf M.

```javascript
vm.books = vm.books.filter((book)=>{
return book[0]==='M'
})
```
Implementasi pada Vue biasanya dengan menggunakan properti computed. Properti ini berupa fungsi-fungsi
yang nilainya senantiasa dipantau atau observe sesuai dengan perubahan data.

```javascript
<div id="app">
<ul>
<li v-for="(book, index) of booksPrefixM" :key="index">
{{ book }}
</li>
</ul>
</div>
<script type="text/javascript">
var vm = new Vue({
el: '#app',
data: {
books : [
'C++ High Performance',
'Mastering Linux Security and Hardening', 'Python Programming
Blueprints',
'Mastering PostgreSQL 10'
]
},
computed: {
booksPrefixM() {
return this.books.filter((book)=>{
return book[0]==='M'
})
}
}
})
</script>
```

jika pada contoh sebelumnya untuk mengubah data pada suatu elemen array menggunakan fungsi splice
maka sebenarnya kita bisa juga melakukannya dengan fungsi set Vue.
```javascript
Vue.set(data_array, index_yang_akan_diganti, nilai_baru)
```
## Perubahan Data Pada Objek
Seperti halnya array, objek pun juga perlu fungsi khusus untuk melakukan perubahan data.
Untuk melakukan penambahan data properti pada objek, kita bisa gunakan fungsi set bawaan Vue.

```javascript
Vue.set(object, key, value)
atau
vm.$set(object, key, value)
```

```javascript
Vue.set(vm.books2, 'title', 'C++ High Performancee')
```

Namun, jika properti yang ingin kita tambahkan lebih dari satu maka kita bisa gunakan cara berikut.
```javascript
vm.books2 = Object.assign({}, vm.books2, {
  id: 66,
  price: 15000
})
```
## Input Binding
```javascript
<div id="app">
<form>
<input type="text" name="title" :value="title" @input="title =
$event.target.value" />
{{ title }}
</form>
</div>
<script>
var vm = new Vue({
  el: '#app',
  data: {
    title: "Mastering "
},
})
</script>
```
Atribut value di-bind dan event oninput di-listen. Kode :value="title" menunjukkan bahwa nilai dari field
input ini di-bind dengan variabel title, sedangkan kode @input="title = $event.target.value"
artinya ketika field diinput maka nilai variabel title akan diubah sesuai isian user.

> Catatan: metode ini akan dipakai ketika kita bermain dengan komponen.

untuk mengatasi "kerumitan" ini, Vue memperkenalkan directive v-model yang bertugas
melakukan two way data binding tersebut. Berikut ini contohnya.

```javascript
<form>
<input type="text" name="title" v-model="title" placeholder="masukkan
judul">
{{ title }}
<br><br>
<textarea name="description" v-model="description"
placeholder="masukkan deskripsi"></textarea>
{{ description }}
</form>
var vm = new Vue({
            el: '#app',
            data: {
                title: "",
                description: ""
            }
        })
```

Directive v-model juga memiliki modifier, diantaranya adalah number dan trim.
Modifier number digunakan untuk memastikan input data dari user bertipe numeric. Sedangkan modifier
trim digunakan untuk menghapus spasi putih diawal atau akhir dari string.
```html
<input type="number" name="price" v-model.number="price">
<br><br>
<input type="text" name="title" v-model.trim="title"
placeholder="masukkan judul"> "{{ title }}"
```
### Array menggunakan v-model
Input data bertipe array terjadi pada field input dengan kemungkinan pilihan lebih dari satu seperti checkbox
multiple dan select multiple.
```html
<select v-model="categories" multiple>
<option value="01">Graphics Programming</option>
<option value="02">Mobile Application Development</option>
<option value="03">Virtual and Augmented Reality</option>
</select>
<span>Selected: {{ categories }}</span>
```

```javascript
var vm = new Vue({
el: '#app',
data: {
hobbies: [],
categories: []
}
})
```
### generate menggunakan directive v-for

```javascript
var vm = new Vue({
            el: '#app',
            data: {
                categories: [],
                options: [{
                        text: 'Graphic Programming',
                        value: '01'
                    },
                    {
                        text: 'Mobile Application Development',
                        value: '02'
                    },
                    {
                        text: 'Virtual and Augmented Reality',
                        value: '03'
                    }
                ]
            }
        })
        ```
       
        ```javascript
        <select name="categories" v-model="categories" multiple>
          <option v-for="option in options" :value="option.value">
            {{ option.text }}
          </option>
        </select>
        <span>Selected: {{ categories }}</span>
        ```
        

