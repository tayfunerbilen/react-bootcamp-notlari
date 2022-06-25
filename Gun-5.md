# 5. Gün

## NPM'de Paket Paylaşmak

Bir react paketi oluşturup NPM'de paylaşarak open-source dünyasına katkı sağlayabilirsiniz. Peki en zahmetsiz şekilde bunu nasıl yapabilirsiniz? `create-react-library` paketi ile. Hadi gelin nasıl kurarız bir bakalım.

### Kurulum

Öncelikle paketi global olarak kurmak isterseniz:

```
npm i -g create-react-library
```

npx ile kurmak istersenizde (ilgili dizinin içinde):

```js
npx create-react-library
```

Size belli başlı sorular soracak, bunları cevaplayarak kurulumu gerçekleştirin. Paket adı, açıklaması varsa github linki vs.

Kurulduktan sonra iki farklık klasör yapınız olacak.

./src - Geliştirmelerinizi burada yapacaksıız
./example - Denemelerinizi burada yapacaksınız

### Yayınlama

Paketinizi npm'de yayınlamak istediğinizde ilk olarak npm'e şu komutla giriş yapmalısınız:

```shell
npm login
```

Kullanıcı adı, parola ve OTP kodunuz ile giriş yapacaksınız.

Daha sonra şu komutla publish işlemini yapabilirsiniz:

```shell
npm publish
```

### Sürüm Güncelleme

Paketinizde bir sürüm güncellemek istediğinizde bunu semantic versiyonlamaya uygun yapmanız gerekir. Komut ile şu şekilde sürüm güncellemesi yapabilirsiniz:

```shell
npm version patch --force
npm version minor --force
npm version major --force
```

### Paket Kaldırma

Yayınlanan paketi kaldırmak için şu komutu kullanabilirsiniz:

```js
npm unpublish <paket_adi>
```

## Gerçek Zamanlı İşlemler (Realtime Socket)

Gerçek zzamanla verilerle çalışmak için ilk olarak bir backend hazırlamamız gerekiyor. Öncelikle `server` adında bir klasör oluşturalım ve şu komutu çalıştıralım.

```shell
npm init -y
```

Daha sonra kullanacağımız paketleri yükleyelim.

```shell
npm i express cors socket.io nodemon
```

Ve `server/index.js` dosyasını açıp şu kodları yazalım.

```js
import express from "express"
import http from "http";
import cors from "cors"
import { Server } from "socket.io";

const app = express()

app.use(cors())

const server = http.createServer(app)
const io = new Server(server, {
	cors: {
		origin: 'http://localhost:3001',
		methods: ['GET', 'POST']
	}
})

io.on('connection', socket => {
	console.log('user connected', socket.id)
	socket.on('disconnect', () => {
		console.log('user disconnected')
	})
})

server.listen(3004, () => console.log('server running!'))
```

Daha sonra `server/package.json` dosyamızda `scripts` kısmını şöyle değiştirelim.

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js"
},
```

Ve ek olarak `type` kısmını belirtelim.

```json
"type": "module",
```

Artık şu komutlarla projemizi ayağa kaldırabiliriz.

```shell
npm start
npm run dev
```

Sıra geldi `client` klasörü oluşturup içine react projesi kurmaya.

```shell
cd client && npx create-react-app .
```

Daha sonra react tarafında `socket.io` nun client'ını kurmamız gerekiyor.

```shell
npm i socket.io-client
```

Ve artık şu şekilde bağlantımızı gerçekleştirebiliriz:

```js
// ./App.js
import io from "socket.io-client"
const socket = io.connect('http://localhost:3004')

function App() {
  return (
    <>
      Chat App
    </>
  )
}
```

Socket'e veri göndermek için `emit()` kullanılır. Örneğin:

```js
socket.emit('join', 'test-oda')
```

Ve socket tarafında gelen datayı dinlemek için `socket.on()` kullanılır. Örneğin:

```js
// server/index.js
// ...

io.on('connection', socket => {
	console.log('user connected', socket.id)
  
  socket.on('join', data => {
    // odaya bağla
    socket.join(data)
    console.log('Gönderilen değerler = ', data)
  })
  
	socket.on('disconnect', () => {
		console.log('user disconnected')
	})
})

// ...
```

Ayrıca bazı yararlı bilgiler:

```js
io.emit('event', 'data') // Herkese gönderir
socket.emit('event', 'data') // aktif socket bağlantısı olana gönderir
socket.broadcast.emit('event', 'data') // aktif socket bağlantısı hariç herkese gönderir
```
