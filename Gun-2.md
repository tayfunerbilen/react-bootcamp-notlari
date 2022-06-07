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
node_modules/
public/
  favicon.ico
  index.html
src/
  index.js
  App.js
.gitignore
package.json
package-lock.json
README.md
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

Bir koşula bağlı olarak bazı alanları yüklemek istediğinizde `&&` operatörünü kullanabilirsiniz. Örneğin bir buton'a bastığımızda state'in değerini `true` yapalım ve eğer bu değer `true` ise bir başka JSX elemanı render edelim.

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

Bunu neden istiyor? Performans sebeplerinden dolayı. Şöyle düşünün. Aşağıdaki gibi bir markupımız olsun.

```
<li>Todo 1</li>
<li>Todo 2</li>
```

Biz yukarıdaki todo örneğinde yeni bir todo eklediğimizde yukarıdaki markupumuz şöyle olacak:

```
<li>Todo 1</li>
<li>Todo 2</li>
<li>Todo 3</li>
```

React'in diffing algoritması ile bunu yönetmesi çokta zor değil. Ancak yeni bir todo değerini başa ekleseydim, yani şöyle olsaydı:

```
<li>Todo 4</li>
<li>Todo 1</li>
<li>Todo 2</li>
<li>Todo 3</li>
```

Burada ilk elemanın `Todo 1` olması beklenirken `Todo 4` olmasıyla diğerleri de yeniden hesaplanmak zorunda kalırdı. İşte bu noktada react bize tekrarlanan elemanlar için benzersiz bir `key` değeri girmemizi istiyor. Böylece react hangisinin değiştiğini, güncellendiğini, silindiğini vs. çok daha efektik bir şekilde takip edecek ve perforamns kaybı yaşatmayacak.

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

Genelde `map()` metodu 2. parametre olarak index değeri döndüğü için bu kullanılabilir. Ya da benzersiz olan herhangi bir şey tercih edilebilir.

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
  }, [])
  
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
