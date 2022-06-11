# 3. Gün

## React Router

Routing yapısı için `react-router` paketini kullanacağız. Böylece sayfalarımızı kolayca yöneteceğiz ve paketin verdiği avantajları kullanarak gücümüze güç katacağız :)

### Kurulum

Kurmak için:

```shell
npm install react-router-dom@6
```

İlk olarak `index.js` de `BrowserRouter` component'ini çağırıyoruz.

```js
import { BrowserRouter } from "react-router-dom";
```

Ve `App` component'ini bunun içinde render ediyoruz.

```js
<BrowserRouter>
  <App />
</BrowserRouter>
```

Ve `App` component'i içinde `Routes`, `Route` ve `Link` component'lerini çağırıyoruz. Ve routing işlemlerini belirliyoruz.

```js
import { Routes, Route, Link } from "react-router-dom"
import Home from "./pages/Home";
import Contact from "./pages/Contact";

function App() {
  return (
    <>
      <nav>
        <Link to="/">Anasayfa</Link>
        <Link to="/contact">İletişim</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="contact" element={<Contact />} />
      </Routes>
    </>
  );
}

export default App;
```

`pages` klasörüne `Home` ve `Contact` adında 2 component oluşturup içlerini basitçe doldurabiliriz.

Burada `a` etiketi ile de linklendirme yapabiliriz ancak `Link` componenti ile sayfa yenilenmeden bu işlemleri daha kolayca yönetebiliriz.


DEVAM EDECEK....
