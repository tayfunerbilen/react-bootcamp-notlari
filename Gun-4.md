# 4. Gün

## Redux Toolkit ile Global State Yönetimi

Global state yönetimi için kullanabileceğimiz bir araçtır. Toolkit ile işleri çok daha kolay bir şekilde yönetebiliriz.

### Kurulum

İLk olarak paketimiz şöyle kuralım:

```shell
npm install @reduxjs/toolkit react-redux
```

### Kullanımı

İlk olarak bir store dosyası oluşturmamız gerekiyor. Bunun için `store/` klasöründe örnek bir store dosyası oluşturalım. Bu dosya global değerlerimizi tutacak. Örneğin site teması ve dili için bir store dosyası tanımlayabiliriz.

`store/site.js` adında bir dosya oluşturalım ve şu kodları yazalım:

```js
import { createSlice } from '@reduxjs/toolkit'

const initialState = {
  theme: 'light',
  language: 'tr'
}

export const site = createSlice({
  name: 'site',
  initialState,
  reducers: {
    setTheme: (state, action) => {
      state.theme = action.payload
    },
    setLanguage: (state, action) => {
      state.language = action.payload
    }
  },
})

export const { setTheme, setLanguage } = site.actions
export default site.reducer
```

Ve `store/index.js` dosyası oluşturup ilgili store'larımızı şöyle dahil edelim:

```js
import { configureStore } from '@reduxjs/toolkit'
import site from "./site"

const store = configureStore({
  reducer: {
    site
  },
})

export default store
```

Redux değerlerini provide edebilmek için yine en üst component'de ya da `index.js` de şu işlemleri yapalım:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import store from './store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

Artık component'lerde bu global değerlere erişip kullanmaya çalışalım:

```js
import { useSelector, useDispatch } from "react-redux"
import { setTheme, setLanguage } from "./store/site"

export default function Header() {

  const { theme, language } = useSelector(state => state.site)
  const dispatch = useDispatch()
  
  const switchTheme = () => {
    dispatch(setTheme(theme === 'light' ? 'dark' : 'light'))
  }
  
  const switchLanguage = () => {
    dispatch(setLanguage(language === 'tr' ? 'en' : 'tr'))
  }

  return (
    <>
      <h1>Header</h1>
      Tema = {theme} <br />
      <button onClick={switchTheme}>Temayı değiştir</button>
      <hr />
      Dil = {language} <br />
      <button onClick={switchLanguage}>Dili değiştir</button>
    </>
  )
}
```

Gördüğünüz gibi yapı Context API + reducer kullanımına çok benziyor. Yine bir `dispatch()` metodumuz var ve `site.js` içinde export ettiğimiz reducer'ları çalıştırmamızı sağlıyor.

### Component Dışında State'lere Erişmek ve Kullanmak

Bazen component dışında da state'lere erişmek ya da state'lerin değerlerini değiştirmek isteriz. Örneğin sorguları yönettiğimiz js dosyamızda state'lerden bir değer almamız icap edebilir. Bu gibi durumlarda `store` dosyasını çağırıyoruz.

```js
import store from "./store"

store.getState() // stateleri öner
store.dispatch(..) // bir reducer çalıştırmamızı sağlar
```

## React Query ile İstekleri Global Olarak Yönetmek

Çoğu zaman istekleri yönetmek istediğimizde zorlanabiliriz. Örneğin post'ları çektiğimiz bir sayfamız olsun, birkaç state tanımlayarak başlarız:

```js
const [loading, setLoading] = useState(false)
const [posts, setPosts] = useState(false)
```

Daha sonra `useEffect()` ile component ilk kez yüklendiğinde gidip isteği atarız, ona göre durumları belirleriz.

```js
useEffect(() => {
  setLoading(true)
  fetchPosts()
    .then(res => setPosts(res))
}, [])
```

Ancak bu süreç biraz yorucu olabilir, query'leri yani sorgularınızı yönetmek için react query kullanmak projelerde işinizi çok kolaylaştıracaktır.

### Kurulum

Projemize react-query paketini kurarak başlayalım:

```shell
npm i react-query
```

### Kullanım

İlk olarak `index.js` de provider'ı sarmalamak gerekiyor.

```js
import { QueryClient, QueryClientProvider } from "react-query"

const queryClient = new QueryClient()

render(
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider>
)
```

### Sorgular

Artık sorgularımızı `useQuery()` hook'u ile kolayca yönetebiliriz:

```js
const info = useQuery('todos', fetchTodoList)
```

> Burada `fetchTodoList` bir Promise döndüren metod olmalı. Yani sorgular mutlaka promise dönen bir metod olarak tanımlanmalı.

Burada `useQuery` bize bir obje dönüyor.

- isLoading - Yüklenme aşamasında true olarak döner
- isError - Denemeler sonucu bir hata olursa true döner
- isSuccess - İşlem başarılıysa true döner
- isIdle - Sorgu disable ise true döner
- error - `isError` true olduğunda hataları döner
- data - `isSuccess` true olduğunda verileri döner
- isFetching - Bir veri çekiliyorsa (arkaplanda çekilmesi dahil) true döner

Buna bağlı olarak kodumuzu şöyle düzenleyebilirdik:

```js
import { useQuery } from "react-query"

function TodoApp() {
  const { isLoading, isError, error, data } = useQuery('todos', fetchTodoList)
  
  if (isLoading) return 'Yükleniyor..'
  if (isError) {
    return <pre>{JSON.stringify(error, null, 2)}</pre>
  }
  
  return <pre>{JSON.stringify(data, null, 2)}</pre>
  
}
```

### Sorgu Key'leri

Her sorgu bir isimle tutulur. Yukarıdaki örneğimizde ismi `todos` idi. Ve her sorgu key'ler alabilir, bunu şöyle düşünün sayfa değiştiğinde sorgunun yeniden tetiklenmesini istersiniz değil mi? İşte bu gibi durumlarda sorgu key'lerini kullanacaksınız. Bu bir string olabilir, obje olabilir, array olabilir, canınız ne isterse ekleyin :)

```js
useQuery('todos') // ['todos']
useQuery(['todo', page]) // ['todos', 2]
useQuery(['todo', {
  page: 1
}]) // ['todos', {page: 1}]
```

> Not: Eğer sorgularınızda bu state'lere bağlı olarak değişecekse method'da bunları belirtmeyi unutmayın.

### Sorgu Fonksiyon Değişkenleri

`useQuery()` metoduna 2. parametre olarak sorgu metodumuzu ekliyoruz. Ve dilersek bu sorgu metodumuz içerisinde sorgu key'lerine erişebiliriz.

```js
function fetchTodoList({.queryKey }) {
  console.log(queryKey)
}

...

useQuery(['todos', { page: 2 }], fetchTodoList)
```

### `enabled` ile Sorgu Bekletme

Bazen sorgumuzu yazmak ama çalıştırma eylemi için bir aksiyon olarak bunu gerçekleştirmek isteyebiliriz. Örneğin sorgumzu hazırlarız, ancak bir buton'a basıldığında çalışmasını isteyebiliriz. İşte bu gibi durumlar için bekleme modunda kalması adına `enabled` değerini kullanabiliriz.

```js
useQuery('todos', getTodos, {
  enabled: false
})
```

Buradaki `false` değeri bir state'e bağlı olursa ve butona basınca bu state true olarak güncellenirse sorgumuz çalışacaktır.

Ayrıca `useQuery` altında gelen `refetch` ile de yeniden sorgulama işlemini yapabilirsiniz.

### `refetchOnWindowFocus` ile Sorgu Tazeleme

Bazen kullanıcı tarayıcıda başka bir taba geçtiğinde ya da focus'unu kaybettiğinde ve siteye geri döndüğünde mevcut sorguların yeniden çalıştıırlıp güncel veriyi almasını isteyebilirsiniz. React Query zaten böyle çalışıyor :D Ama bazen de bunu istemeyebilirsiniz, işte `refetchOnWindowFocus` ile bunu belirleyebilirsiniz.

```js
useQuery('todos', getTodos, {
  refetchOnWindowFocus: false
})
```

Focus işleminde artık yeniden çekmeye çalışmayacaktır.

### `retry` ile Hata Denemesi

Bir istek hata vermeden önce kaç defa yeniden denenmeli bunu bu değer ile belirtebilirsiniz.

```js
useQuery('todos', getTodos, {
  retry: 10
})
```

10 kere denedikten sonra hala hata veriyorsa o zaman hatayı döndürecektir.

### `keepPreviousData` ile Önceki Veriyi Tutmak

Bazen özellikle sayfalama işlemlerinde 2. sayfaya geçerken 1. sayfanın verisi kaybolur. Bu süreci eski veri kaybolmadan yapmak ve yeni veri geldiğinde eskisiyle değiştirilmesini isterseniz bu değeri kullanabilirsiniz.

```js
useQuery('todos', getTodos, {
  keepPreviousData: true
})
```

### `invalidateQueries` ile Sorguları Geçersiz Kılmak

Sorgularınız artık güncelliğini yitirmişse örneğin yeni bir veri eklediniz ve son halini çektirmek istiyorsunuz, mevcut sorgunuzu invalidate ederek bu işlemi sağlayabilirsiniz.

> Bu işlemi daha sonra mutate ile yapmayı öğreneceğiz.

```js
queryClient.invalidateQueries('todos')
queryClient.invalidateQueries(['todos', { page: 1 }])
```

### Mevcut Dataya Yenisini Eklemek

Örneğin `todos` ismiyle istek atıp değerleri çekiyoruz. Peki yeni bir todo eklemek istediğimizde, bu değeri nasıl güncelleyeceğiz?

Ya yukarıda `invalidateQueries()` ile isteği geçersiz kılarak yenisini çektireceğiz ki bu saçma olur, ya da şöyle yapacağız:

```js
import { useQueryClient } from "react-query"

const queryClient = useQueryClient()

const submitHandle = async () => {
  const todo = await newTodo({
    title: 'todo 1',
    done: false
  })
  
  // mevcut todoları getir
  const previousTodos = queryClient.getQueryData('todos')
  
  // Yeni todo ile birlikte güncelle
  queryClient.setQueryData('todos', old => [todo, ...old])
  
}
```

### Mutation

Sorguların aksine mutation'lar oluşturma/güncelleme/silme işlemlerini yönetmek için kullanılır. Bu amaç içinde, `useMutation()` hooku kullanılır. Örnek bir kullanım şöyledir:

```js
import { useMutation } from "react-query"

export default function NewTodo() {
  
  const mutation = useMutation(newTodo)
  
  return (
    <>
      {mutation.isLoading && 'Ekleniyor..'}
      {mutation.isError && 'Bir hata oluştu'}
      {mutation.isSuccess && 'Todo başarıyla eklendi'}
      
      <button onClick={() => {
        mutation.mutate({
          title: 'todo 1',
          done: false
        })
      }}>
        Todo Ekle
      </button>
    </>
  )
  
}
```

Ayrıca `mutate` metoduna 2. parametre olarak bir obje verilip `onSuccess`, `onError` gibi durumlarda mevcut data güncellemesi yapılabilirdi.

> Bunun için bir önceki örneği ele alıp test edebilirsiniz.

### `broadcastQueryClient` ile Tab/Pencere Arası Senkronizasyon

Henüz deneysel bir çalışma olsada 2 farklı tabı senkron etmek istediğinizde bu eklentiyi kullanabilirsiniz. Ekstra olarak yüklemenize gerek yok, react-query ile birlikte geliyor. `index.js` e şu şekilde çağırıp dahil edebilirsiniz:

```js
import { broadcastQueryClient } from 'react-query/broadcastQueryClient-experimental'

const queryClient = new QueryClient()

broadcastQueryClient({
  queryClient,
  broadcastChannel: 'my-app',
})
```

Artık 2 farklı tab react-query ile bir işlem yapıldığında senkron hale gelecek, denemesi bedava :)

## React i18next ile Dil Sistemi

React ile proje geliştirirken, dil sistemine de mutlaka ihtiyaç duyacağız. Bu gibi durumlarda bu işin altından hakkıyla kalkmak için `react-i18n` paketine ihtiyacımız olacak.

### Kurulum

Kurmak için:

```shell
npm i react-i18next i18next
```

### Kullanım

İlk olarak `i18n.js` dosyası oluşturup şu şekilde kodlarımızı yazalım.

```js
import i18n from "i18next";
import {initReactI18next} from 'react-i18next';
```

Ve hemen altında ufak bir çeviri ekleyelim ingilizce ve türkçe için.

```js
const resources = {
	en: {
		translation: {
			welcome: 'Welcome to React'
		}
	},
	tr: {
		translation: {
			welcome: 'React\'e Hoşgeldin'
		}
	}
}
```

Ve ayarlarımızı başlatalım.

```js
i18n
	.use(initReactI18next)
	.init({
		resources,
		lng: 'en'
	})
  
export default i18n
```

`i18n.js` dosyasını `index.js` e çağırdıktan sonra hazırız.

Artık component'ler içinde kullanmaya hazırız. Örneğin `App` componentimizde şu şekilde kullanabiliriz:

```js
import { useTranslation } from "react-i18next";

function App() {
  
  const { t, i18n } = useTranslation()

  return (
    <>
      <h1>{t('welcome')}</h1>
      <button onClick={() => i18n.changeLanguage(i18n.language === 'en' ? 'tr' : 'en')}>Dili Değiştir</h1>
    </>
  )
}
```

### Dil Dosyalarını Ayırmak

Inline olarak dil kodları tanımlamak elbette çok mantıklı değil, `public/locales` altında dil dosyalarını ayrı ayrı tutmak isteyebiliriz. Bunun için şu paketi kuracağız:

```shell
npm i i18next-http-backend
```

Ve setup'a şöyle dahil edeceğiz:

```js
import Backend from 'i18next-http-backend';

i18n
  .use(Backend)
	.use(initReactI18next)
	.init({
		lng: 'en'
	})
```

Ve örnek dil dosyalarımız tr ve en dilinde olacaksa dosya yapısı şöyle olmalı:

- public/locales/en/translation.json
- public/locales/tr/translation.json

Örnek `translation.json` dosyası ise şöyle olmalı:

```json
{
  "welcome": "Welcome to React"
}
```

### Varsayılan Tarayıcı Dilini Tespit Etmek

Varsayılan tarayıcı diline göre dil dosyası yükletmek için şu paketi kurmamız lazım:

```shell
npm i i18next-browser-languagedetector
```

Ve setup'ımız şöyle olmalı:

```js
import LanguageDetector from 'i18next-browser-languagedetector';

i18n
  .use(Backend)
	.use(LanguageDetector)
	.use(initReactI18next)
	.init({
		fallbackLng: 'en'
	})
```

Artık tarayıcının dil dosyası ne ise onu kullanmaya çalışacak, eğer ilgili dile ait bir dosya yoksa `fallbackLng` altındaki mevcut dil dosyasını gösterecektir.

### Çevirilere React Componentleri Dışında Erişmek

Böyle bir ihtiyaç halinde `i18n.js` dosyasını import edip içindeki `t` metodunu kullanabilirsiniz:

```js
import i18n from "./i18n"

console.log(i18n.t('welcome'))
```
