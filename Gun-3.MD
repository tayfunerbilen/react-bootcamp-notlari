# 3. Gün

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
