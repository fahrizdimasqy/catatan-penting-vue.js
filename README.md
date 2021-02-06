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
```javascript
var vm = new Vue({
  el: '#app',
  data: {
  counter: 0
 },
 methods: {
  increment () {
  this.counter++
  }
 }
})
```
```javascript
<div id="app">
 <h1>{{ counter }}</h1>
 <button onclick="vm.increment()"> + </button>
</div>
```
### Properti Computed
computed digunakan sebagai variabel bayangan yang nilainya bergantung pada variabel data,
```javascript
var vm = new Vue({
  el: '#app',
  data: {
  firstName: 'Hafid',
  lastName: 'Mukhlasin'
 },
 computed: {
  fullName: function () {
  return this.firstName + ' ' + this.lastName
  }
 }
})
```
```javascript
<div id="app">
{{ fullName }}
</div>
```
### Properti Filters
filters digunakan untuk memanipulasi tampilan dari suatu teks.
Disamping methods dan computed, Objek Vue juga memiliki properti filters yang dapat berisi fungsi untuk
digunakan memanipulasi tampilan atau format teks pada template. Filters ditulis dengan menggunakan
simbol | atau “pipe”.
Contoh penggunaan filters adalah untuk mengubah bentuk teks menjadi huruf kapital.
```javascript
<h1>{{ message | upper }}</h1>
```
```javascript
var vm = new Vue({
  el: '#app',
  data: {
  message: 'Hello world!',
 },
 filters: {
  upper (text) {
  return text.toUpperCase()
  }
 }
})
```

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
ita bisa mencegah redirect dengan menggunakan perintah
```
event.preventDefault(),
```
caranya,
tambahkan parameter $event pada pemanggilan fungsi link di template.

```html
<a href="http://vuejs.org" v-on:click="link($event)">
Vuejs.org
</a>
```

```javascript
...
methods: {
  link (event) {
  alert('Go to link')
  event.preventDefault()
  }
}

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

## List
Menampilkan Data Array 
```javascript
books : [
 'C++ High Performance',
 'Mastering Linux Security and Hardening', 'Python Programming
Blueprints',
 'Mastering PostgreSQL 10'
]
var vm = new Vue({
 el: '#app',
 data: {
  books : [
      'C++ High Performance',
      'Mastering Linux Security and Hardening',
      'Python Programming Blueprints',
      'Mastering PostgreSQL 10'
    ]
  }
})
```
```javascript
<div id="app">
 <ul>
  <li v-for="book in books">
  {{ book }}
  </li>
 </ul>
</div>
```
Vue mempunyai directive v-for yang berfungsi untuk melakukan perulangan sebanyak elemen data yang
ada pada variabel books. Sedangkan book (tanpa s) merupakan elemen (item satuan) dari array books
yang bisa langsung ditampilkan tentunya dengan menggunakan mustache {{ }}
Mari kita lihat hasilnya.

v-for Menggunakan Tag Template
```javascript
<div id="app">
  <ul>
    <template v-for="book in books">
      <li> {{ book }} </li>
    </template>
 </ul>
</div>
```
v-for Menggunakan Index
Index dari suatu array yang kita tampilkan melalui v-for bisa kita gunakan dengan menambahkan argumen
kedua sebagai berikut
```javascript
<li v-for="(book, index) in books">
 {{ index+1 }}. {{ book }}
</li>
```
> Selain in, kita bisa juga menggunakan delimiter of pada directive v-for.
Menampilkan Data Objek 
```javascript
book: {
 id: 99,
 title: 'C++ High Performance',
 description: 'Write code that scales across CPU registers, multi-core,
and machine clusters',
 authors: 'Viktor Sehr, Björn Andrist',
 publish_year: 2018,
 price: 100000,
}

<li v-for="value of book">
 {{ value }}
</li>

//Kita juga bisa menambahkan argumen kedua untuk key, seperti berikut.
<li v-for="(value, key) of book">
{{ key }} : {{ value }}
</li>

//Adapun argumen ketiga yang bisa kita tambahkan akan menjadi index dari objek tersebut.
<li v-for="(value, key, index) of book">
{{ index+1 }}. {{ key }} : {{ value }}
</li>
```
### Menampilkan Data Collection
```javascript
books : [
 {
 id: 99,
 title: 'C++ High Performance',
 description: 'Write code that scales across CPU registers, multicore, and machine clusters',
 authors: 'Viktor Sehr, Björn Andrist',
 publish_year: 2018,
 price: 100000,
 image: 'c++-high-performance.png'
 },
 {
 id: 100,
 title: 'Mastering Linux Security and Hardening',
 description: 'A comprehensive guide to mastering the art of
preventing your Linux system from getting compromised',
 authors: 'Donald A. Tevault',
 publish_year: 2018,
 price: 125000,
 image: 'mastering-linux-security-and-hardening.png'
 },
 {
 id: 101,
 title: 'Mastering PostgreSQL 10',
 description: 'Master the capabilities of PostgreSQL 10 to
efficiently manage and maintain your database',
 authors: 'Hans-Jürgen Schönig',
 publish_year: 2016,
 price: 90000,
 image: 'mastering-postgresql-10.png'
 },
 {
 id: 102,
 title: 'Python Programming Blueprints',
 description: 'How to build useful, real-world applications in the
Python programming language',
 authors: 'Daniel Furtado, Marcus Pennington',
 publish_year: 2017,
 price: 75000,
 image: 'python-programming-blueprints.png'
 }
Pada kasus nyata, seringkali kita dapati data tidak dalam bentuk array sederhana ataupun objek, melainkan
dalam bentuk yang lebih kompleks semisal array dari objek atau dalam format JSON (Javascript Object
Notation).
Perhatikan contoh data list berikut.
## Fungsi fungsi built in javascript
* push
Fungsi push digunakan untuk menambahkan data elemen baru pada suatu array pada posisi index terakhir.
contoh: vm.books.push('Mastering PHP 7')
```
Misalnya data tersebut ingin kita tampilkan dalam bentuk HTML tabel, maka melalui pendekatan yang sama
dengan sebelumnya kita bisa menyusun templatenya sebagai beriku
```javascript
<div id="app">
 <table border=1>
 <tr v-for="book of books">
 <td>
 <img width=100 :src="'images/books/' + book.image" />
 </td>
 </td>
 <td>
 title: {{ book.title }} <br>
 description: {{ book.description }} <br>
 authors: {{ book.authors }} <br>
 price: {{ book.price }}
 </td>
 </tr>
 </table>
</div>
```
Mari kita bedah satu persatu kode di atas.
Pertama, directive v-for kita letakkan di elemen tr pada table karena elemen itulah yang akan di-looping.
Pilihan lain kita bisa juga menggunakan elemen <template>
  
Kedua, pada kolom pertama tabel ini kita akan tampilkan cover buku menggunakan kode <img width=100
:src="'images/books/' + book.image" />. Supaya nilai dari atribut src dari elemen image menjadi
dinamis sesuai dengan data books, maka (sebagaimana yang telah kita bahas pada bab terdahulu) kita perlu
perlu menambahkan directive v-bind pada atribut tersebut. v-bind:src atau disingkat menjadi :src.
Oleh karena variabel book yang dihasilkan dari perulangan variabel books berbentuk objek, maka kita bisa
panggil setiap item didalamnya dengan menggunakan titik diikuti nama keynya.

karena lokasi cover buku pada tutorial ini ada dalam direktori images/vue/books maka kita bisa
tambahkan definisi direktori tersebut pada atribut src.
Ketiga, sebagaimana poin kedua, pada kolom kedua dari tabel, bisa kita tampilkan detail bukunya.
```javascript
<td>
 title: {{ book.title }} <br>
 description: {{ book.description }} <br>
 authors: {{ book.authors }} <br>
 price: {{ book.price }}
</td>
```
```javascipt
<td>
 <template v-for="(value, key) of book">
 {{ key }} : {{ value }} <br>
 </template>
</td>
```
Atribut Key 
Terkait dengan metode menampilkan list, Vue menyarankan agar sebisa mungkin menggunakan atribut key
pada tag HTML yang ikut dalam perulangan v-for. Key tersebut berperan sebagai penanda unik.
```javascript
<li v-for="(book, index) of books" v-bind:key="index">
{{ index+1 }}. {{ book }}
</li>
```
Boleh juga kita gunakan directive v-for lagi sebab variabel book berbentuk objek
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
lorem 
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
   ```html
          <select name="categories" v-model="categories" multiple>
          <option v-for="option in options" :value="option.value">
            {{ option.text }}
          </option>
        </select>
        <span>Selected: {{ categories }}</span>
  ```
  
  ## Handling Submit Form & Validation
Sebagaimana kita ketahui bahwa form memiliki event submit yang umumnya digunakan untuk mengirimkan
data (yang berasal dari field input user) ke server atau ke bagian lain dari aplikasi.
```html
<form @submit="submitForm($event)" action="http://example.com/add-
product" method="post">
```

### Validasi Data
Proses validasi adalah proses memastikan setiap isian yang diinput oleh user melalui form tersebut sesuai
dengan persyaratan minimal yang kita tentukan.Misalnya: title harus diisi dan minimal 3 karakter, description
boleh tidak diisi namun kalau diisi maka tidak boleh lebih dari 500 karakter, price tidak boleh minus, dsb. Jika
ditemukan terdapat isian yang tidak memenuhi syarat minimal maka akan dimunculkan pesan peringatan
berupa alert serta dicatat sebagai error (counter). Pada bagian akhir kemudian dicek secara total apakah
terdapat error (error lebih dari 0) ataukan tidak (error = 0), jika tidak ada error maka ditampilkan pesan terima
kasih dan kode untuk mengirim data tersebut ke server.

```javascript
submitForm(event){
  let error = 0
  if(this.title.length < 3){
  error++
  alert('Title minimal 3 karakter!')
}
else if(this.description.length > 500){
  error++
  alert('Description maximal 500 karakter!')
}
else if(this.authors.length < 3){
  error++
  alert('Authors minimal 3 karakter!')
}
else if(this.price < 0){
  error++
  alert('Price tidak boleh minus!')
}
else if(this.categories.length === 0){
  error++ 
  alert('Pilih minimal 1 category!')
}
if( error === 0 ){
  alert('Terima kasih telah mengisi data dengan benar!')
  // kirim data ke server
}
event.preventDefault()
}

```
Kita bisa juga menambahkan method setFocus pada input yang belum memenuhi syarat. Sehingga akan
memudahkan dari sisi user. Caranya dengan menambahkan directive ref sebagai penanda unik pada field
input.

```html
<input type="text" name="title" ref="title" v-model="title">
```

```javascript
var vm = new Vue({
 el: '#app',
 data: {
 title: 'Google Glass with VueJS',
 description: 'Control Google Glass with VueJS',
 authors: 'Hafid Mukhlasin',
 price: 75000,
 categories: [],
 options: [
 { text: 'Graphics Programming', value: '01' },
 { text: 'Mobile Application Development', value: '02' },
 { text: 'Virtual and Augmented Reality', value: '03' }
 ],
 errors: []
 },
 methods: {
 submitForm(event){
 this.errors = []
 if(this.title.length < 3){
 this.errors.push('Title minimal 3 karakter!')
 this.$refs.title.select()
 }
 if(this.description.length > 500){
 this.errors.push('Description maximal 500 karakter!')
 this.$refs.description.select()
 }
 if(this.authors.length < 3){
 this.errors.push('Authors minimal 3 karakter!')
 this.$refs.authors.select()
 }
 if(this.price < 0){
 this.errors.push('Price tidak boleh minus!')
 this.$refs.price.select()
 }
 if(this.categories.length === 0){
 this.errors.push('Pilih minimal 1 category!')
 this.$refs.categories.focus()
 }
 if( this.errors.length === 0 ){
 alert('Terima kasih telah mengisi data dengan benar!')
 // kirim data ke server
 }
 }
 }
})
```
```javascript
<form ref="formBook" @submit.prevent="submitForm($event)"
action="http://example.com/add-product" method="post">

 <p v-if="errors.length">
 <b>Please correct the following error(s):</b>
 <ul>
 <li v-for="error in errors">{{ error }}</li>
 </ul>
 </p>
 <label>Title:</label>
 <input name="title" ref="title" type="text" v-model="title">
 <label>Description:</label>
 <textarea name="description" ref="description" v-model="description">
</textarea>

 <label>Authors:</label>
 <input name="authors" ref="authors" type="text" v-model="authors">

 <label>Price:</label>
 <input name="price" ref="price" type="number" v-model.number="price">

 <label>Categories:</label>
 <select name="categories" ref="categories" v-model="categories"
multiple>
 <option v-for="option in options" :value="option.value">
 {{ option.text }}
 </option>
 </select>

 <label></label>
 <input type="submit" value="Submit">
</form>
```

>Bisa juga dengan kode ini this.$refs.title.select().

>Catatan: tambahkan juga directive ref pada form supaya lebih mudah nantinya menangani elemen
form tersebut.

### Prepare Data Submit
Setelah validasi form dilakukan, langkah berikutnya adalah mempersiapkan data sebelum dikirimkan ke
server. Kita bisa menggunakan object FormData
(https://developer.mozilla.org/en-US/docs/Web/API/FormData/FormData)  untuk mem-packing data hasil isian form menjadi sebuah object.

```javascript
let formData = new FormData();
// tambahkan satu persatu field
formData.append('username', 'Chris');
```

Berikut ini implementasi pada kasus kita.
```javascript
if( this.errors.length === 0 ){
    alert('Terima kasih telah mengisi data dengan benar!')
    // persiapkan data
    let formData = new FormData()
    formData.append('title', this.title)
    formData.append('description', this.description)
    formData.append('authors', this.authors)
    formData.append('price', this.price)
    formData.append('categories', this.categories)
    // kirim data ke server
}
```

Di samping cara di atas yaitu manual satu persatu dalam memasukkan data field, FormData juga
menyediakan cara cepat untuk memasukkan semua data field yang dimiliki form ke dalam objek formData.
Berikut yang kita dapat dari dokumentasi FormData.

```javascript
let myForm = document.getElementById('myForm');
formData = new FormData(myForm);
```
Dengan sedikit modifikasi maka implementasi pada kasus kita menjadi sebagai berikut

```javascript
// persiapkan data
let formBook = this.$refs.formBook
formData = new FormData(formBook);
// kirim data ke server
```
selesai

> Catatan: untuk bisa menggunakan cara singkat ini syaratnya satu yaitu field pada form harus ada atribut name-nya.

### Send Data To Server
Setelah data di-bundle dalam satu objek formData, maka data siap untuk dikirim ke server. Pada bagian ini,
kita akan mensimulasikan pengiriman data form ke server menggunakan PHP native.

> Catatan: 80 adalah nomer port dari webserver, kita bebas mengubahnya dengan nomer lain yang sedang tidak digunakan.
```javascript
if( this.errors.length === 0 ){
 //alert('Terima kasih telah mengisi data dengan benar!')
 // persiapkan data
 let formBook = this.$refs.formBook
 formData = new FormData(formBook);
 // kirim data ke server
 let xhttp = new XMLHttpRequest() // create objek XMLHttp
 // definisikan fungsi ketika terjadi perubahan state
 xhttp.onreadystatechange = function() {
 // state ini menunjukkan data terkirim dan diterima server dengan
baik
 if (this.readyState == 4 && this.status == 200) {
 // respon text dari server
 console.log(this.responseText)
 }
 }
 // sesuaikan dengan lokasi file index.php di lokasi komputer kamu
 xhttp.open("POST", "http://localhost/index.php", true)
 // bisa juga langsung nama filenya jika berada dalam satu folder yang
sama
 // xhttp.open("POST", "index.php", true)
 // kirim objek formData
 xhttp.send(formData)
}
```
### Handling File Upload
Pada sisi client, penanganan field bertipe file pada form pada dasarnya hampir sama saja dengan field
bertipe lain.
```javascript
<label>Cover:</label>
<input name="cover" ref="cover" type="file">
```
```javascript
// get file yang dibrowse user
let cover = this.$refs.cover.files[0]
// tambahkan ke object formData
formData.append("cover", cover);
```

### Kirim Data ke Component
Vue mempunyai mekanisme untuk mengirimkan atau mengeset suatu data pada component yaitu dengan
menggunakan properti props
Untuk mendefinisikan props, pada component caranya cukup dengan menambahkan properti props.
```javascript
props: [ 'nama_props1', 'nama_props2' ],
```
Kemudian pada template dari component tersebut kita bisa akses props tersebut layaknya properti data
```javascript
template : `<div> {{ nama_props1 }} </div>`
```
Sebagai contoh, kita mempunyai component book, dengan tiga props yaitu title, description dan image
```javascript
var BookComponent = {
  data () {
    return {
      classCard: 'card'
    }
   },
 props: [ 'title', 'description', 'image' ],
 template : `
    <div :class="classCard">
    <h3>{{ title }}</h3>
    <img :src="'images/books/'+image" style="width:100%">
    <p v-html="description"></p>
    </div>
  `
}
```
Seperti biasa, kita perlu daftarkan component book pada objek utama Vue.
```javascript
new Vue({
 el: '#app',
 components: {
 'book': BookComponent,
 }
})
```
Pada bagian template utama, kita bisa deklarasikan component book dan sekaligus mendefinisikan nilai dari
masing-masing props, sebagai berikut.
```javascript
<div id="app">
 <book
 title="C++ High Performance"
 description="Write code that scales across CPU registers, multicore, and machine clusters"
 image = "c++-high-performance.png"
 >
 </book>
</div>
```
### Directive Pada Component
Component pada Vue bisa diibaratkan sebagai elemen HTML di mana ada atribut berupa directive yang bisa
diaplikasikan padanya. Sebagai contoh, kita akan menampilkan data dalam bentuk list menggunakan elemen
berupa component. Adapun directive yang kita gunakan adalah v-for dan v-bind atau :.
```javascript
// deklarasi component book, lihat contoh sebelumnya
// ...
// deklarasi object vue dengan data books
var vm = new Vue({
 el: '#app',
 components: {
 'book': BookComponent,
 },
 data: {
 books : [
 {
 id: 99,
 title: 'C++ High Performance',
 description: 'Write code that scales across CPU
registers, multi-core, and machine clusters',
 authors: 'Viktor Sehr, Björn Andrist',
 publish_year: 2018,
 price: 100000,
 image: 'c++-high-performance.png'
 },
 {
 id: 100,
 title: 'Mastering Linux Security and Hardening',
 description: 'A comprehensive guide to mastering the art
of preventing your Linux system from getting compromised',
 authors: 'Donald A. Tevault',
 publish_year: 2018,
 price: 125000,
 image: 'mastering-linux-security-and-hardening.png'
 },
 {
 id: 101,
 title: 'Mastering PostgreSQL 10',
 description: 'Master the capabilities of PostgreSQL 10 to
efficiently manage and maintain your database',
 authors: 'Hans-Jürgen Schönig',
 publish_year: 2016,
 price: 90000,
 image: 'mastering-postgresql-10.png'
 },
 {
 id: 102,
 title: 'Python Programming Blueprints',
 description: 'How to build useful, real-world
applications in the Python programming language',
 authors: 'Daniel Furtado, Marcus Pennington',
 publish_year: 2017,
 price: 75000,
 image: 'python-programming-blueprints.png'
 },
 ]
 }
})
```
Properti data books berbentuk array dari objek, silakan merujuk kembali ke bab yang membahas tentang List.
Kemudian pada template utama, kita akan gunakan v-for untuk merender data books di atas, sebagai berikut.
 ```javascript
 <div id="app">
 <book
 v-for="book in books"
 :key="book.id"
 :title="book.title"
 :description="book.description"
 :image = "book.image"
 >
 </book>
</div>
```
Sebagaimana penjelasan sebelumnya, maka semua atribut untuk props kita perlu tambahkan directive v-bind
atau disingkat : karena nilainya dinamis mengikuti objek book pada v-for.
 
Supaya kode lebih rapi dan mudah dibaca, tentunya kita bisa saja mengubah tipe data yang kita kirimkan ke
component melalui props, di mana sebelumnya teks biasa menjadi objek buku.
```javascript
var BookComponent = {
 data () {
 return {
 classCard: 'card'
 }
 },
 props: [ 'book' ],
 template : `
 <div :class="classCard">
 <h3>{{ book.title }}</h3>
 <img :src="'images/books/'+book.image" style="width:100%">
 <p v-html="book.description"></p>
 </div>
 `
}
```
Karenya, sekarang kode template utamanya cukup seperti berikut
```javascript
<div id="app">
 <book
 v-for="book in books"
 :key="book.id"
 :book="book"
 >
 </book>
</div>
```
 
### Content Distribution with Slots
Pada bagian terdahulu tentang mengirimkan data ke Component, telah dibahas mengenai cara mengirimkan
data ke component dengan menggunakan props, nah pada bagian ini kita akan belajar mengirimkan konten
ke dalam component.
```javascript
Vue.component('information', {
 template: `
 <div class="card">
 <strong>Informasi</strong>
 </div>`
})
```

Sehingga untuk mengaksesnya pada template cukup dengan <information></information>, Nah kita
tidak bisa serta merta menambahkan sesuatu konten di antara tag information tersebut layaknya tag html
biasa.

Lalu bagaimana kita bisa menambahkan konten di dalam component? caranya, kita harus deklarasikan
menggunakan elemen slot.
```javascript
Vue.component('information', {
 template: `
 <div class="card">
 <strong>Informasi</strong>
 <hr>
 <slot></slot>
 </div>
 `
})
```
Kemudian component tersebut dapat kita panggil sebagai berikut.
```javascript
<div id="app">
 <information>
 <p>Hati-hati lantai licin!</p>
 </information>
</div>
```
Nah, pada contoh di atas, elemen <slot></slot> akan di-replace dengan konten <p>Hati-hati
lantai licin!</p>

### Fallback Slots
Kita juga diizinkan memberikan konten default untuk suatu slot yang tidak diset di parent, caranya dengan
menambahkan konten di dalam tag slot.
```javascript
<div class="card">
 <strong>Informasi</strong>
 <hr>
 <slot>Tanpa informasi</slot>
</div>
```

Sehingga jika component ini dipanggil menyertakan kontent dari slot misalnya:
```javascript
<information></information>
```
Maka ketika dirender akan menampilkan konten Tanpa informasi
Sedangkan jika kita definisikan konten slotnya sebagai berikut:
```javascript
<information>
 <p>Hati-hati lantai licin!</p>
</information>
```
Maka ketika dirender akan menampilkan konten <p>Hati-hati lantai licin!</p>
### Penamaan Slot
Bagaimana jika konten yang akan kita kirimkan lebih dari satu bagian? misal pada component information
kita memiliki dua bagian konten di mana bagian pertama berisi konten judul dari informasi dan konten kedua
berisi isi dari informasi, kodenya sebagai berikut
```javascript
div class="card">
 <slot>Judul Informasi</strong>
 <hr>
 <slot>Isi Informasi</slot>
</div>
```
a Vue mempunyai cara untuk mendefinisikan masing-masing
konten slot tersebut yaitu dengan memberikan atribut name pada tag slot.
```javascript
<div class="card">
 <slot name="judul"></strong>
 <hr>
 <slot name="isi"></slot>
</div>
```
Sehingga kode componentnya menjadi
```javascript
Vue.component('information', {
 template: `
 <div class="card">
 <slot name="judul"></strong>
 <hr>
 <slot name="isi"></slot>
 </div>
 `
})
```
Lalu bagaimana cara mendefinisikan konten masing-masing slot component tersebut pada template utama?
Kita bisa melakukannya dengan menggunakan elemen template dengan attribut v-slot yang bernilai nama
dari slot yang dituju

### Dynamic Components
Ide dari dynamic component ini adalah kita bisa memuat (meload) suatu component yang sudah diregister
secara dinamis, atau hanya akan kita muat jika kita perlukan saja.
Misalnya kita memiliki dua component yaitu list dan detail.

Nah, kita bisa memuat component secara dinamis dengan menggunakan elemen <component>, elemen ini
memiliki directive v-bind:is atau :is untuk memantau pergantian component yang dimuat.
  
```javascript
<component :is="currentComponent"></component>
```
Baik, untuk mensimulasikannya mari kita edit kode kita pada objek Vue, tambahkan variabel
currentComponent, seperti berikut ini.
```javascript
 var list = {
            template: `
                <div class="card">
                    <strong>Bahasa Pemrograman</strong>
                    <ul>
                        <li>Javascript</li>
                        <li>PHP</li>
                        <li>Java</li>
                    </ul>
                </div>
                `
        }
        var detail = {
            template: `
                    <div class="card">
                        <strong>PHP</strong>
                        <p> PHP adalah singkatan dari PHP Hypertext Preprocessor. </p>
                    </div>
        `
        }
        var vm = new Vue({
            el: '#app',
            data: {
                currentComponent: 'list'
            },
            components: {
                'list': list,
                'detail': detail,
            },
        })
```
Lalu pada template ubah menjadi.

```javascript
<div id="app">
        <button @click="currentComponent = 'list'">List</button>
        <button @click="currentComponent = 'detail'">Detail</button>
        <hr>
        <component :is="currentComponent"></component>
</div>
```
### Transition Effect
Vue menyediakan cara yang sederhana untuk memberikan efek animasi transisi. Pada bagian sebelumnya
yaitu dynamic component, kita dapat menambahkan efek animasi atas pergantian component supaya
pergantiannya terasa lembut dan menarik.
Caranya dengan menggunakan elemen <transition>. Ubah template menjadi sebagai berikut
```javascript
   <div id="app">
        <button @click="currentComponent = 'list'">List</button>
        <button @click="currentComponent = 'detail'">Detail</button>
        <hr>
        <transition name="fade" mode="out-in">
            <component :is="currentComponent"></component>
        </transition>
    </div>
```
