  * [Pass by reference or value](#pass-by-reference-or-value)
  * [Anchor tag](#anchor-tag)
  * [Filter](#filter)
  * [console with color](#console-with-color)
  * [Logical Assignmnet Operators](#logical-assignmnet-operators)
  * [Numberic Seperators](#numberic-seperators)
  * [Private Accessors](#private-accessors)
  * [JSON.stringify](#jsonstringify)
  * [Optional chaining](#optional-chaining)
  * [IntersectionObserver](#intersectionobserver)
  * [requestAnimationFrame](#requestanimationframe)
  * [Deep Clone and Shallow Clone](#deep-clone-and-shallow-clone)
    + [Shallow Clone vs Deep Clone](#shallow-clone-vs-deep-clone)
    + [Deep Clone](#deep-clone)

## Pass by reference or value

1. `Primitive` types like `Number` or `String` are passed by ***value***, not by reference

```jsx
// 就是說 對方是 Primitive 的話 我們去複製它
// 然後 再更改 它的 value的話 他不會讓 Origin 的 data 也被改變
例子： 
value：我有一杯空茶 我複製了 那杯茶 過後再 給它加水 那樣那原本的茶 還是一樣是空的
refenrence：我有一杯空茶 我複製了 那杯茶 過後再 給它加水 他們兩個都會加(其實 就是 這杯茶是
origin的分身罷了 做什麼都會應該到 原本的obj)
```

## Anchor tag

```html
<style>
	html, body {   
		scroll-behavior: smooth;
	}
</style>

<a href="#tag">Anchor link</a>
<div id="tag"></div>
```

## Filter

```jsx
// return true 就留下來
// return false 就是 拿掉它
const data = [
	{id:1, name: "wilker:", age: 25},
	{id:1, name: "yee"},
	{id:1, name: "laoyeche", age: 25}
]
//首先 我們先思考 我們不要什麼
// 1. 我們不要名字裡有 : 的
// 2. 我們不要沒有 age 的
const filtered = data.filter(item => {
	/*
		indexOf 返回 號碼 就是 說 它存在這裡面 返回 true condition 就是 我不要的
	*/
	if(item.name.indexOf(":") >= 0 || !item.age) return false; // 不要的 
	return true // 要的
})

```

## console with color

```jsx
console.log("%cGame End", 'color: red; font-size: 30px');
```

## Logical Assignmnet Operators

```jsx
let x = 1;
let y = 2;
if(x) {
	x = y
}

// we can do in this way
x &&= y; // if x is true
x ||= y // if x is false

console.log(x);
```

## Numberic Seperators

let x = 1_000_000;

```jsx
console.log(x) // 1000000
```

## Private Accessors

```jsx
class Person {
	#showType {
		console.log("從 instance是拿不到這個的 只能從裡面別的 fn")
	}
	showHello {
		console.log("this is not private");
	}

	showAll() {
		this.#showType();
		this.showHello();
	}
}

const person = new Person();
person.showType(); // person.showType is not a function

```

## JSON.stringify

```jsx
const language = {
	name: 'Javascript',
	year: 1995,
}

JSON.stringify(language, ['name']) // { "name": "Javascript" }
JSON.stringify(language, (key, value) => { return key === 'name': 'js' : v });
// { "name": "js", "year": 1995 }

JSON.stringify(language, null, 4) 
/*
{
		"name": "Javascript",
		"year": 1995
}
*/

JSON.stringify(language, null, 😍) // 如果是 string 它會代替 emtpy space
/*
	😍"name": "Javascript",
	😍"year": 1995
*/ 

```

## Optional chaining

```jsx
const user = {
	name: 'Wilker',
	age: 18
}

user.age // 18
user.details.address // Error
user.details?.address // undefined
```

## IntersectionObserver

```html
Scroll down...
<div data-inviewport="scale-in"></div>
<div data-inviewport="fade-rotate"></div>
```

```css
[data-inviewport] {
  /* THIS DEMO ONLY */
  width: 100px;
  height: 100px;
  background: #0bf;
  margin: 150vh 0;
}

/* inViewport */

[data-inviewport="scale-in"] {
  transition: 2s;
  transform: scale(0.1);
}
[data-inviewport="scale-in"].is-inViewport {
  transform: scale(1);
}

[data-inviewport="fade-rotate"] {
  transition: 2s;
  opacity: 0;
}
[data-inviewport="fade-rotate"].is-inViewport {
  transform: rotate(180deg);
  opacity: 1;
}
```

```jsx
/*
	IntersectionObserver 出來的 instance 是說 
	等下 使用這個 instance的 對象 當它進去 screen 會使用裡面的 邏輯

	obsOptions 裡可以放的值
		root: null|"#tagID" 放null的話 就是 跟著 screen，如果有 tag(必须是目标元素的父级元素)的會 就是跟著那個tag
		rootMargin: "0px",
		threshold: 1.0 | 0.1 > 1 1的放就是 說 在element 最下面 100% 
		isIntersecting 才會是 true 不然 就是 false
*/
const obsOptions = {};
// 做一個 instance and 給他 過後要執行的 事情
const Obs = new IntersectionObserver(inViewport, obsOptions);

const inViewport = (entries, observer) => {
	// entries === 數組 裡面是 當前EL
  entries.forEach((entry) => {
		// enter === e 來的
		/*
			entry.isIntersecting 的意思 是 符合 obsOptions裡設置的規則 過後就變 true 
			代表開始 run 這個 邏輯

			entry.intersectionRatio的用處在 如果 你不想設置 obsOptions在全部
			這個entry.intersectionRatio 就是 0 > 1 你可以在這裡做判斷 去覺得 它什麼時候run邏輯
			if(entry.intersectionRatio > 0) [還不確定 需要再找點資料]
		*/
		if(entry.isIntersecting) {
			entry.target.classList.toggle("is-inViewport", entry.isIntersecting);

			/* trigger 了 就不再 監視這個 element了 （ 第二個參數 ） */
			observer.unobserve(entry.target);
		}
  });
};

// Attach observer to every [data-inviewport] element:
const ELs_inViewport = document.querySelectorAll("[data-inviewport]");
// 每個 [data-inviewport]的 elements 都被加上 監視了 當 dom mounted的時候
ELs_inViewport.forEach((EL) => { 
  Obs.observe(EL, obsOptions);
});
```

## requestAnimationFrame

## Deep Clone and Shallow Clone

```jsx
const food = { beef: '🥩', bacon: '🥓' };

const cloneFood = JSON.parse(JSON.stringify(food));

console.log(cloneFood);
// { beef: '🥩', bacon: '🥓' }
```

- **JSON.stringify/parse** only work with Number and String and Object literal without function or Symbol properties.
- **deepClone** work with all types, function and Symbol are copied by reference.

### Shallow Clone vs Deep Clone

```jsx
const nestedObject = {
  flag: '🇨🇦',
  country: {
    city: 'vancouver',
  },
};

const shallowClone = { ...nestedObject };
// Changed our cloned object
shallowClone.flag = '🇹🇼';
shallowClone.country.city = 'taipei';

console.log(shallowClone);
// {country: '🇹🇼', {city: 'taipei'}}
console.log(nestedObject);
// {country: '🇨🇦', {city: 'taipei'}} <-- 😱
```

### Deep Clone

```jsx
const deepClone = JSON.parse(JSON.stringify(nestedObject));

console.log(deepClone);
// {country: '🇹🇼', {city: 'taipei'}}

console.log(nestedObject);
// {country: '🇨🇦', {city: 'vancouver'}} <-- ✅
```
