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
