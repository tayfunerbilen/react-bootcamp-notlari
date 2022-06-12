# 3. Gün

## Fetch API

Ek bir paket kullanmadan HTTP istekleri yönetmek için bize modern bir arayüz sunar.

En basit olarak bir HTTP isteği şöyle yapılır:

```js
fetch('/api')
```

Bu bize bir promise döndürüyor, 1. günde promise'leri öğrenmiştik. Yönetmek için `then()` ve `catch()` bloklarını kullanabiliriz. Eğer istek attığımız yer bize json formatında veri gönderiyorsa bunu json olarak iletmemiz gerekiyor. Yani:

```js
fetch('/api')
	.then(res => res.json())
	.then(res => console.log(res))
	.catch(err => console.log(err))
```

`async/awiat` ile isteklerimizi yönetmek isteseydik:

```js
async function getTodo() {
	const response = await fetch('https://jsonplaceholder.typicode.com/todos/1')
	if (response.status === 200) {
		const data = await response.json()
		console.log(data)
	} else if (response.status === 400) {
		console.log('Hata', response.statusText)
	}
}
```

Ayrıca istek yaparken bazı ayarlarıda belirtebilirsiniz. Örneğin:

```js
const formData = new FormData()
formData.append('name', 'Tayfun')
formData.append('surname', 'Erbilen')

fetch('/api', {
	method: 'POST',
	body: formData
})
```

Ayrıca isteği json olarak göndermekte isteyebilirsiniz:

```js
let data = {
	name: 'Tayfun',
	surname: 'Erbilen'
}

fetch('/api', {
	method: 'POST',
	body: JSON.stringify(data),
	headers: {
    'Content-Type': 'application/json',
  },
})
```

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

### `Link` Componenti

Sayfalar arası geçiş için kullanılır. Örneğin:

```js
<Link to="/contact">İletişim</Link>
```

Tek sorun, aktif sayfada bununla bir aktif class'ı vermek mümkün değil :)

### `NavLink` Componenti

Diğerinden farklı olarak burada aktif class ya da style vermek mümkün.

```js
<NavLink to="/contact" style={({ isActive }) => isActive ? {
	backgroundColor: 'yellow'
}}>İletişim</NavLink>

<NavLink to="/contact" className={({ isActive }) => isActive ? 'active' : ''}>İletişim</NavLink>

<NavLink to="/contact">
	{({isActive}) => (
		<>
			İletişim
			{isActive.toString()}
		</>
	)}
</NavLink>
```

> NavLink'e hiçbir class ya da style tanımı yapılmazsa otomatik olarak `active` sınıfı atanır.

### `Outlet` Componenti

Parent component'de child component'leri render etmek için kullanılır. Örneğin `/blog` sayfasında nested bir yapı var, ve layout düzeni oluşturup child component'leri `<Outlet />` ile render edebiliriz.

```js
import { Outlet } from "react-router-dom"

function Blog() {
	return (
		<>
			<nav>..linkler</nav>
			<Outlet />
		</>
	)
}
```

### `Navigate` Componenti

Yönlendirme işlemini component bazında yapmak isterseniz kullanabilirsiniz.

```js
<Navigate to="/blog" state={{
	state1: 'test'
}} />
```

### `useLocation()` Hook'u

Mevcut `location` nesnesini döndürür. Bu useEffect() ile mevcut lokasyon değiştiğinde de kullanılabilir.

```js
import { useEffect } from 'react';
import { useLocation } from 'react-router-dom';

function App() {
  let location = useLocation();

  useEffect(() => {
    // analyticse data gönder
  }, [location]);
	
	console.log(location)

  return (
    // ...
  );
}
```

> Ayrıca `Navigate` component'i ya da `useNavigate` hook'u ile gönderilen state'lere de erişip kullanabilirsiniz. Örneğin login'e yönlendirme yaparken yönlendirilen sayfayı state'de tutup giriş yaptıktan sonra ilgili sayfaya yönlendirebilirsiniz.

### `useNaviate()` Hooku

Yönlendirme işlemini hook seviyesinde yapmak için kullanılır. Örneğin:

```js
import { useNavigate } from "react-router-dom"

function App() {
	const navigate = useNavigate()
	navigate('/blog', { replace: true, state: {
		state1: 'test'
	}})
}
```

Burada `replace: true` derseniz geriye gittiğinde o yönlendirdiği sayfayı görmezden gelir.

### `useResolvedPath()` Hooku

Verilen path'in location bilgisini döndürür.

```js
useResolvedPath('/blog')
```

### `useRoutes()` Hooku

Route yapısını bir diziden türeterek kullanmanızı sağlar. Böylece route'ları ayrı bir dosyada tanımlarsınız. Örneğin:

```js
// ./routes.js
import Home from "./pages/Home";
import Blog from "./pages/Blog";
import BlogPostDetail from "./pages/Blog/BlogPostDetail";

const routes = [
	{
		path: '/',
		element: <Home />
	},
	{
		path: 'blog',
		element: <Blog />,
		children: [
			{
				path: 'konu/:url',
				element: <BlogPostDetail />
			}
		]
	}
]

export default routes
```

Ve kullanırkende:

```js
import { useRoutes } from "react-router-dom"
import routes from "./routes"

function App() {
	<>
		{useRoutes(routes)}
	</>
}
```

Ayrıca `lazy()` ve `import()` kullanılarak lazy load'da yapılabilirdi.

```js
impory { lazy, Suspense } from "react"
const Home =  lazy(() => import("./pages/Home"))
const Blog = lazy(() => import("./pages/Blog"))
const BlogPostDetail = lazy(() => import("./pages/Blog/BlogPostDetail"));

const routes = [
	{
		path: '/',
		element: <Suspense><Home /></Suspense>
	},
	{
		path: 'blog',
		element: <Suspense><Blog /></Suspense>,
		children: [
			{
				path: 'konu/:url',
				element: <Suspense><BlogPostDetail /></Suspense>
			}
		]
	}
]

export default routes
```

### `useSearchParams()` Hooku

Query String'leri yönetmek için kullanılır. Örneğin şöyle bir link yapınız var:

```
/blog?category=php&search=test
```

Buradaki `category` ve `search` parametrelerini almak için:

```js
import { useSearchParams } from "react-router-dom"

function Blog() {
	const [searchParams, setSearchParams] = useSearchParams()
	
	console.log(searchParams.get('category'))
	console.log(searchParams.get('search'))
}
```

Yeni bir query string tanımlamak içinde:

```js
setSearchParams('category=css')
```

### `generatePath()` Metodu

Verilen değerlerden geçerli bir path çıkartır.

```js
generatePath("/users/:id", { id: 42 }); // "/users/42"
generatePath("/files/:type/*", {
  type: "img",
  "*": "cat.jpg",
}); // "/files/img/cat.jpg"
```

Ayrıca route'ı isimlendirerek ve `generatePath()` ile birlikte isimli olarakta kullanabilirdik: Örneğin:

```js
import routes from "./Routes";
import { generatePath } from "react-router-dom"

export const getPath = (path, data = {}) => {
	let route
	path.split('.').map((current, index) => {
		if (index === 0) {
			route = routes.find(r => r.name === current)
		} else {
			route = route.children.find(r => r.name === current)
		}
	})
	return generatePath(route.path, data)
}
```

Bu işlemi `reduce()` ile yapmak isteseydik:

```js
export const getPath = (path, data = {}) => {
	let route = path.split('.').reduce((route, current) => {
			return !route ?
				routes.find(r => r.name === current) :
				route.children.find(r => r.name === current)
	}, false)
	return generatePath(route.path, data)
}
```

## Formik ile Form İşlemleri

Form'ları daha kontrollü bir şekilde yönetmek için `formik` gibi form paketlerini kullanabiliriz. Bir çok farklı form paketi mevcut, biz formik'i inceleyeceğiz.
