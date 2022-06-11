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

### Nested ve Dinamik Route

İç içe route tanımlamak istediğinizde `Route` component'ini self-closing yerine açıp kapatarak içine yine tanımlarınızı yapabilirsiniz. Ayrıca dinamik route'da kolayca belirlenebilir. Örneğin blog sabit bir route'ken blog post'u her zaman değişeceği için dinamik bir route'dur. Örneğin:

```js
<Route path="blog" element={<Blog />}>
  <Route path="konu/:url" element={<BlogPost />} />
</Route>
```

Artık `/blog` ve `/blog/konu/url-ne-olacaksa` şeklinde erişebilirsiniz. Elbette öncesinde `Blog` ve `BlogPost` componentlerini oluşturmayı unutmayın :)

### index Route - Outlet

Nested route'larda örneğin yukarıdaki örnekte `blog` route'u aslında bizim layout sayfamız olmalı. Ve diğer sayfalar bir şekilde bunun içinde gösterilmeli. Bunun için `<Outlet />` componenti kullanılmalı. Yani `Blog` component'i şu şekilde olabilir:

```js
import { Outlet } from "react-router-dom"

export default function Blog() {
  return (
    <>
      <h1>Blog</h1>
      <nav>
        .. global menü blog sayfaına özel ..
      </nav>
      <Outlet />
    </>
  )
}
```

Artık alt componentler Outlet'in olduğu yerde render edilecek. Böylece blog'da bütün sayfalarda görünecek kodları burada yazabilirsiniz. Ancak tek bir sorun var `/blog` yazınca layout olduğu için Outlet'i dolduramayacağız, işte bu gibi durumlar için route yapısını şöyle değiştirmeliydik:

```js
<Route path="blog" element={<Blog />}>
  <Route index element={<BlogPage />} />
  <Route path="konu/:url" element={<BlogPost />} />
</Route>
```

Artık `BlogPage` component'i `/blog` adresine girince render edilecek :)

### `useParams()` ile Parametrelere Erişmek

Yukarıda `konu/:url` ile dinamik bir route belirledik. Buradaki `url` değerini `useParams()` ile alıp kullanabilirdik.

```js
import { useParams } from "react-router-dom";

export default function PostDetail() {

	const { url } = useParams()

	return (
		<>
			<h1>Blog Post Detay</h1>
			<p>URL = {url}</p>
		</>
	)
}
```

> Fark edeceğiniz üzere iki noktadan sonra ne yazarsanız onu `useParams()` içinden alıp kullanabilirsiniz.

### "404 - Sayfa Bulunamadı"

Oluşturduğunuz router'larla eşleşmeyen bir sayfaya girildiğinde 404 sayfası göstermek için `path` değerine `*` yazmanız yeterli. Nested route'larda da kullanılabilir.


```js
<Routes>
	<Route path="/" element={<Home />} />
	<Route path="contact" element={<Contact />} />
	<Route path="blog" element={<Blog />}>
		<Route index element={<BlogPage />} />
		<Route path="konu/:url" element={<BlogPost />} />
		<Route path="*" element={<BlogPageNotFound />} />
	</Route>
	<Route path="*" element={<PageNotFound />} />
</Routes>
```

Artık şu adresler dışında nereye girilirse girilsin 404 sayfası gözükecektir.

```
/
/contact
/blog
/blog/konu/test-konu
```

