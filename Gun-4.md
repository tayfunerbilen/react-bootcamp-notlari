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

