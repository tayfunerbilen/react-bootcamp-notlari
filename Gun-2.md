# 2. Gün

## Terminal

React ile proje geliştirirken mecburen terminal'de bazı kodlar yazmamız gerekecek :) İşletim sistemlerine uygun olarak terminallerimizi açmayı öğrenmeyeliz.

- MacOS kullanıyorsanız `command` + `space` kombinasyonu ile arama kutusunu açıp `terminal` yazarak terminali çalıştırabilirsiniz.
- Windows kullanıyorsanız başlatma çubuğuna basıp `CMD` yazarsanız varsayılan terminali açacaktır. Ek olarak `PowerShell` de kullanılabilir. Ayrıca harici bir terminal isterseniz [CMDER](http://cmder.net/) terminalini indirip kurabilirsiniz.

## React Kurulumu

React projesini kurmak için farklı yollar olsada, en çok tercih edilen yollardan bir tanesi olan [create-react-app](https://create-react-app.dev/) ile kurulum yapacağız.

Kurulum yapmak için ilk olarak bir klasör oluşturun. Bunu elle de oluşturabilirsiniz, `mkdir` komutu ile terminalden de oluşturabilirsiniz.

```shell
mkdir react-projesi
```

Daha sonra terminal'de oluşturduğunuz klasörün içine `cd` komutu ile giriş yapın.

```shell
cd react-projesi
```

ve şu komutla ilgili klasöre react projenizi kurun.

```shell
npx create-react-app .
```

> **Not**: Eğer `npx` hata veriyorsa `npm i -g npx` komutu ile global olarak kurup tekrar deneyin.

Burada `.` o an bulunduğunuz dizine kurmanız için gerekli değer. Yani siz `app` yazsaydınız ilgili klasörün içine `app` adıyla bir klasör açıp buraya kuracaktı.

> Kurulum ortalama 3-5 dakika arasında bir zaman alacaktır.

Kurulum başarıyla tamamlandıktan sonra projeyi şu komutla başlatın.

```shell
npm start
```

Artık `localhost:3000` adresinden react projenizi canlı olarak görebilirsiniz.

## React Proje Yapısı

Kurulumu yaptıktan sonra, ilk olarak proje yapımızı inceleyelim. Kurulan projemizde şöyle bir klasör yapısı olmasını bekliyoruz:

```
├── README.md
├── node_modules/
├── package-lock.json
├── package.json
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    ├── reportWebVitals.js
    └── setupTests.js
```

- `node_modules` - Bu klasör ilgili paketlerin yüklendiği klasördür. Yeni bir paket yüklendiğinde burada yer alacaktır, herhangi bir şekilde bu klasörle işimiz olmayacak.
- `public` - Bu klasörde static olarak tutmak istediğiniz dosyalarınız yer alacak. Projeyi build aldığınızda buradaki dosyalar değiştirilmeden build klasörüne aktarılacaktır.
  - `index.html` - Burada `#root` id'li `div` etiketi `src/index.js` altında seçilip render işlemi sağlanıyor. Bu dosyaya font dosyalarınızı çağırabilirsiniz, analytics gibi genel kodları vs. ekleyebilirsiniz.
- `src` - Projemizin tüm kodları bu klasör altında yer alacak. Geliştirmelerin tamamını buradan yapacağız.
  - `index.js` - Bu projemizin başlangıç dosyası. Burada React kütüphanesi ve ihtiyaç olanları yükleyip `#root` etiketini seçerek `App()` componentini `render()` ediyoruz. Burayıda genel olarak çok ellemeyeceğiz.
  - `App.js` - Genelde temel component'imiz bu olacak. Bunun üzerinden projemizi inşa etmeye başlayacağız ancak bunun adı herhangi bir şeyde olabilirdi. İstediğinizi koymakta özgürsünüz.
- `.gitignore` - Bir versiyon kontrol sistemi ile çalışırsanız burada git'e gönderilmeyecek dosya ve klasörler belirleniyor.
- `package.json` - 1. günde bu dosyanın ne işe yaradığını ve nasıl kullanıldığını öğrendik. Detaylar için 1. gün özetine bakabilirsiniz.
- `README.md` - Bir versiyon kontrol sistemi ile çalışıyorsanız projenin ne olduğunun anlatıldığı bir markdown dosyasıdır.

Artık klasör yapısıyla ilgili genel bir fikrimiz var. Yavaştan bir şeylere bakmaya hazırız :)

## JSX

React kodlarında gördüğünüz HTML kodlarının aslında HTML kodu olmadığını biliyor musunuz? Elbette çıktı olarak bunlar nihayetinde HTML etiketlerine dönüştürülüyor ancak bizim yazdıklarımız birer javascript objesinden ibaret. Bu yapının adıda JSX olarak geçiyor.

JSX ile HTML arasında çok fazla fark olmamakla birlikte bazı önemli farklar da mevcut elbette.

- HTML'de `class` olarak tanımlanan nitelik JSX'de `className` olarak tanımlanmalı.
- HTML'de tek kelime olmayan `tabindex` gibi nitelikler JSX'de `camelCase` yapısına uygun olarak `tabIndex` şeklinde dönüştürülür.
- HTML'de `onclick`, `onchange` gibi nitelikler yukarıdaki `camelCase` örneğine göre `onClick`, `onChange` şeklinde dönüştürülür.
- HTML'de `label` etiketinde belirtilen `for` niteliği JSX'de `htmlFor` olarak tanımlanmalı.
- HTML'de `value` niteliği JSX'de `defaultValue` olarak tanımlanmalı.
- Ayrıca `{}` içinde javascript ifadeleri çalıştırılabilir. Buna `map`, `&&` gibi yapılarda dahil.
- Ayrıca niteliklere geçilen değerler `nitelil={degisken}` şeklinde kullanılabilir.

## Component Yapısı

Component'leri belli bir amaca hizmet eden birer UI olarak düşünebilirsiniz. Örneğin button, dropdown, tab, form elemanları ve aklınıza gelecek her şey birer component olabileceği gibi react'de render edilen her şey birer component'dir.

Projemiz üstte de bahsettiğimiz gibi bir component'in üzerine inşa edilir. Belki bu kafanızı karıştırabilir, ancak zamanla daha iyi kafanızda oturacaktır.

Component isimleri büyük harfle başlar, çünkü diğer JSX etiketleri ile karıştırılmaması için bu şekilde bir kural belirlenmiştir.

En yalın haliyle bir component şuna benzer:

```jsx
// ./components/Button.js
function Button() {
  return (
    <button>buton örneği</button>
  )
}
```

`Button` adında bir component geriye ise `button` etiketiyle static bir değer döndürüyor.q

Bu component'i bir başka component içinde çağırıp kullanmak isterseniz şöyle bir yol izlemelisiniz:

```jsx
import Button from "./components/Button"

function App() {
  return (
    <div>
      <h3>Component Kullanımı</h3>
      <Button />
      <Button>Bu şekilde de kullanılabilir.</Button>
    </div>
  )
}
```

Elbette component'in bu hali pek bir şey ifade etmiyor, zira bu component'e bazı bilgiler göndererek component içinde bunlara göre kontroller yapıp geriye bir şeyler döndürmeliyiz.

### `props` Kullanımı

prop'ları bir fonksiyona gönderilen parametreler gibi düşünebilirsiniz. Component'e gönderilen prop'lar `anahtar="değer"` şeklinde gönderilir. Yukarıdaki component'imize bazı proplar göndererek daha anlamlı bir component haline dönüştürelim.

```jsx
// ./components/Button.js
function Button(props) {
  return (
    <button className={props.variant}>{props.text}</button>
  )
}
```

Artık bu component'e gönderebileceğimiz `text` ve `variant` adında iki prop'umuz var. Kullanırken şöyle kullanacağız:

```jsx
import Button from "./components/Button"

function App() {
  return (
    <div>
      <h3>Component Kullanımı</h3>
      <Button text="Buton Örneği" variant="danger" />
    </div>
  )
}
```

Burada `props` yerine 1. günde öğrendiğimiz `destructuring` ile objeleri isimleriyle kullanabiliriz. Şöyle ki:

```jsx
// ./components/Button.js
function Button({ variant, text }) {
  return (
    <button className={variant}>{text}</button>
  )
}
```

Ek olarak bu component'e bazı prop'lar opsiyonel olarak geçilebilir. Örneğin `disabled`, `onClick`, `style` gibi prop'lar geçmek isteyebilirsiniz.
Ancak bunları tek tek yazmak yerine, kalanları `rest` operatörü yardımıyla alıp `spread` operatörü yardımıyla kullanabilirsiniz.

```jsx
// ./components/Button.js
function Button({ variant, text, ...props }) {
  return (
    <button {...props} className={variant}>{text}</button>
  )
}
```

Kullanırken de şöyle kullanabilirsiniz:

```jsx
import Button from "./components/Button"

function App() {
  return (
    <div>
      <h3>Component Kullanımı</h3>
      <Button 
        onClick={() => alert('Butona tıklandı.')} 
        disabled={true} 
        style={{ backgroundColor: 'red', color: '#fff' }} 
        text="Buton Örneği" 
        variant="danger"
      />
    </div>
  )
}
```

Artık bir buton etiketine eklenecek prop'ları düşünmemize gerek yok, biz sadece bizi ilgilendirenleri alıp kalanlarını buton etiketine prop olarak geçebiliriz.

#### `children` Prop'u

Yukarıdaki buton component'ini şöyle kullanmak isteseydim, etiket içindeki değerleri nasıl alabilirdim?

```js
import Button from "./components/Button"

function App() {
  return (
    <Button>
      bu alan içindeki
      değer nasıl alınacak?
    </Button>
  )
}
```

Evet, `props` altında `children` değeri geliyor, bu etiket içindeki değeri temsil ediyor. Bu sayede bir component'in içine farklı JSX elemanlarını ekleyip, `children` prop'u ile bunları kolayca yönetebilirsiniz.

```jsx
function Button({ children, ...props}) {
  return (
    <button {...props}>
      {children}
    </button>
  )
}
```

### Ortam Değişkenleri

Bazı durumlarda bazı değerleri global olarak ortama göre tutmak isteyebilirsiniz. React ile proje geliştirirken temelde 2 ortamımız olacak. `development` ortamı yani geliştirme yaptığımız ortam ve `production` ortamı yani ürün seviyesinde ki ortamımız. 

Örneğin bir API URL'imiz olduğunu düşünelim, bu değer development ya da production ortamlarında farklı olabilir. Bu gibi değerleri `.env` uzantılı dosyalarda tutacağız.

Şu isimlerle .env dosyası oluşturabilirsiniz ve ana dizinde yer almalı:

- `.env.development` - geliştirme ortamında geçerli ortam değişkenleri
- `.env.production` - production ortamında geçerli ortam değişkenleri

Sırasıyla bu dosyaları ana dizinimizde oluşturalım ve içine 1-2 ortam değişkeni tanımlayalım.

> Ortam değişkenlerini tanımlarken ön ek olarak `REACT_APP_` gelmeli ve sonrasında değişken adınız yer almalı.

```
# .env.development
REACT_APP_API_URL=http://localhost/api
REACT_APP_TITLE=test title
```

```
# .env.production
REACT_APP_API_URL=http://test.com/api
REACT_APP_TITLE=production title
```

Artık bunları component içinde kullanmak istediğimizde şöyle erişip kullanabiliriz.

```jsx
function App() {
  return (
    <div>
      {process.env.REACT_APP_API_URL} <br />
      {process.env.REACT_APP_TITLE}
    </div>
  )
}
```

Ayrıca hangi ortamda olduğunuzu anlamak için `process.env.NODE_ENV` ile kontrol yapabilirsiniz. Sadece production ortamında bazı kodlarınızı çalıştırmak isteyebilirsiniz. Örn: analytics bilgileriniz gibi. 

```jsx
if (process.env.NODE_ENV === 'production') {
  // analytics bilgileri
}
```

Ayrıca ortam değişkenlerini HTML dosyasında da `%REACT_APP_TITLE%` şeklinde kullanabileceğinizi unutmayın.

### State Kavramı

State kavramını sıkça duyacaksınız. Aslında bir değişken ve onun taşıdığı değer gibi düşünebilirsiniz. Farkı ise, bu değişkenler reaktiftir. Yani bir değişiklik olduğunda react bunu bilir ve kullanılan yerleri günceller, buna bağlı hesaplamalar varsa bunları yeniden hesaplar vs.

Elbette tanımlarken bir değişken gibi tanımlamaktan ziyade react'in bize verdiği `useState` hook'unu (metodunu) kullanacağız.

`useState` hook'u bize bir iki değerli bir dizi döndürür. İlki state'in değeri, ikincisi ise state'i güncellemek için gerekli fonksiyondur. Biz de `destructuring` yardımı ile isimlendirip component'imizin içinde kullanacağız.

Ayrıca `useState()` içine bir başlangıç değeri belirleyeceğinizi unutmayın. Örnek kullanımlar:

```jsx
const [name, setName] = useState('tayfun')
const [tags, setTags] = useState(['html', 'css', 'js'])
const [user, setUser] = useState({
  name: 'Tayfun',
  surname: 'Erbilen'
})
```

Gelin bir component içinde basit bir state oluşturalım ve bunu ekrana basalım. Daha sonra buton'a basıldığında bu state'in değerini güncelleyelim.

```jsx
import { useState } from "react"

function App() {

  const [name, setName] = useState('tayfun')
  
  const changeName = () => {
    setName('recep')
  }
  
  return (
    <div>
      <button onClick={changeName}>İsmi değiştir</button>
      <h3>İsim = {name}</h3>
    </div>
  )
}
```

Bir başka örneği de input'dan yazılan değeri state'e atayarak görelim.

```jsx
import { useState } from "react"

function App() {

  const [name, setName] = useState('tayfun')
  
  return (
    <div>
      <input defaultValue={name} onChange={e => setName(e.target.value)} />
      <h3>İsim = {name}</h3>
    </div>
  )
}
```

### Koşullu Render

Component'lerde return işleminden sonra bir kod varsa bunlar çalışmayacaktır. Bu yüzdenk koşullu olarakta render yapabilirsiniz. Örneğin kullanıcı giriş yapmışsa başka bir değer, yapmamışsa başka bir değer return edebilirdik.

```jsx
function App() {

	const isLoggedIn = true;
	
	if (isLoggedIn) {
		return (
			<div>
				<h3>Hoşgeldiniz</h3>
			</div>
		)
	}
  
  return (
    <div>
      <button onClick={() => setShow(show => !show)}>
        {show ? 'Gizle' : 'Göster'}
      </button>
      {show && (
        <div>bu alan show state değeri true olduğunda görünür!</div>
      )}
    </div>
  )

}
```

Ayrıca return içinde de duruma göre göstermek istediğimiz alanları `&&` ile belirleyip kullanabilirdik.

```jsx
function App() {

  const [show, setShow] = useState(false)
  
  return (
    <div>
      <button onClick={() => setShow(show => !show)}>
        {show ? 'Gizle' : 'Göster'}
      </button>
      {show && (
        <div>bu alan show state değeri true olduğunda görünür!</div>
      )}
    </div>
  )

}
```

### Tekrarlanan Elemanlar

Bir dizi değerinizi `map()` metodu yardımıyla kullanabilirsiniz. Örneğin basit bir todo uygulaması yapalım, input'dan bir değer alalım ve butona basınca todos state'ini güncelleyip bu yeni değeride diziye dahil edelim. Ve diziyi `map()` yardımı ile listeleyelim.

```jsx
import { useState } from "react"

function App() {

  const [todo, setTodo] = useState('')
  const [todos, setTodos] = useState([])
  
  const addTodo = () => {
    //setTodos([...todos, todo])
    // yukarıdaki ya da aşağıdaki gibi tanımlanabilir
    setTodos(todos => [...todos, todo])
  }
  
  return (
    <div>
      <input defaultValue={todo} onChange={e => setTodo(e.target.value)} />
      <button disabled={!todo} onClick={addTodo}></button>
      <ul>
        {todos.map(todo => (
          <li>{todo}</li>
        ))}
      </ul>
    </div>
  )
}
```

### `key` Prop'u

Tekrarlanan işlemlerde react bizden tekrarlanan elemanlara `key` propu geçmemizi bekler. Bu prop'un değeri tekrarlanan işlem içinde benzersiz olmalıdır.

Genelde son çare olarak index değerini key olarak belirlemeniz beklenir. Dizi öğelerinizin değişme ihtimali varsa (örneğin dizinin sonuna başına ortasına yeni değerler geldi) bu gibi durumlarda index'i kullanmamamız tavsiye ediliyor. Şimdi nasıl kullanacağız bir bakalım.

Todo örneğindeki şu kısmı:

```js
<ul>
  {todos.map(todo => (
    <li>{todo}</li>
  ))}
</ul>
```

şöyle değiştirelim:

```js
<ul>
  {todos.map((todo, index) => (
    <li key={index}>{todo}</li>
  ))}
</ul>
```

### propTypes ile Tip Kontrolü

Component'lere geçilen prop'ların daha kontrollü olması için `prop-types` paketini kurup proplarımızın tiplerini belirtebiliriz. Genelde react projelerini bir takım halinde yazacağımız için sizden sonra gelenlerinde component'leri daha rahat kullanması ve daha anlamlı olması için bu işlemi yapmanız fena olmaz.

Paketi kurmak için:

```shell
npm i prop-types
```

Daha sonra bir component'de şu şekilde işlem yapmanız gerekir:

```jsx
// ./components/Button.js
import PropTypes from "prop-types"

function Button({ variant, text, ...props }) {
  return (
    <button {...props} className={variant}>{text}</button>
  )
}

Button.propTypes = {
  variant: PropTypes.string.isRequired,
  text: PropTypes.string.isRequired
}

export defaul Button
```

Kısaca kullanabilecekleriniz:

- PropTypes.array - diziler için
- PropTypes.bool - mantıksal değerler için
- PropTypes.func - fonksiyonlar için
- PropTypes.number - sayılar için
- PropTypes.object = nesneler için
- PropTypes.string - ifadeler için
- PropTypes.any - herhangi birisi için
- PropTypes.element - bir JSX elemanı için
- PropTypes.oneOf(['success', 'danger']) - bazı durumlarda sadece belirtilen değerlerin olduğundan emin olmak için
- PropTypes.oneOfType([ PropTypes.string, PropTypes.number ]) - birden fazla tip uygulamak için
- PropTypes.arrayOf(PropTypes.number) - bir dizi içindeki tipler için
- PropTypes.objectOf(PropTypes.string) - bir nesne içindeki tipler için
- PropTypes.shape({ name: PropTypes.string, age: PropTypes.number }) - Obje içindeki tipleri belirtmek için

> **Not:** `isRequired` değerini yukarıdaki herhangi bir tanımın sonuna ekleyip zorunlu olduğunu belirtebilirsiniz.

Ayrıca `defaultProps` ile component'e gönderilen prop'ların varsayılan değerlerini belirleyebiliyoruz.

```jsx
// ./components/Button.js
import PropTypes from "prop-types"

function Button({ variant, text, ...props }) {
  return (
    <button {...props} className={variant}>{text}</button>
  )
}

Button.propTypes = {
  variant: PropTypes.string.isRequired,
  text: PropTypes.string.isRequired
}

Button.defaultProps = {
  variant: 'default'
}

export defaul Button
```

### Form Elemanlarıyla Çalışmak

React'de en çok kullanacağımız şeylerden biride form elemanları, ve bunları nasıl yöneteceğimiz konusu. Gelin tek tek form elemanlarını nasıl yönetebiliriz inceleyelim.

#### Input

Tipi checkbox ya da radio olmadığı sürece herhangi bir input türü şöyle kullanılabilir:

```js
import React, { useState } from 'react';

export default function App() {
  const [name, setName] = useState('tayfun');
  const [surname, setSurname] = useState('erbilen');
  return (
    <div>
      <button onClick={() => setName('Recep')}>Adı Değiştir</button>
      <button onClick={() => setSurname('Durmuş')}>Soyadı Değiştir</button>
      <hr />
      <input
        type="text"
        defaultValue={name}
        onChange={(e) => setName(e.target.value)}
      />
      <input
        type="text"
        value={surname}
        onChange={(e) => setSurname(e.target.value)}
      />
      <hr />
      Ad = {name} <br />
      Soyad = {surname}
    </div>
  );
}
```

#### Textarea

HTML'de `<textarea>açıklama</textarea>` şeklinde tanımlansada react'de `value` prop'u ile değeri belirtilerek kullanılabilir.

```js
import React, { useState } from 'react';

export default function App() {
  const [description, setDescription] = useState('örnek description');
  return (
    <div>
      <button onClick={() => setDescription('bu bir test açıkalamadır')}>
        Açıklamayı Değiştir
      </button>
      <hr />
      <textarea
        value={description}
        onChange={(e) => setDescription(e.target.value)}
      />
      <hr />
      Açıklama = {description}
    </div>
  );
}
```

#### Select

Select'de de yine `value` ile değer belirlenir.

```js
import React, { useState } from 'react';

export default function App() {
  const [category, setCategory] = useState();
  return (
    <div>
      <button onClick={() => setCategory(3)}>Kategoriyi Değiştir</button>
      <hr />
      <select value={category} onChange={(e) => setCategory(e.target.value)}>
        <option>- Seçin -</option>
        <option value="1">PHP</option>
        <option value="2">CSS</option>
        <option value="3">HTML</option>
        <option value="4">JavaScript</option>
      </select>
      <hr />
      Seçilen Kategori = {category}
    </div>
  );
}
```

Ayrıca seçilen kategori'nin adını getirmek isteseydik şöyle de kullanabilirdik:

```js
import React, { useState } from 'react';

const categories = ['PHP', 'CSS', 'HTML', 'JavaScript'];

export default function App() {
  const [category, setCategory] = useState();
  return (
    <div>
      <button onClick={() => setCategory(3)}>Kategoriyi Değiştir</button>
      <hr />
      <select value={category} onChange={(e) => setCategory(e.target.value)}>
        <option>- Seçin -</option>
        {categories.map((category, index) => (
          <option value={index} key={index}>{category}</option>
        ))}
      </select>
      <hr />
      Seçilen Kategori = {categories[category] || 'Seçilmedi'}
    </div>
  );
}
```

Ya da obje olarak şöyle kullanabilirdik:

```js
import React, { useState } from 'react';

const categories = [
  { id: 1, name: 'PHP' },
  { id: 2, name: 'CSS' },
  { id: 3, name: 'HTML' },
  { id: 4, name: 'JavaScript' },
];

export default function App() {
  const [category, setCategory] = useState();
  return (
    <div>
      <button onClick={() => setCategory(3)}>Kategoriyi Değiştir</button>
      <hr />
      <select value={category} onChange={(e) => setCategory(e.target.value)}>
        <option>- Seçin -</option>
        {categories.map((category) => (
          <option value={category.id} key={category.id}>
            {category.name}
          </option>
        ))}
      </select>
      <hr />
      Seçilen Kategori ={' '}
      {categories[category]
        ? JSON.stringify(categories[category])
        : 'Seçilmedi'}
    </div>
  );
}
```

Multiple selectbox örneğimiz ise şöyle olurdu:

```js
import React, { useState } from 'react';

const categoryList = [
  { id: 1, name: 'PHP' },
  { id: 2, name: 'CSS' },
  { id: 3, name: 'HTML' },
  { id: 4, name: 'JavaScript' },
];

export default function App() {
  const [categories, setCategories] = useState();
  const getCategories =
    categories && categoryList.filter((c) => categories.includes(c.id));
  return (
    <div>
      <button onClick={() => setCategories([2, 3])}>
        Kategorileri Değiştir
      </button>
      <hr />
      <select
        multiple={true}
        value={categories}
        onChange={(e) =>
          setCategories(
            [...e.target.selectedOptions].map((option) => Number(option.value))
          )
        }
      >
        <option>- Seçin -</option>
        {categoryList.map((category) => (
          <option value={category.id} key={category.id}>
            {category.name}
          </option>
        ))}
      </select>
      <hr />
      Seçilen Kategori = {categories ? JSON.stringify(categories) : 'Seçilmedi'}
      <hr />
      <pre>{JSON.stringify(getCategories, null, 2)}</pre>
    </div>
  );
}
```

#### Checkbox

```js
import React, { useState } from 'react';

export default function App() {
  const [checked, setChecked] = useState(false);
  return (
    <div>
      <button onClick={() => setChecked((checked) => !checked)}>
        Checkboxı Değiştir
      </button>
      <hr />
      <label>
        <input
          type="checkbox"
          value={checked}
          checked={checked}
          onChange={(e) => setChecked(e.target.checked)}
        />
        Kuralları kabul ediyorum
      </label>
      <hr />
      Kurallar kabul edildi = {checked.toString()}
    </div>
  );
}
```

Birden fazla checkbox'ı yönetmek isteseydik:

```js
import React, { useState } from 'react';

export default function App() {
  const [rules, setRules] = useState([
    { id: 1, label: 'Kuralları kabul ediyorum', checked: false },
    { id: 2, label: 'Sözleşmeyi kabul ediyorum', checked: false },
    { id: 3, label: 'Kullanım koşullarını kabul ediyorum', checked: false },
  ]);
  const changeHandle = (e) => {
    setRules((rules) =>
      rules.map((r) => {
        if (r.id == e.target.value) {
          r.checked = e.target.checked;
        }
        return r;
      })
    );
  };
	const acceptTogggle = () => {
    setRules((rules) =>
      rules.map((rule) => ({
        ...rule,
        checked: !rule.checked,
      }))
    );
  };
	const disabled = rules.every((rule) => rule.checked);
  return (
    <div>
      <button onClick={acceptTogggle}>Tümünü Kabul Et</button>
      <hr />
      {rules.map((rule) => (
        <label style={{ display: 'block' }} key={rule.id}>
          <input
            type="checkbox"
            value={rule.id}
            checked={rule.checked}
            onChange={(e) => changeHandle(e)}
          />
          {rule.label}
        </label>
      ))}
			<hr />
			<button disabled={!disabled}>Devam et</button>
      <hr />
      <pre>{JSON.stringify(rules, null, 2)}</pre>
    </div>
  );
}
```

#### Radio

```js
import React, { useState } from 'react';

const options = [
  { option: 'Option 1', id: 1 },
  { option: 'Option 2', id: 2 },
  { option: 'Option 3', id: 3 },
];

export default function App() {
  const [selected, setSelected] = useState(false);
  const option = options.find((o) => o.id === selected);
  return (
    <div>
      <button onClick={() => setSelected(2)}>Option'ı Değiştir</button>
      <hr />
      {options.map((option) => (
        <label style={{ display: 'block' }} key={option.id}>
          <input
            type="radio"
            value={option.id}
            checked={selected === option.id}
            onChange={(e) => setSelected(option.id)}
          />
          {option.option}
        </label>
      ))}
      <hr />
      <pre>{option ? JSON.stringify(option) : 'Seçilmedi'}</pre>
    </div>
  );
}
```

#### File Input

Dosyalarla çalışırken biraz daha farklı yaklaşıyoruz:

```js
import React, { useState } from 'react';

export default function App() {
  const [file, setFile] = useState(false);
  const changeHandle = (e) => {
    setFile(e.target.files[0]);
  };
  return (
    <div>
      <label>
        <input type="file" onChange={changeHandle} />
      </label>
      {file && <div>Seçilen dosya = {file.name}</div>}
    </div>
  );
}
```

Seçilen bir görseli önizlemek için:

```js
import React, { useState } from 'react';

export default function App() {
  const [file, setFile] = useState(false);
  const changeHandle = (e) => {
    if (e.target.files[0]) {
      const fileReader = new FileReader();
      fileReader.addEventListener('load', function () {
        setFile(this.result);
      });
      fileReader.readAsDataURL(e.target.files[0]);
    } else {
      setFile(false);
    }
  };
  return (
    <div>
      <label>
        <input type="file" accept="image/*" onChange={changeHandle} />
      </label>
      {file && <img src={file} />}
    </div>
  );
}
```

Birden fazla dosya seçimi için:

```js
import React, { useState } from 'react';

export default function App() {
  const [files, setFiles] = useState(false);
  const changeHandle = (e) => {
    setFiles([...e.target.files]);
  };
  return (
    <div>
      <label>
        <input type="file" multiple={true} onChange={changeHandle} />
      </label>
      {files && files.map((file) => <div>{file.name}</div>)}
    </div>
  );
}
```

### Component Yaşam Döngüsü (Life Cycle)

Bir component'in yaşam döngüsü vardır. Bu component ilk yüklendiğinde, component render edildiğinde, component yok edildiğinde çalışacak kodları belirlememizi sağlar.

Bunun için `useEffect()` hook'u kullanılır. İlk parametre bir callback fonksiyonu, ikinci parametre ise yukarıdaki durumlara göre belirlenir.

Eğer 2. parametre olarak boş bir array verilirse, component ilk yüklendiğinde çalışması sağlanır.

```jsx
import { useEffect } from "react"

function App() {

  useEffect(() => {
    // component ilk yüklendiğinde burası çalışır
  }, [])
  
}
```

Eğer 2. parametre hiç verilmezse component her render ediliğinde çalışır.

```jsx
import { useEffect } from "react"

function App() {

  useEffect(() => {
    // component her render edildiğinde burası çalışır
  })
  
}
```

Eğer 2. parametre boş bir array verilirse ve geriye bir fonksiyon return edilirse component yok ediliğinde çalışması sağlanır.

```jsx
import { useEffect } from "react"

function App() {

  useEffect(() => {
    // component ilk yüklendiğinde burası çalışır
    return () => {
      // component yok edildiğinde burası çalışır
    }
  }, [])
  
}
```

Ayrıca state'lerde component'in bir parçası olduğu için, bunların değişimini de yine 2. parametre'de dizi içinde belirterek kullanabilirsiniz.

```jsx
import { useState, useEffect } from "react"

function Counter() {
  
  const [count, setCount] = useState(0)
  
  useEffect(() => {
    console.log('count değeri değişti', count)
  }, [count])
  
  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>Değer Artır</button>
      <button onClick={() => setCount(c => c - 1)}>Değer Azalt</button>
      <p>counter değeri = {counter}</p>
    </div>
  )
}
```

> birden fazla state'i tek bir `useEffect` de takip etmek istereseniz 2. parametreyi `[state1, state2]` şeklinde belirleyebilirsiniz.

## Fragment Kullanımı

React varsayılan olarak geriye bir JSX elemanı return etmenizi bekler. Yani birkaç tane JSX elemanını her zaman bir elemana sarmalayıp öyle döndürmenizi bekler. Ancak bazı durumlarda sarmalama işlemini bir etiketle yapmak yerine boş olarak ayarlamak isteyebilirsiniz. İşte bu gibi durumlarda `Fragment` kullanılır.

```jsx
function App() {
  return (
    <h3>Başlık</h3>
    <p>Paragraf</p>
    <button>buton örneği</button>
  )
}
```

Yukarıdaki hatalı bir kullanımdır, bu üç elemanı sarmalayıp tek bir eleman return etmeliyiz.

```jsx
function App() {
  return (
    <div>
      <h3>Başlık</h3>
      <p>Paragraf</p>
      <button>buton örneği</button>
    </div>
  )
}
```

Bu kullanım doğru olsada gereksiz yere `div` etiketi içinde yer alacaktır kodlarımız. Bunun yerine Fragment'i kullanalım birde.

```jsx
import { Fragment } from "react"

function App() {
  return (
    <Fragment>
      <h3>Başlık</h3>
      <p>Paragraf</p>
      <button>buton örneği</button>
    </Fragment>
  )
}
```

Artık gereksiz bir etiketin içinde değiller ve yine başarıyla kodlarımız çalışıyor. Ayrıca `Fragment` import etmeden `<>` ile kısa kullanımınıda kullanabiliriz.

```jsx
function App() {
  return (
    <>
      <h3>Başlık</h3>
      <p>Paragraf</p>
      <button>buton örneği</button>
    </>
  )
}
```

## craeteElement() Kullanımı

React'de JSX kullandığımızı söylemiştim. JSX ise kodları aslında birer javascript objesine çevriliyor. `createElement()` ile bir JSX objesi oluşturabiliriz. Yani örneğin şöyle bir JSX kodunu:

```jsx
<button className="btn" onClick={() => console.log('tıkladın!')}>
  Button
</button>
```

şöyle yazabilirdik:

```jsx
import { createElement } from "react"

const button = createElement('button', {
  className: 'btn',
  onClick: () => console.log('tıkladın!')
}, 'Button')
```

bir başka örnekte şöyle olabilir:

```jsx
function App() {
  const todos = ['todo 1', 'todo 2', 'todo 3']
  return (
    <ul>
      {todos.map((todo, index) => (
        <li key={index}>{todo}</li>
      ))}
    </ul>
  )
}
```

bu da şu şekilde yazılabilirdi:

```jsx
import { createElement } from "react"

function App() {
  const todos = ['todo 1', 'todo 2', 'todo 3']
  
  const ul = createElement('ul', null, todos.map((todo, index) => createElement('li', {
    key: index
  }, todo)))
  
  return ul
}
```

## useRef() Kullanımı

Bazı durumlarda react elemanlarına javascript tarafında erişip işlem yaptırmak isteyebilirsiniz. Bu gibi durumlarda o elemanı ref'lemek gerekiyor ve bunun içinde `useRef()` hook'u kullanılıyor.

Örneğin bir input'umuz olsun ve bir button'a basınca bu input'a focuslamak istediğimizi varsayalım. Input'a focuslamak için evvela input'a erişebilmemiz lazım bunun içinde `useRef()` ile refleme işlemi yapabiliriz.

```jsx
import { useRef } from "react"

function App() {
  const inputRef = useRef()
  return (
    <>
      <input type="text" ref={inputRef} />
      <button onClick={() => {
        inputRef.current.focus()
      }}>Focusla</button>
    </>
  )
}
```

Gördüğünüz gibi dom elemanına ulaşmak için `inputRef.current` şeklinde bir kullanım gerçekleştirdik.

## forwardRef() Kullanımı

Yukarıdaki senaryomuz aynı kalsaydı ve input etiketi yerine bu bir component olsaydı, bu işlemi `forwardRef()` ile ref'i ileterek yapmak zorundaydık. Yani:

```jsx
import { useRef } from "react"

const Input = forwardRef((props, ref) => {
  return <input type="text" {...props} ref={ref} />;
});

export default function App() {
  const inputRef = useRef();
  return (
    <>
      <Input ref={inputRef} />
      <button
        onClick={() => {
          inputRef.current.focus();
        }}
      >
        Focusla
      </button>
    </>
  );
}
```

## useReducer() Kullanımı

useState() ile yönetmekte zorlandığınız daha karmaşık yapılarınızda `useReducer()` kullanabilirsiniz. Örnek bir counter uygulamasından durumu açıklayalım.

```jsx
import {useState} from "react";

function Todos() {

	const [todos, setTodos] = useState([])
	const [todo, setTodo] = useState('')

	const addTodo = () => {
		setTodos([...todos, todo])
		setTodo('')
	}

	const deleteTodo = index => {
		setTodos([...todos.filter((t, i) => i !== index)])
	}

	return (
		<>
			<input type="text" value={todo} onChange={e => setTodo(e.target.value)}/>
			<button disabled={!todo} onClick={addTodo}>Ekle</button>
			<ul>
				{todos.map((todo, index) => (
					<li onClick={() => deleteTodo(index)} key={index}>{todo}</li>
				))}
			</ul>
		</>
	)
}

export default Todos
```

Yukarıdaki örnek basit bir todo örneği, şimdi bunu `useReducer()` ile yeniden düzenleyelim.

```jsx
import {useReducer} from "react";

function reducer(state, action) {
	switch (action.type) {
		case 'UPDATE_TODO':
			return {
				...state,
				todo: action.value
			}

		case 'ADD_TODO':
			return {
				...state,
				todo: '',
				todos: [...state.todos, action.todo]
			}

		case 'DELETE_TODO':
			return {
				...state,
				todos: [...state.todos.filter((t, i) => i !== action.index)]
			}
	}
}

function Todos() {

	const [state, dispatch] = useReducer(reducer, {
		todos: [],
		todo: ''
	})

	const addTodo = () => {
		dispatch({ type: 'ADD_TODO', todo: state.todo })
	}

	const deleteTodo = index => {
		dispatch({ type: 'DELETE_TODO', index })
	}

	return (
		<>
			<input type="text" value={state.todo} onChange={e => dispatch({ type: 'UPDATE_TODO', value: e.target.value })}/>
			<button disabled={!state.todo} onClick={addTodo}>Ekle</button>
			<ul>
				{state.todos.map((todo, index) => (
					<li onClick={() => deleteTodo(index)} key={index}>{todo}</li>
				))}
			</ul>
		</>
	)
}

export default Todos
```

Artık state'ler `useRecuder()` altında dönen değerlerden okunuyor. Bir işlem yaptırmak içinde `dispatch()` metoduna `type` ve gerekli değerleri geçerek tanımlıyoruz. Reducer fonksiyonunu da başka bir dosyada tutup daha düzenli bir kod yapısı oluşturabilirsiniz.

## Memoization

Memoization, bir optimizasyon tekniğidir. Primitive (ilkel) veri türlerinde 2 veriyi değerlerine göre karşılaştırmak kolaydır. Örneğin:

```js
"a" === "a" // true
true === true // true
34 === 34 // true
```

Ancak iş non-primitive (ilkel olmayan) veri türlerine geldiğinde bu sefer değerleri değil referansları karşılaştırılır. Yani şu örnekler birbirine eşit değildir:

```js
{} === {}
[] === []
```

Görünürde bir farkları olmasada bellekte tutuldukları referansları farklı olduğu için hiçbir zaman aynı olmayacaklar. İşte react'de bazı metodlar yardımıyla bu non-primitive türleri saklayıp, değişmediği taktirde bunu saklandığı yerden döndürerek tekrar bir maliyet çıkarmadan işlemimizi daha performanslı sürdürebiliriz.

### memo() Kullanımı

Component'lerin gereksiz yere render edilmemesi için kullanılır. Örneğin bir component'in içinde başka bir component'i çağırdık. Eğer üst component'imiz render olursa, çağırdığımız çocuk component'de render olacaktır. En sık göreceğiniz örneği ise şöyledir:

```js
// ./components/Header.js
function Header({ text }) {
	console.log('header component render edildi')
	return (
		<header>{text}</header>
	)
}

export default Header
```

```js
import { useState } from "react"
import Header from "./components/Header"

function App() {
	
	console.log('App component render edildi')
	const [count, setCount] = useState(0)
	const [text, setText] = useState('header')
	
	return (
		<>
			<Header text={text} />
			<h3>{count}</h3>
			<button onClick={() => setCount(c => c + 1)}>Artır</button>
			<button onClick={() => setText('yeni header yazısı')}>Header Yazısını Değiştir</button>
		</>
	)
	
}
```

Yukarıdaki örnekte Artır butonuna her bastığımızda konsol'da `header component render edildi'` mesajını göreceğiz. İşte bunu önlemek için `Header` component'ini export ederken `memo()` içinde export edebilirdik.

Yani şunun yerine:

```js
export default Header
```

şu şekilde kullanmalıydık:

```js
import { memo } from "react"

// ..

export default memo(Header)
```

Arıtk `App` component'i render olsa bile `Header` component'i sadece ona geçilen prop'lar değişirse yeniden render olacaktır. Bunu anlamak için header yazını değiştirme butonuna basıp test edebilirsiniz. 2. kez basıldığında state öncekiyle aynı olduğu için component yine render edilmeyecektir.

Ayrıca iç içe kullanımlarda da bu tekniği kullanmak faydalı olacaktır. Bir todo örneğinden yola çıkacak olursak:

```js
// ./AddTodo.js
import { useState, memo } from 'react';

function AddTodo({ setTodos }) {
  console.log('add todo');
  const [todo, setTodo] = useState('');
  const submitHandle = (e) => {
    e.preventDefault();
    setTodos((todos) => [...todos, todo]);
    setTodo('');
  };
  return (
    <form onSubmit={submitHandle}>
      <input
        type="text"
        value={todo}
        onChange={(e) => setTodo(e.target.value)}
      />
      <button disabled={!todo} type="submit">
        Ekle
      </button>
    </form>
  );
}

export default memo(AddTodo);
```

```js
// ./TodoList.js
import { memo } from 'react';
import TodoItem from './TodoItem';

function TodoList({ todos }) {
  console.log('todo list');
  return (
    <ul>
      {todos.map((todo, index) => (
        <TodoItem todo={todo} key={index} />
      ))}
    </ul>
  );
}

export default memo(TodoList);
```

```js
// ./TodoItem.js
import { memo } from 'react';

function TodoItem({ todo }) {
  console.log('todo item', todo);
  return <li>{todo}</li>;
}

export default memo(TodoItem);
```

```js
import { useState } from 'react';

import AddTodo from './AddTodo';
import TodoList from './TodoList';

export default function App() {
  const [todos, setTodos] = useState([]);
  const [count, setCount] = useState(0);
  return (
    <>
      <h3>{count}</h3>
      <button onClick={() => setCount((c) => c + 1)}>Artır</button>
      <AddTodo setTodos={setTodos} />
      <TodoList todos={todos} />
    </>
  );
}
```

Bu örnekte `memo()` metodunu kaldırarak konsol'dan neler olduğunu inceleyip durumu daha iyi anlayabilirsiniz :)

### useMemo() Kullanımı

Component render olduğunda çocuk component'e prop olarak geçilen değerler non-primite (ilkel olmayan) değerler ise çocuk component'de yeniden render olacak. Yukarıdaki örneğimizde `todos` değeri yerine `filteredTodos` gibi bir değeri prop olarak geçiyor olsaydık, yine `count` artırınca `App` render olacağı için ve `filteredTodos` artık eski referansına sahip olmayacağı için prop değişmiş gibi görünecek ve çocuk component'de render olacaktır. İşte bu gibi durumlarda bunu `useMemo()` ile optimize edebiliriz. Böylece component render olsa bile refransı değişmez, sadece gerçekten bir değişiklik olduğunda çocuk component'i render eder.

Örnek olarak yukarıdaki `App` component'ine bir search input'u ekleyip todo'ları filtreleme işlemi yapalım. Ve `TodoList` e prop olarak `todos` değilde `filteredTodos` propunu geçelim.

```js
import { useState } from 'react';

import AddTodo from './AddTodo';
import TodoList from './TodoList';

export default function App() {
  const [todos, setTodos] = useState([]);
  const [count, setCount] = useState(0);
	const [search, setSearch] = useState('')
	
	const filteredTodos = todos.filter(todo => todo.toLocaleLowerCase().includes(search.toLocaleLowerCase()))
	
  return (
    <>
      <h3>{count}</h3>
      <button onClick={() => setCount((c) => c + 1)}>Artır</button>
			<hr />
			<input type="text" placeholder="Todolarda ara" value={search} onChange={e => setSearch(e.target.value)} />
			<hr />
      <AddTodo setTodos={setTodos} />
      <TodoList todos={filteredTodos} />
    </>
  );
}
```

Şimdi burada eğer `Artır` butonuna basarsak `TodoList` in `memo()` kullanmamıza rağmen render olduğunu görüyoruz. Yukarıda açıkladığım sebeplerden dolayı, bu yüzden `filteredTodos` değerini `useMemo()` ile kullanmamız lazım. Yani:

```js
import { useState, useMemo } from 'react';

import AddTodo from './AddTodo';
import TodoList from './TodoList';

export default function App() {
  const [todos, setTodos] = useState([]);
  const [count, setCount] = useState(0);
	const [search, setSearch] = useState('')
	
	const filteredTodos = useMemo(() => {
		return todos.filter(todo => todo.toLocaleLowerCase().includes(search.toLocaleLowerCase()))
	}, [todos, search])
	
  return (
    <>
      <h3>{count}</h3>
      <button onClick={() => setCount((c) => c + 1)}>Artır</button>
			<hr />
			<input type="text" placeholder="Todolarda ara" value={search} onChange={e => setSearch(e.target.value)} />
			<hr />
      <AddTodo setTodos={setTodos} />
      <TodoList todos={filteredTodos} />
    </>
  );
}
```

`useMemo()` ilk parametresi callback fonksiyonu 2. parametresi ise bağımlılıkları yani hangi değerler değiştiğinde yeniden hesaplanması gerektiği 2. parametrede dizi olarak belirleniyor. Artık tekrar aynı işlemi denerseniz render olmadığını göreceksiniz.

### useCallback() Kullanımı

Yine component'e prop olarak geçilen metodların üst component render olduğunda yeniden hesaplanıp prop geçilen component'i de render etmemesi için metodlarda `useCallback()` kullanılır. Örneğin yukarıdaki örneğimizin devamı olarak aranan kelimeyi silen bir butonu component olarak hazırlayalım ve tıklanınca çalışacak metodu `App` component'inden prop olarak geçelim.

```js
// ./ClearButton.js
import { memo } from "react"

function ClearButton({ onClick }) {
	console.log('clear button rendered')
	return <button onClick={onClick}>Temizle</button>
}

export default memo(ClearButton)
```

```js
import { useState, useMemo, useCallback } from 'react';

import AddTodo from './AddTodo';
import TodoList from './TodoList';
import ClearButton from './ClearButton';

export default function App() {
 	const [todos, setTodos] = useState([]);
 	const [count, setCount] = useState(0);
	const [search, setSearch] = useState('')
	
	const filteredTodos = useMemo(() => {
		return todos.filter(todo => todo.toLocaleLowerCase().includes(search.toLocaleLowerCase()))
	}, [todos, search])
	
	condt clearSearch = useCallback(() => {
		setSearch('')
	})
	
	return (
		<>
			<h3>{count}</h3>
			<button onClick={() => setCount((c) => c + 1)}>Artır</button>
			<hr />
			<input type="text" placeholder="Todolarda ara" value={search} onChange={e => setSearch(e.target.value)} />
			<ClearButton onClick={clearSearch} />
			<hr />
			<AddTodo setTodos={setTodos} />
			<TodoList todos={filteredTodos} />
		</>
	);
}
```

## Context Yapısı

Tipik bir react uygulamasında state'ler üst component'den alt component'lere iletilerek kullanılır. Fakat bu tarz bir kullanım, bire süre sonra component ve state'lerin artmasıyla büyük bir sorun haline gelir.

İşte bu gibi durumlar için state'leri global yönetmek adına react'in bize sağladığı `context` yapısını kullanabiliriz.

> Global state yönetim araçları elbette var, ancak bu react'in kendisinde bulunan bir yapı olduğu için bilmekte fayda var.

Context yapısını genelde kullanıcı işlemlerinde, tema ve dil gibi global işlemler için kullanabiliriz.

### Kullanımı

`createContext()` ile kullanmak üzere bir context yapısı oluşturulur. Örneğin sitenin dilini ve temasını tutan bir context yapısı oluşturalım ve bunu `context/` klasörü içinde `SiteContext.js` adıyla oluşturalım.


```js
// ./context/SiteContext.js
import { createContext } from "react"

const SiteContext = createContext()

export default SiteContext
```

Context yapısını kullanırken diğer tüm kodlarımızı kapsayacak şekilde sarmalamak gerekir.

```js
import Header from "./components/Header"
import SiteContext from "./context/SiteContext"

function App() {

  const data = {
    theme: 'light',
    language: 'tr'
  }

  return (
    <SiteContext.Provider value={data}>
      <Header />
    </SiteContext.Provider>
  )
}
```

`SiteContext.Provider` dedikten sonra `value` prop'una paylaşılacak değerler geçilir. Ve örneğin `Header` component'inde buna erişmek istediğimizde:

```js
// ./components/Header.js
import { useContext } from "react"
import SiteContext from "../context/SiteContext"

export default function Header() {

  const { theme, language } = useContext(SiteContext)

  return (
    <header>
      Site dili: {language} <br />
      Site teması: {theme}
    </header>
  )
}
```

`useContext()` içine `SiteContext` belirlenir ve objeden destructuring yardımı ile değerler çıkartılıp kullanılır. Artık component'e prop geçmeden de global olarak değerlerimizi çekmeyi başardık. Ancak hala bir eksik var, Header component'inden bu değerleri değiştiremiyoruz. O zaman `App` componentimizde değerleri `useState()` ile oluşturup bir de öyle deneyelim.

```js
import { useState } from "react"
import Header from "./components/Header"
import SiteContext from "./context/SiteContext"

function App() {

  const [theme, setTheme] = useState('light')
  const [language, setLanguage] = useState('tr')

  const data = {
    theme,
    setTheme,
    language,
    setLanguage
  }

  return (
    <SiteContext.Provider value={data}>
      <Header />
    </SiteContext.Provider>
  )
}
```

Artık state'i ve onu güncellemek için gerekli fonksiyonlarda global olarak paylaştığım datanın içerisinde. Şimdi `Header` component'ini şöyle güncelleyebiliriz.

```js
// ./components/Header.js
import { useContext } from "react"
import SiteContext from "../context/SiteContext"

export default function Header() {

  const { theme, setTheme, language, setLanguage } = useContext(SiteContext)

  return (
    <header>
      Site dili: {language} <br />
      <button onClick={() => setLanguage('en')}>Dili Değiştir</button>
      
      <hr />
      Site teması: {theme} <br />
      <button onClick={() => setTheme(theme => theme === 'light' ? 'dark' : 'light')}>Dili Değiştir</button>
    </header>
  )
}
```

Artık header component'inden değişikliği yaptık, aynı yapıyı kullanarak diğer bütün component'lerde bu değere erişip kullanabiliriz ve bir değişiklikte bunlar otomatik olarak güncellenir.

### Daha İyi Kullanımı

Böyle çok karmaşık gibi gelmedi mi? Context'i biraz daha tek dosyada tutup mümkün olduğunca basitleştirmeye çalışalım. İlk olarak context yapımızı şöyle değiştireceğiz:

```js
// ./context/SiteContext.js
import { createContext, useState } from "react"

const SiteContext = createContext()
const SiteProvider = ({ children }) => {
  
  const [theme, setTheme] = useState('light')
  const [language, setLanguage] = useState('en')
  
  const data = {
    theme,
    setTheme,
    language,
    setLanguage
  }
  
  return (
    <SiteContext.Provider value={data}>
      {children}
    </SiteContext.Provider>
  )
}

export const useSite = () => useContext(SiteContext)
export default SiteProvider
```

Artık `App` componentimiz şöyle olacak:

```js
import { useState } from "react"
import Header from "./components/Header"
import SiteProvider from "./context/SiteContext"

function App() {
  return (
    <SiteProvider>
      <Header />
    </SiteProvider>
  )
}
```

Ve `Header` componentimizde şöyle kullanabileceğiz:

```js
// ./components/Header.js
import { useSite } from "../context/SiteContext"

export default function Header() {

  const { theme, setTheme, language, setLanguage } = useSite()

  return (
    <header>
      Site dili: {language} <br />
      <button onClick={() => setLanguage('en')}>Dili Değiştir</button>
      
      <hr />
      Site teması: {theme} <br />
      <button onClick={() => setTheme(theme => theme === 'light' ? 'dark' : 'light')}>Dili Değiştir</button>
    </header>
  )
}
```

Yan etkileri de context dosyamız içinde kolayca yönetebiliriz. Örneğin temayı ve dili değiştirdiğimizde bunları localStorage'da tutalım, sayfayı yenilediğimizde en son belirlediğimiz tema ve dil hangisi ise onlar kalmaya devam etsin.


```js
// ./context/SiteContext.js
import { createContext, useState, useEffect } from "react"

const SiteContext = createContext()
const SiteProvider = ({ children }) => {
  
  const [theme, setTheme] = useState(localStorage.getItem('theme') || 'light')
  const [language, setLanguage] = useState(localStorage.getItem('language') || 'en')
  
  const data = {
    theme,
    setTheme,
    language,
    setLanguage
  }
  
  useEffect(() => {
    localStorage.setItem('theme', theme)
    localStorage.setItem('language', language)
  }, [theme, language])
  
  return (
    <SiteContext.Provider value={data}>
      {children}
    </SiteContext.Provider>
  )
}

export const useSite = () => useContext(SiteContext)
export default SiteProvider
```

state'lerin başlangıç değerlerini eğer varsa `localStorage` nesnesinden okuduk. Ve `useEffect` ile `theme` ya da `language` değiştiğinde bunu localStorage'a kaydettik. Böylece bir sonraki yenileyişte değişiklikleri hatırlayacaktır.

Ayrıca birden fazla context yapımız olduğunda bunları her seferinde `App` de çağırmayalım ya da state'leri kullanmak istediğimizde farklı dosyalardan çekmeyeli onun yerine `context` klasörüne bir `index.js` dosyası oluşturup tümünü burada yapalım.

```js
import SiteProvider, { useSite } from "./SiteContext"

export default function Provider({ children }) {
  return <SiteProvider>{children}</SiteProvider>
}

export {
  useSite
}
```

Artık App'de kullanırken:

```js
import { useState } from "react"
import Header from "./components/Header"
import Provider from "./context"

function App() {
  return (
    <Provider>
      <Header />
    </Provider>
  )
}
```

Ve `Header` componentinde erişirken:

```js
// ./components/Header.js
import { useSite } from "../context"

// diğer kodlar...
```

Böylece bir başka context yapımız olduğunda bunu `context/index.js` de çağırıp kullanıcaz ve istediğimiz her yerde onun custom hook'u ile datalarına erişip müdehale edebileceğiz.
