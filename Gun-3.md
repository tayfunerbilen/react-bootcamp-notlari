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

## React Helmet ile SEO

React projelerinde en temel problem seo gibi görünsede, google ve benzer büyük arama motorları bu konuyu çoktan aştılar. Elbette biz de onlara yardımcı olabiliriz, her sayfaya özel meta dataları bu paket sayesinde kolayca tanımlayıp onların işini kolaylaştırabiliriz.

Kurmak için:

```shell
npm i react-helmet
```

Kullanmak için:

```js
impory { Helmet } from "react-helmet"

function App() {
	return (
		<>
			<Helmet>
				<title>Başlık</title>
				<meta name="description" content="örnek site açıklaması" />
			</Helmet>
		</>
	)
}
```

## Formik ile Form İşlemleri

Form'ları daha kontrollü bir şekilde yönetmek için `formik` gibi form paketlerini kullanabiliriz. Bir çok farklı form paketi mevcut, biz formik'i inceleyeceğiz.


### Kurulum

Kurmak için:

```shell
npm i formik
```

### Kullanım

En basit haliyle kullanımı ise şu şekilde:

```js
import {Formik} from "formik";

export default function Home() {
	return (
		<Formik initialValues={{
			name: 'Tayfun',
			surname: ''
		}} onSubmit={(values, actions) => {
			console.log(values)
			console.log(actions)
		}}>
			{({values, errors, touched, handleChange, handleBlur, handleSubmit, isSubmitting}) => (
				<form onSubmit={handleSubmit}>
					<input type="text" name="name" value={values.name} onChange={handleChange}/> <br/>
					<input type="text" name="surname" value={values.surname} onChange={handleChange}/> <br/>
					<button type="submit" disabled={isSubmitting}>Gönder</button>
				</form>
			)}
		</Formik>
	)
}
```

Formik'in verdiği componentleri kullanarak yönetmek istersek:

```js
import {Formik, Form, Field, ErrorMessage} from "formik";

export default function Home() {
	return (
		<Formik initialValues={{
			name: 'Tayfun',
			surname: '',
			about: '',
			rules: false,
			level: 'junior',
			gender: '2',
			skills: ['php', 'css']
		}} onSubmit={(values, actions) => {
			console.log(values)
			console.log(actions)
		}}>
			{({isSubmitting}) => (
				<Form>
					<Field type="text" name="name" /> <br/>
					<Field type="text" name="surname" /> <br/>
					<Field name="about" component="textarea" /> <br/>
					<div>
						<label>
							<Field type="radio" value="beginner" name="level" />
							Beginner
						</label>
						<label>
							<Field type="radio" value="junior" name="level" />
							Junior
						</label>
						<label>
							<Field type="radio" value="senior" name="level" />
							Senior
						</label>
					</div>
					<label>
						<Field type="checkbox" name="rules" />
						Kuralları kabul et
					</label> <br/>
					<Field component="select" name="gender">
						<option>Seçin</option>
						<option value="1">Erkek</option>
						<option value="2">Kadın</option>
					</Field> <br/>
					<Field component="select" multiple={true} name="skills">
						<option>Seçin</option>
						<option value="php">php</option>
						<option value="javascript">javascript</option>
						<option value="css">css</option>
						<option value="html">html</option>
					</Field> <br/>
					<button type="submit" disabled={isSubmitting}>Gönder</button>
				</Form>
			)}
		</Formik>
	)
}
```

### `useField()` ile Custom Componentler

Formik'in componentleri ya da varsayılan form elemanları yerine daha da özelleştirdiğimiz kendi component'lerimizi de şöyle kullanabiliriz.

```js
import {Formik, Form, useField} from "formik";

function Input({label, ...props}) {
	const [field, meta, helpers] = useField(props)
	return (
		<label>
			{label}
			<input {...field} {...props}/>
		</label>
	)
}

function Textarea({label, ...props}) {
	const [field, meta, helpers] = useField(props)
	return (
		<label>
			{label}
			<textarea {...field} {...props}/>
		</label>
	)
}

function Select({label, options, ...props}) {
	const [field, meta, helpers] = useField(props)
	return (
		<label>
			{label}
			<select {...field} {...props}>
				{options.map((option, key) => <option value={option.key} key={option.key}>{option.value}</option>)}
			</select>
		</label>
	)
}

function Checkbox({label, ...props}) {
	const [field, meta, helpers] = useField(props)
	const clickHandle = () => {
		helpers.setValue(!field.value)
	}
	return (
		<label onClick={clickHandle}>
			<span style={{display: 'inline-block', width: 20, height: 20, backgroundColor: field.value ? 'red' : '#eee'}}/>
			{label}
		</label>
	)
}

function Radio({label, options, ...props}) {
	const [field, meta, helpers] = useField(props)
	const clickHandle = value => {
		helpers.setValue(value)
	}
	return <>
		<h6>{label}</h6>
		{options.map((option, key) => (
			<label key={key} onClick={() => clickHandle(option.key)}>
				<span style={{display: 'inline-block', width: 20, height: 20, borderRadius: '50%', backgroundColor: field.value === option.key ? 'red' : '#eee'}}/>
				{option.value}
			</label>
		))}
	</>
}

function File({label, ...props}) {
	const [field, meta, helpers] = useField(props)
	const changeHandle = e => {
		helpers.setValue(e.target.files[0])
	}
	return (
		<label>
			{label}
			<input type="file" onChange={changeHandle}/>
			Seçilen dosya = {field?.value?.name}
		</label>
	)
}

export default function Home() {
	return (
		<Formik initialValues={{
			name: 'Tayfun',
			surname: '',
			about: '',
			rules: false,
			level: 'junior',
			gender: '2',
			avatar: false,
			skills: ['php', 'css']
		}} onSubmit={(values, actions) => {
			console.log(values)
			console.log(actions)
		}}>
			{({isSubmitting}) => (
				<Form>
					<Input label="Ad" name="name"/> <br/>
					<Textarea label="Hakkımda" name="about"/> <br/>
					<Select label="Cinsiyet" name="gender" options={[
						{key: '1', value: 'Erkek'},
						{key: '2', value: 'Kadın'}
					]}/> <br/>
					<Select label="Yetenekler" name="skills" multiple={true} options={[
						{key: 'css', value: 'CSS'},
						{key: 'javascript', value: 'Javascript'},
						{key: 'php', value: 'php'}
					]}/> <br/>
					<Checkbox label="Kuralları kabul ediyorum" name="rules"/> <br/>
					<Radio label="Seviye" name="level" options={[
						{key: 'beginner', value: 'Başlangıç'},
						{key: 'junior', value: 'Jr. Dev'},
						{key: 'senior', value: 'Sr. Dev'}
					]} /> <br/>
					<File label="Avatar" name="avatar" /> <br/>
					<button type="submit">Gönder</button>
				</Form>
			)}
		</Formik>
	)
}
```

### Yup ile Validasyon

Formik'te validasyon işlemleri yapmak yerine Yup paketini kullanarak çok daha efektik bir şekilde yönetebiliriz. Zaten formik'i yazan arkadaşlarda Yup'a özel bir entegrasyon yapmışlar, kolayca kullanabiliyoruz.

#### Kurulum

İlk olarak yupu şöyle kuralım:

```shell
npm i yup
```

#### Varsayılan Mesajları Ayarlamak

İlk olarak `validations/` diye bir klasör oluşturun ve `validation.js` dosyasına şunları yazın.

```js
import * as Yup from "yup";

Yup.setLocale({
    mixed: {
        required: 'Bu alanı doldurmanız gerekiyor.'
    },
    string: {
        email: 'Geçerli bir e-posta adresi girin.',
        min: 'Bu alan minimum ${min} karakter olmalıdır.',
        max: 'Bu alan maksimum ${max} karakter olmalıdır.',
        url: 'Geçerli bir URL girmelisiniz.'
    },
    boolean: {
        oneOf: 'Bu alanı işaretlemeniz gerekiyor.'
    }
})

export default Yup
```

Artık bir validasyon dosyamız şöyle olmalı:

```js
import Yup from "./validation"

export const UserSchema = Yup.object().shape({
	name: Yup.string()
		.required(),
	surname: Yup.string()
		.required()
});
```

Ve formik ile kullanırken:

```js
import { UserSchema } from "./validations/UserSchema"
<Formik ... validationSchema={UserSchema} >
```

Yukarıdaki örneğimize ait validasyon işlemleri için:

```js
import Yup from "./validation"

export const SampleSchema = Yup.object().shape({
	name: Yup.string().required(),
	surname: Yup.string().required(),
	about: Yup.string().required(),
	gender: Yup.string().required(),
	rules: Yup.boolean().oneOf([true]),
	skills: Yup.array().test({
		message: 'En az 2 seçenek seçin',
		test: arr => arr.length >= 2
	}),
	avatar: Yup.mixed().test({
		message: 'Bir dosya seçin!',
		test: file => file?.name
	})
});
```
