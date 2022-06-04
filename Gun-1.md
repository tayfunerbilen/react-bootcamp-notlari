# 1. Gün

## React Nedir?

React, kullanıcı arayüzleri geliştirmek için kullanılan bir javascript kütüphanesidir.

Geliştirme yaparken nodejs'e ihtiyaç duyar. Bu yüzden node.js'in geliştirme yapılan cihazda kurulu olması gerekir.

## Node.js Kurulumu

İşletim sistemine uygun olan versiyonunu [buradan](https://nodejs.org/tr/download/) indirerek normal bir program kurar gibi kurabilirsiniz.

Kurduğunuzdan emin olmak için kurulum sonrası terminalinizi açıp şu komutu çalıştırarak testini yapabilirsiniz:

```shell
node -v
```

Eğer versiyonu görüyorsanız başarılı olmuşsunuzdur.

> **Not:** `Windows` ortamına `NodeJS` kurmuş olanlar terminal olarak hali hazırda mevcut olan `PowerShell` i ya da biraz daha gelişmiş olan [Cmder](https://cmder.net/) terimalini kurarak testini yapabilir.

### Node.js ile js dosyalarını çalıştırma

Node.js ile bir javascript dosyasını çalıştırmak için şu komutu kullanmanız yeterli;
```shell
node <dosya_yolu>
```

Burada `<dosya_yolu>` yerine sizin oluşturduğunuz javascript dosyasının yolunu vereceksiniz. Örneğin;

```shell
node ./index.js
```

## NPM (Node Paket Yöneticisi)

NPM, açılımındanda anlaşılacağı üzere nodejs'in paket yöneticisidir. [Buradan](https://www.npmjs.com/) kullanmak istediğiniz paketleri `npm` yardımıyla projenize kurabilir, yönetebilirsiniz.

NPM ile bir paket yüklemek için şu komut kullanılır:

```shell
npm install <paket_adi>
# kısa hali
npm i <paket_adi>
```

Bazı durumlarda kuracağınız paketleri projenize özel değilde global olarak kurmanız gerekebilir. Bu durumda global kurulum yapmak için `-g` flag'ı eklenir.

```shell
npm install -g <paket_adi>
# kısa hali
npm i -g <paket_adi>
```

NPM ile bir paketi kaldırmak için şu komut kullanılır:

```shell
npm uninstall <paket_adi>
```

Global olarak kurulan bir paketi kaldırmak içinde yine `-g` flag'ı kullanılır.

```shell
npm uninstall -g <paket_adi>
```

Paketler zamanla güncellendiği için projenizde kullandığınız paketleri güncellemek isteyebilirsiniz. Eğer topluca bir güncelleme yapmak isterseniz şu komutu kullanabilirsiniz:

```shell
npm update
```

Eğer global olarak kurduğunuz tüm paketleri güncellemek isterseniz `-g` flag'ı eklemeniz gerekir.

```shell
npm update -g
```

Eğer belirli bir paketi güncellemek isterseniz şu komutu kullanabilirsiniz:

```shell
npm update <paket_adi>
```

Eğer global olarak kurduğunuz belirli bir paketi güncellemek isterseniz de yine `-g` flag'ı eklemelisiniz.

```shell
npm update -g <paket_adi>
```

## package.json Dosyası

`package.json` dosyası javascript/node projenizin ana dizininde bulunan bir JSON dosyasıdır. Projeye ait üst bilgileri ve projenin bağımlılıklarını, scriptlerini, versiyonunu vb. bilgileri tutar.

### package.json Dosyası Oluşturmak

Bunun farklı yolları vardır. Eğer otomatik oluşturmak isterseniz şu komutu çalıştırarak sizden istenen bilgileri doldurarak oluşturabilirsiniz:

```shell
npm init
```

ya da `-y` flag'ı ile varsayılan değerlerle bilgi doldurmadan hızlıca oluşturabilirsiniz:

```shell
npm init -y
```

ya da dilerseniz hiç terminali işin içine karıştırmadan projenin kök dizininde `package.json` adıyla bir dosya oluşturup zorunlu alanları ekleyebilirsiniz. Zorunlu alanlar `name` ve `version` alanlarıdır.

```json
{
  "name": "proje-adi",
  "version": "1.0.0"
}
```

### package.json içinde kullanılacak değerler

#### `name` değeri

Projenin adını temsil eder ve tanımlanması zorunludur. Proje adını tanımlarken şunlara dikkat etmelisiniz:

- tümü küçük harf olmalıdır
- tek kelime olmalıdır
- tire, alt çizgi ve nokta içerebilir
- alt çizgi ya da nokta ile başlamamalıdır.

```json
"name": "proje-adi.falan_filan"
```

#### `version` değeri

Projenin versiyonunu temsil eder ve tanımlanması zorunludur. Versiyonlamayı [semantic versiyonlamaya](https://semver.org/) uygun olarak belirlemelisiniz.

Yani 3 değerli bir versiyon olacak.
`MAJOR.MINOR.PATCH` şeklinde.

- `MAJOR` - Bir önceki versiyon tamamen ya da kısmen değiştirildiyse ve mevcut yapı artık korunmuyorsa ise
- `MINOR` - Mevcut yapı korunarak yeni özellikler eklenmiş ise
- `PATCH` - Mevcut yapı korunarak hatalar vs. düzeltilmiş ise.

```json
"version": "1.0.0"
```

#### `description` değeri

Projenin ne olduğu, amacı genelde bu değerde belirtilir.

```json
"description": "Bir amacı olmayan proje"
```

#### `engines` değeri

Projenin çalışması için gerekli olan sürümler burada anahtar-değer şeklinde belirtilir. Örneğin projenizin çalışması için node'un belli bir sürümü gerekiyorsa bunu şöyle belirtebilirsiniz:

```json
"engines": {
  "node": "14.18.0"
}
```

#### `dependencies` değeri

Projenin bağımlılıkları burada belirtilir. Yani projenin ihtiyaç duyduğu paketler. Yeni bir paket yüklediğinizde bu listeye otomatik eklenecektir.

```json
"dependencies": {
    "bcryptjs": "^2.4.3",
    "cors": "^2.8.5",
    "dotenv": "^6.1.0",
    "express": "^4.16.4",
}
```

#### `devDependencies` değeri

Adındanda anlaşılacağı üzere, projenizin geliştirme ortamında ihtiyaç duyduğunuz ancak production ortamında ihtiyacınız olmayacak paketler.

```json
"devDependencies": {
    "eslint": "^4.19.1",
    "mocha": "^6.2.0",
    "nodemon": "^1.19.1",
}
```

#### `scripts` değeri

Çalıştırılmasını istediğiniz komutları burada anahtar-değer şeklinde belirtiyorsunuz. Örneğin node ile bir javascript dosyasını çalıştırmak için kullandığımız komutu script'lere ekleyebilirdik:

```json
"scripts": {
  "baslat": "node ./index.js"
}
```

ve artık uzun uzun yazmak yerine şu şekilde çalıştırabilirdik:

```shell
npm run baslat
```


#### `keywords` değeri

Proje ya da paketle ilgili anahtar kelimeleri belirliyoruz.

```json
"keywords": ["node", "test", "learning"]
```
#### `type` değeri

Varsayılan olarak node kendi modül yapısı olan `commonjs` kullanıyor. Yani [dayjs](https://www.npmjs.com/package/dayjs) paketini yükleyip bunu kullanmak isteseydik şöyle kullanacaktık:

```shell
# paketi kur
npm i dayjs
```

paketi js dosyamızda çağırıp basit bir kullanımını görelim.

```js
// index.js
let dayjs = require('dayjs')

console.log('Bugünün tarihi', dayjs().format('YYYY-MM-DD'))
```

Eğer `type` değerini `module` e çevirirsek, artık ES6 modül yapısını kullanabiliriz.

```js
// index.js
import dayjs from 'dayjs'

console.log('Bugünün tarihi', dayjs().format('YYYY-MM-DD'))
```

> Bu modül yapısını daha detaylı inceleyeceğiz, merak etmeyin :)

## ES6 Yenilikleri

JavaScript'in ES6 sürümü ile gelen ve react yazarkende sıkça kullanacağımız yenilikleri tek tek inceleyelim. React ile geliştirme yaparken bunlara hakim olmak sizi projenizi yazarken daha özgüvenli olmanızı sağlayacaktır.

### Arrow (Ok) Fonksiyonu

Javascript'de bugüne kadar `function` anahtar kelimesiyle fonksiyon oluşturmuş olabilirsiniz. Ancak ES6 ile birlikte arrow fonksiyonları oluşturmak ve kullanmak çok daha kolay.

Normal bir fonksiyon şöyle oluşturuluyor:

```js
function sayHi(name) {
	return `Hi, ${name}`
}
```

Bu fonksiyonunun aynısını arrow fonksiyonu ile oluşturmak isteseydik:

```js
const sayHi = (name) => {
	return `Hi, ${name}`
}
```

Eğer tek bir parametre alıyorsanız `()` kullanmadan da yazabilirsiniz.

```js
const sayHi = name => {
	return `Hi, ${name}`
}
```

Ve tek satıra bir return işlemi yapıyorsanız şöyle daha kısa olarak yazabilirdiniz:

```js
const sayHi = name => `Hi, ${name}`
```

### Modül Yapısı

ES6 ile birlikte standart haline gelmiş dil seviyesinde bir modül yapımız var. Temel olarak yaptığımız kodlarımızı ayırmak ve ihtiyaç halinde bunları çağırarak kullanmak.

`export` ile dosyalardan değişken, fonksiyon vs. dışa aktarabiliyoruz. Ve ihtiyaç halinde ilgili dosyayı `import` ederek export edilen değerlere ulaşıp kullanabiliyoruz.

#### `export` işlemi

Bir javascript dosyasından daha sonra kullanmak üzere hemen hemen her şeyi dışa aktarabiliriz. Örneğin bir değişken, bir fonksiyon, bir class vs.

Dışa aktarırken `default` olarak ya da obje olarak aktarım yapabiliriz. Elbette bu aktarım türüne göre çağırırken bazı farklılıklar olacak.

##### `default export` kullanımı

Örneğin verdiğimiz değeri tersine çevirip geri döndüren bir fonksiyonumuz olsun. Ve bunu varsayılan olarak dışarı aktarmayı görelim.

```js
// utils.js
function getReverseText(text) {
  return text.split('').reverse().join('')
}

export default getReverseText
```

Şimdi de dışa aktarılan fonksiyonu bir başka dosyada çağırıp kullanalım.

```js
// app.js
import getReverseTextFunction from "./utils.js"

console.log(getReverseTextFunction('tayfun erbilen'))
```

Ve bunu node yardımı ile çalıştırıp testimizi yapalım.

```shell
node ./app.js
```

Sonuç olarak başarıyla terse çevrilmiş bir değer göreceğiz. Fark ettiğiniz üzere import ederken export edilen fonksiyonun adı yerine farklı bir isim kullandık. Çünkü varsayılan olarak dışa aktarılan değeri istediğimiz gibi isimlendirebiliriz.

##### çoklu `export` kullanımı

Varsayılan olarak export işlemi haricinde farklı şeyleri de dışa aktarmak isteyebiliriz. Bunu yapmak için `default` değeri olmadan dışa aktarmak gerekir. Aynı dosyada birkaç şeyi daha dışarı aktaralım.

```js
// utils.js
const API_URL = 'https://api.test.com/'

const secondsToTime = seconds => {
  return new Date(seconds * 1000)
    .toISOString()
    .substr(11, 8)
}

function getReverseText(text) {
  return text.split('').reverse().join('')
}

export default getReverseText

export {
  API_URL,
  secondsToTime
}
```
Şimdi bu dışa aktardığımız şeyleri yine kullanmayı deneyelim.

```js
// app.js
import getReverseTextFunction, { API_URL, secondsToTime } from "./utils.js"

console.log(getReverseTextFunction('tayfun erbilen'))
console.log(API_URL)
console.log(secondsToTime(360))
```

Gördüğünüz gibi varsayılan olarak dışa aktarmıyorsak obje içinde dışa aktarılanları alıp kullanmamız gerekiyor. Ve isimlendirme dışa aktarılanla birebir aynı olmalı.

Ayrıca en altta `export {}` ile belirtmek yerine değişken ya da fonksiyonların başına koyarakta bu dışa aktarımı uygulayabilirdik.

```js
// utils.js
export const API_URL = 'https://api.test.com/'

export const secondsToTime = seconds => {
  return new Date(seconds * 1000)
    .toISOString()
    .substr(11, 8)
}

export default function getReverseText(text) {
  return text.split('').reverse().join('')
}
```

##### `as` ile dışa aktarılanı yeniden isimlendirme

Değişken, fonksiyon ya da sınıfları dışa aktarırken farklı bir isimle kullanmak isterseniz `as` ile yeniden izimlendirip dışa aktarabilirsiniz.

```js
// ...

export {
  API_URL as BASE_URL,
  secondsToTime as convertSeconds
}
```

##### `as` ile içe aktarılanı yeniden isimlendirme

İçe aktarılan değişken, fonksiyon ya da sınıfları yeniden isimlendirmek içinde `as` değerini kullanabilirsiniz.

```js
import getReverseText, { API_URL as BASE_URL, secondsToTime as convertSeconds } from "./utils.js";
```

##### `import *` kullanımı

Tüm dışa aktarılanları tek bir isimde toplayıp kullanmak isterseniz `import *` değerinden sonra `as` ile isimlendirip kullanabilirsiniz.

```js
import * as Utils from "./utils.js"

console.log(Utils)
console.log(Utils.API_URL)
console.log(Utils.default('tayfun erbilen'))
```

##### Dinamik import

Bazı durumlarda dosyayı direk import etmek yerine ihtiyaç anında import edip kullanmak isteyebilirsiniz. İşte bu gibi durumlarda `import()` metodunu kullanacağız.

Bu bize bir `Promise` dönecek. Henüz öğrenmediğimiz bir kavram ancak kullanımına bakalım, promise dersinde buraya dönüp tekrar incelersiniz kodu.

```js
setTimeout(() => {
	console.log('import ediliyor..')
	import('./utils.js')
		.then(utils => {
			console.log(utils)
		})
}, 2000)
```

Ayrıca yine henüz öğrenmediğimiz `async/await` yapısı ile de şöyle yapabilirdik:

```js
setTimeout(async () => {
  console.log('import ediliyor..')
  const utils = await import('./utils.js')
  console.log(utils)
}, 2000)
```

### Callback ve Callback Hell

Bir metoda parametre olarak atanan fonksiyonlara `callback fonksiyonları` denir. Callback fonksiyonları aslında faydalıdır ancak bazı durumlar hariç.

Mesela şöyle bir senaryomuz olsun, bazı görevler başlatacağız, fakat başlanan görev bitmeden diğer göreve başlamasını istemiyoruz. Örneğin kod yapımız şöyle olabilirdi:

```js
setTimeout(() => {
  console.log('1. task')
  setTimeout(() => {
    console.log('2. task')
    setTimeout(() => {
      console.log('3. task')
      setTimeout(() => {
        console.log('4. task')
        setTimeout(() => {
          console.log('5. task')
          setTimeout(() => {
            console.log('6. task')
          }, 1000)
        }, 1000)
      }, 1000)
    }, 1000)
  }, 1000)
}, 1000)
```

işte tam bu noktada bu şekli alıyorsa kodlarımız `callback hell` dediğimiz illete yakalanıyor :D

Yani callback içinde callback içinde callback... Bu böyle gider, bunu daha düzenli yazmak için `Promise` kullanabilirdik.

### `Promise` kullanımı

Promise, ES6 ile gelen özelliklerden bir tanesi. Amaç ise, bir işlem başarılı ise yani `resolve` olmuşsa ya da başarısız ise yani `reject` olmuşsa ve her koşulda çalışabilecek kod blokları yazmak.

En basit haliyle bir promise yapısına bakacak olursak:

```js
const promise = new Promise((resolve, reject) => {
  if (true) {
    resolve('başarılı')
  } else {
    reject('başarısız!')
  }
})
```

Yukarıdaki örnekte her türlü `resolve()` metodu çalışacak ancak önemli değil, mantığı anlamak için bakabiliriz.

Şimdi bu promise yapısında başarılı olanlar yani resolve edilenler `then()` bloğunda yakalanıyor.

Başarısız yani reject edilenler ise `catch()` bloğunda yakalanıyor.

Her iki durumda da bir kod bloğu çalıştırmak isterseniz de `finally()` bloğunu kullanabilirsiniz.

Buna göre yukarıdaki promise'i çalıştıralım.

```js
promise.then(response => console.log(response))
  .catch(error => console.log(error))
  .finally(() => console.log('paşa gönlüme göre!'))
```

Şimdi mesela basit bir örnek yapalım. Callback hell bölümünde bahsettiğimiz o cehennemden kurtulmak için promise kullanalım.

```js
const wait = second => new Promise(resolve => setTimeout(resolve, second * 1000))

wait(1)
  .then(() => {
    console.log('1. task')
    return wait(1)
  })
  .then(() => {
    console.log('2. task')
    return wait(1)
  })
  .then(() => {
    console.log('3. task')
    return wait(1)
  })
  .then(() => {
    console.log('4. task')
    return wait(1)
  })
```

> Unutmayın, `then()` ile return ettiğiniz işlemi bir sonraki then'de yine callback'te parametre olarak alabilirsiniz.

### `Async/Await` Kullanımı

Javascript'de her şey senkron olarak işleniyor. Bazı durumlarda, asenkron işlemleri sıraya koymamız gerekebilir.

Yine yukarıdaki örneği düşündüğümüzde, API'mize bir istek attığımızda cevabın ne zaman döneceği belli olmadığı için, `Promise` yapısını kullanarak bunu çözüyoruz.

Ancak `Promise` de her zaman `than()` `catch()` ve `finally()` bloklarını kullanmak istemeyebiliriz. Bu gibi durumlarda, `async/await` kullanacağız.

Yukarıdaki örneğimizi `async/await` yapısına çevirelim.

```js
const wait = second => new Promise(resolve => setTimeout(resolve, second * 1000))

async function runTasks() {
  await wait(1)
  console.log('1. task')
  await wait(1)
  console.log('2. task')
  await wait(1)
  console.log('3. task')
  await wait(1)
  console.log('4. task')
}

runTasks()
```

Gördüğünüz gibi bir fonksiyon içinde bunu yapmamız gerekiyor. Çünkü fonksiyon'a `async` değerini atıyoruz.

Böylece fonksiyon içinde işlemin bitmesi gerektiğini `await` ile belirtiyoruz. Bu cevap dönene kadar altındaki kodlar çalışmıyor. Ne zmana cevap döndü, bir sonraki kod çalıştırılıyor ve tekrar bir bekleme varsa onu bekleyip yine işlem bu şekilde devam edip gidiyor.

### `spread` ve `rest` operatörleri

`spread` operatörü adındanda belli olduğu üzere bir diziyi ya da objeyi yaymak için kullanılıyor.
`rest` operatörü de yine adındanda belli olduğu üzere kalanı temsil ediyor.

Her ikiside `...degisken` şeklinde kullanılıyor. Kullanıldığı yere göre `spread` ya da `rest` işlemini görüyor.

Örneğin bir dizimiz olsun ve bu dizi elemanlarını fonksiyona göndererek console'a basalım.

```js
const names = ['Tayfun', 'Mehmet', 'Ahmet']
function showNames(name1, name2, name3) {
  console.log(name1)
  console.log(name2)
  console.log(name3)
}
showNames(names[0], names[1], names[2])
```

Sonuç isimleri tek tek yazdıracaktır. Ancak fonksiyon'a böyle dizi elemanlarını tek tek belirtmek biraz sıkıntılı. Çünkü diziye yeni bir eleman geldiğinde bunu tekrar şöyle belirtmem gerekli.

```js
showNames(names[0], names[1], names[2], names[3])
```

Bunun yerine eğer diziyi yayıyor olsaydım o zaman şöyle yapmam yeterli olacaktı.

```js
showNames(...names)
```

Artık `showNames()` fonksiyonunda bir değişiklik yapmadan yine aynı sonucu aldım.

Peki, `spread` operatörünü gördük. Olay bundan ibaret, `rest` operatörü? O da fonksiyonda kullanacağız. Şöyle ki;

```js
const names = ['Tayfun', 'Mehmet', 'Ahmet', 'Gökhan', 'Zerafet']
function showNames(name1, name2, name3, ...otherNames) {
  console.log(name1)
  console.log(name2)
  console.log(name3)
  console.log(otherNames)
}
showNames(...names)
```

Ayrıca `spread` ile referanssız bir objede tanımlayabiliriz. Mesela bir user objemiz olsun, bir başka user'ı bu objeden türetip kullanmayı deneyelim.

```js
const user = {
  name: 'Tayfun',
  surname: 'Erbilen'
}
const newUser = user
newUser.name = 'Recep'

console.log(user)
console.log(newUser)
```

Gördüğünüz gibi her iki objede değişti, çünkü `newUser` referans olarak `user` ı baz alıyor. Dolayısı ile onda bir değişiklik yapınca asıl referans aldığımız objede değişiyor. Ama bunu `spread` ile yayıp şöyle kullansaydık her şey istediğimiz gibi olabilirdi.

```js
const user = {
  name: 'Tayfun',
  surname: 'Erbilen'
}
const newUser = {
  ...user
}
newUser.name = 'Recep'

console.log(user)
console.log(newUser)
```

Bir başka örnekte, react'de sıkça kullanacağımız servisleri yönetmekle alakalı olabilir. Düşünün ki istekleri yönettiğiniz `request` adında bir metodunuz olsun. Ve bu metod bazı varsayılan ayarlar alıyor olsun.

```js
function request(url, opts = {}) {
  const options = {
    error: true,
    success: true,
    ...opts
  }
  console.log(options)
}

request('/', {
  success: false
})
```

Ne yapmış olduk? Varsayılan ayarları, parametre'de göndererek ezmiş olduk. Böylece bu istekte başarılı olursa bir başarılı mesajı göstermek istemediğimi söyledim.

Başka bir örnekte dizileri birleştirmek olabilir. Örneğin;

```js
const numbers = [0, 1, 2, 3]
console.log( [-2, -1, ...numbers, 4, 5] )
```

### `Destructuring`

`Destructuring`, dizi ve objelerde değerleri isimlendirerek kullanmamızı sağlıyor.

Örneğin şöyle bir dizimiz olsun:

```js
const user = ['Tayfun', ' ', 'Erbilen']
console.log(user[0], user[1], user[2])
```

bu şekilde kullanmak yerine daha anlamlı bir isimlendirme ile şöyle kullanabilirdik:

```js
const user = ['Tayfun', ' ', 'Erbilen']

const [name, space, surname] = user
console.log(name, space, surname)
```

Obje içinde bir örnek verecek olursak:

```js
const user = {
  name: 'Tayfun',
  surname: 'Erbilen',
  age: 29
}

console.log(user.name)
console.log(user.surname)
console.log(user.age)
```

Yine böyle uzun uzun yazmak yerine isimlendirip şöyle kullanabilirdik:

```js
const user = {
  name: 'Tayfun',
  surname: 'Erbilen',
  age: 29
}

const { name, surname, age } = user

console.log(name)
console.log(surname)
console.log(age)
```

Farkettiğiniz üzere dizilerde `[]` kullanırken objelerde `{}` kullanıyoruz.

Ayrıca dizilerde bir ismi olmadığı için istediğimiz isimle kullanırken, objelerde mevcut özellik adını yazmak zorundayız.

Diyelim ki yazmak istemiyoruz, o zaman yeniden isimlendirmeyi sonuna `:` koyarak yapabiliriz. Örneğin:

```js
const user = {
  name: 'Tayfun',
  surname: 'Erbilen',
  age: 29
}

const { name: ad, surname: soyad, age: yas } = user

console.log(ad)
console.log(soyad)
console.log(yas)
```

Bu genelde, çalıştığınız bir API düzgün response dönemiyorsa bir standart haline getirmek için tercih edilebilir.

Ayrıca bu `user` objesini bir fonksiyona parametre olarak gönderseydik, fonksiyon içinde de isimlendirip kullanabilirdik. Şöyle ki:

```js
const user = {
  name: 'Tayfun',
  surname: 'Erbilen',
  age: 29
}

function showUser({ name, surname, age }) {
  console.log(name, surname, age)
}
showUser(user)
```

Ayrıca objemiz iç içe de obje içerebilirdi. Bu durumda onlarıda isimlendirerek kullanmak isteseydik şöyle bir örnek verebilirdik:

```js
const user = {
  name: 'Tayfun',
  surname: 'Erbilen',
  age: 29,
  pets: {
    dogs: ['Monti'],
    cats: ['Paşa', 'Göbek']
  }
}

function showUser({ name, surname, age, pets: { dogs: [ kopek1 ], cats: [ kedi1, kedi2 ] } }) {
  console.log(kopek1)
}
showUser(user)
```

Eğer değer yoksa varsayılan bir değerde belirtebilirdik: Örneğin:

```js
const user = {
  name: 'Tayfun',
  //surname: 'Erbilen',
  age: 29
}

function showUser({ name, surname = 'Soyad Yok!', age }) {
  console.log(name, surname, age)
}
showUser(user)
```

### Higher Order Fonksiyonlar (Yüksek Mertebe)

Parametre olarak fonksiyon olan metodlara yüksek mertebe fonksiyonlar diyebiliriz. Javascript'de diziler için bir takım yüksek mertebe fonksiyonlar var, bunlara da sırasıyla bakalım.

#### `map()` metodu

..

#### `filter()` metodu

...

#### `reduce()` metodu

...

#### `find()` metodu

...

#### `findIndex()` metodu

...

#### `includes()` metodu

...

#### `every()` ve `some()` metodu
