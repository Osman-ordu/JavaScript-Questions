# JavaScript Sorular Listesi

## Temelden ileri düzeye: Javascript problemleri
### Cevaplar, soruların altında gizlenmiştir. Görmek için sadece tıklayın.





###### 1. Çıktısı nedir?

```javascript
function sayHi() {
  console.log(name);
  console.log(age);
  var name = "Osman";
  let age = 26;
}

sayHi();
```

- A: `Osman` ve `undefined`
- B: `Osman` ve `ReferenceError`
- C: `ReferenceError` ve `26`
- D: `undefined` ve `ReferenceError`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: D

Fonksiyonun içinde, önce `var` anahtar kelimesi ile `name` değişkenini tanımladık. Bu demektir ki, değişken varsayılan değeri olan `undefined` ile "hoisting" (hafızada alan oluşturma aşaması) olur, ta ki gerçekten değişkene değer ataması yaptığımız satıra varana dek. `name` değişkenini loglayama çalıştığımız satırda henüz değişkeni tanımlamadık, bu yüzden hala `undefined` değerini saklıyor.
 
`let` (ve `const`) anahtar kelimelerine sahip değişkenler de "hoisted" olur, ama `var`'ın aksine <i>ilk değer ataması</i> yapılmaz. Değişkenleri tanımladığımız (ilk değer ataması yaptığımız) satırdan önce erişilebilir değillerdir. Bu, "geçici ölü alan / geçici değişmez çıktı alanı", "temporal dead zone", olarak adlandırılır. Değişkenlere, tanımlanmadan önce erişmeye çalıştığımız zaman, Javascript `ReferenceError` hatası fırlatır.
</p>
</details>

---

###### 2. Çıktısı Nedir?

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```

- A: `0 1 2` ve `0 1 2`
- B: `0 1 2` ve `3 3 3`
- C: `3 3 3` ve `0 1 2`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: C

Javascript'deki olay kuyruğundan dolayı, `setTimeout` callback fonksiyonu, döngü uygulandıktan _sonra_ çağrılır. `i` değişkeni, ilk döngü sırasında `var` anahtar kelimesi ile tanımlandığından, bu değişken globaldir. Döngü boyunca, `++` unary operatörünü kullanarak, `i`'nin değerini her seferinde `1` arttırdık. İlk örnekte, `setTimeout` callback fonksiyonu çağrıldığı zaman, `i`'nin değeri `3`'e eşitti.

İkinci döngüde, `i` değişkeni `let` anahtar kelimesi kullanılarak tanımlandı: `let` (ve `const`) ile tanımlanan değişkenler "block-scope"dur (block `{}` arasındaki herhangi bir şeydir). Her bir tekrarda, `i` yeni değere sahip olacak ve her değer döngü içinde "scoped" olacak.
</p>
</details>

---
###### 3. Çıktısı Nedir?

```javascript
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius
};

console.log(shape.diameter());
console.log(shape.perimeter());
```

- A: `20` ve `62.83185307179586`
- B: `20` ve `NaN`
- C: `20` ve `63`
- D: `NaN` ve `63`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: B

`diameter` sıradan bir fonksiyonken, `perimeter`'in arrow fonksiyon olduğuna dikkat edin.

Arrow fonksiyonlarda, `this` anahtar kelimesi, sıradan fonksiyonların aksine, kendi sardığı mevcut scope'u referans alır. Bu demektir ki, `perimeter`'i çağırdığımız zaman, `shape` objesini değil, kendi sardığı scope'u referans alıyor (örneğin window).

Bu objede, `radius` değeri olmadığından `undefined` döndürüyor. 

</p>
</details>

---
###### 4. Çıktısı Nedir?

```javascript
+true;
!"Osman";
```

- A: `1` and `false`
- B: `false` and `NaN`
- C: `false` and `false`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: A

Artı (unary plus), işlemeye çalıştığı değişkeni sayıya çevirmeye çalışır. `true` `1` ve `false` `0` demektir.

`'Osman'` harf dizisi doğrusal ("truthy") bir değerdir. Aslında sorduğumuz şey, "bu doğrusal değer yanlış-ımsı ("falsy") mı?". Bu da `false` döndürür. 
</p>
</details>

---
###### 5. Hangisi doğru?

```javascript
const bird = {
  size: "small"
};

const mouse = {
  name: "Mickey",
  small: true
};
```

- A: `mouse.bird.size` geçerli değildir
- B: `mouse[bird.size]` geçerli değildir
- C: `mouse[bird["size"]]` geçerli değildir
- D: Hepsi geçerlidir

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: A

Javascript'te, tüm nesne keyleri string'dir (Symbol olmadıkları müddetçe). Keyleri, string olarak _yazmadıysak_ bile, arka planda string'e çevrilirler.

Javascript, ifadeleri yorumlar (ya da açar ("unboxes")). Köşeli parentez notasyonu kullandığımız zaman, Javascript ilk `[` görür ve `]` bulana kadar devam eder. Ancak ondan sonra ifadeyi hesaplar.

`mouse[bird.size]`: Önce `bird.size` çalıştırılır, o da `"small"` demektir. `mouse["small"]`, `true` döndürür.

Ancak, nokta notasyonunda bu gerçekleşmez. `mouse`, `bird` diye bir keye sahip değildir ki bu da `mouse.bird`, `undefined` demektir. Sonra, nokta notasyonunu kullanarak `size`'a ulaşmak istiyoruz: `mouse.bird.size`. `mouse.bird`, `undefined` olduğundan, aslında ulaşmaya çalıştığımız `undefined.size`. Bu geçerli değil ve `Cannot read property "size" of undefined`'a benzer bir hata fırlatacaktır.
</p>
</details>

---


###### 6. Çıktısı Nedir?

```javascript
let c = { greeting: "Hey!" };
let d;

d = c;
c.greeting = "Hello";
console.log(d.greeting);
```

- A: `Hello`
- B: `Hey!`
- C: `undefined`
- D: `ReferenceError`
- E: `TypeError`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: A

Javascript'te tüm nesneler, birbirlerine eşitlendikleri zaman _referansları_ ile etkileşime girerler.

Önce, `c` değişkeni bir nesnenin değerini tutuyor. Sonra, `d`'ye aynı referansı atadık ki bu referans da `c`'nin sahip olduğu nesnedir.
<img src="https://i.imgur.com/ko5k0fs.png" width="200">

Bir nesneyi değiştirdiğiniz zaman, hepsini değiştirirsiniz.

</p>
</details>

---
###### 7. Çıktısı Nedir?

```javascript
let a = 3;
let b = new Number(3);
let c = 3;

console.log(a == b);
console.log(a === b);
console.log(b === c);
```

- A: `true` `false` `true`
- B: `false` `false` `true`
- C: `true` `false` `false`
- D: `false` `true` `true`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: C

`new Number()` yerleşik bir yapıcı fonksiyondur ("function constructor"). Sayı ("number") gibi gözükse de gerçekten bir sayı değil: bir kaç ekstra özelliğe sahip ve o bir nesne.

`==` operatörünü kullandığımız zaman, sadece aynı _değer_'e sahip olup olmadığını kontrol eder. İkisi de `3` değerine sahip, yani `true` döndürür.

Ancak, `===` kullandığımız zaman değer _ve_ tip aynı olmalıdır. Öyle değil: `new Number()`, sayı ("number") değildir, **nesne**dir. İkisi de `false` döndürür.
</p>
</details>

---

###### 8. Çıktısı Nedir?

```javascript
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor;
    return this.newColor;
  }

  constructor({ newColor = "green" } = {}) {
    this.newColor = newColor;
  }
}

const freddie = new Chameleon({ newColor: "purple" });
console.log(freddie.colorChange("orange"));
```

- A: `orange`
- B: `purple`
- C: `green`
- D: `TypeError`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: D

`colorChange` fonksiyonu statiktir. Statik methodlar, sadece oluşturuldukları kurucuda var olmak için tasarlanmıştır ve herhangi bir "children"a aktarılamaz. `freddie`, "child" olduğundan, fonksiyon aktarılmadı ve `freddie` üzerinden erişilebilir değil: `TypeError` hatası fırlatılır.
</p>
</details>

---
###### 9. Çıktısı Nedir?

```javascript
let greeting;
greetign = {}; // Yazım hatası!
console.log(greetign);
```

- A: `{}`
- B: `ReferenceError: greetign is not defined`
- C: `undefined`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: A

Nesneyi loglar çünkü global nesne üzerinde boş bir nesne oluşturduk. `greeting`'i `greetign` olarak yanlış yazdığımız zaman, JS yorumlayıcısı bunu `global.greetign = {}` olarak gördü (ya da tarayıcı içinde `window.greetign = {}`).

Bundan kaçınmak için, `"use strict"` kullanabiliriz. Bu, bir değişkene herhangi bir atama yapmadan önce tanımladığınızdan emin olmanızı sağlar.
</p>
</details>

---

###### 10. Bunu yaptığımız zaman ne gerçekleşir?

```javascript
function bark() {
  console.log("Woof!");
}

bark.animal = "dog";
```

- A: Hiç bir şey, tamamen iyi!
- B: `SyntaxError`. Fonksiyonlara bu şekilde özellik atayamazsın.
- C: `"Woof"` şeklinde loglanır.
- D: `ReferenceError`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: A

Javascript'te bu mümkün, çünkü fonksiyonlar nesnedir! (Primitive tiplere nazaran her şey nesnedir)

Fonksiyon özel bir nesne tipidir. Yazdığınız kod asıl fonksiyon değil. Fonksiyon, özelliklere sahip bir nesnedir. Bu özellik çalıştırabilme kapasitesine sahiptir ("invocable").

</p>
</details>

---
###### 11. Çıktısı Nedir?

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};

console.log(member.getFullName());
```

- A: `TypeError`
- B: `SyntaxError`
- C: `Lydia Hallie`
- D: `undefined` `undefined`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: A

Sıradan fonksiyonlarla yaptığınız gibi bir yapıcıya ("constructor") özellik ekleyemezsiniz. Eğer, tek seferde tüm nesnelere özellik eklemek isterseniz, bunun yerine prototype kullanmalısınız. Yani bu durumda,
```js
Person.prototype.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};
```

`member.getFullName()` çalışmış olacak. Bu neden faydalı? Diyelim ki, yapıcının kendisine bu methodu ekledik. Belki de tüm `Person` instance'leri bu methoda ihtiyaç duymuyor. Bu özelliğe sahip olduklarından dolayı, her bir instance hafızadan yer alır, bu da bir sürü hafıza israfı demek. Bunun yerine, eğer sadece prototype'a eklersek, hafızadan sadece bir tek yer alırız ve yine de tüm instanceler bu özelliğe erişebilir!

</p>
</details>

---

###### 12. Çıktısı Nedir?

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const lydia = new Person("Lydia", "Hallie");
const sarah = Person("Sarah", "Smith");

console.log(lydia);
console.log(sarah);
```

- A: `Person {firstName: "Lydia", lastName: "Hallie"}` ve `undefined`
- B: `Person {firstName: "Lydia", lastName: "Hallie"}` ve `Person {firstName: "Sarah", lastName: "Smith"}`
- C: `Person {firstName: "Lydia", lastName: "Hallie"}` ve `{}`
- D:`Person {firstName: "Lydia", lastName: "Hallie"}` ve `ReferenceError`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: A

`sarah` için `new` anahtar kelimesi kullanmadık. `new` kullandığınız zaman, oluşturduğumuz yeni boş nesneyi referans gösterir. Lakin, `new`'i eklemezseniz, **global nesne**'yi referans gösterir!

`this.firstName`, `"Sarah"`'a eşittir ve `this.lastName`, `"Smith"`'e eşittir dedik. Aslında yaptığımız, `global.firstName = 'Sarah'` ve `global.lastName = 'Smith'` diye tanımlamaydı. `sarah`'ın kendisi `undefined` olarak kalır.
</p>
</details>

---
###### 13. Olay silsilesinin (event propagation) üç aşaması nedir?

- A: Target > Capturing > Bubbling
- B: Bubbling > Target > Capturing
- C: Target > Bubbling > Capturing
- D: Capturing > Target > Bubbling

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: D

**capturing** aşaması süresince, olay ata ("ancestor) elemanlardan hedef elemana doğru gider. Daha sonra **target** elemana ulaşır ve **bubbling** başlar. 

<img src="https://i.imgur.com/N18oRgd.png" width="200">

</p>
</details>

---

###### 14. Bütün nesneler prototiplere sahiptir.

- A: doğru
- B: yanlış

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: B

Bütün nesneler prototiplere ("prototypes") sahiptir, **temel nesne**, **base object**, hariç. Temel nesne, kullanıcı tarafından oluşturulmuş nesnedir, ya da `new` anahtar kelimesi kullanarak oluşturulmuş bir nesnedir. Temel nesne bazı method ve özelliklere erişebilir, `.toString` gibi. Yerleşik gelen Javascript methodlarını kullanabilme sebebi budur! Buna benzer tüm methodlar prototip üzerinden erişebilir. Her ne kadar Javascript, methodu direkt olarak nesneniz üzerinden bulamasa da, prototip zinciri üzerinden aşağıya doğru gider ve orada bulur, böylece sizin için erişebilir yapar.
</p>
</details>

---
###### 15. Çıktısı Nedir?

```javascript
function sum(a, b) {
  return a + b;
}

sum(1, "2");
```

- A: `NaN`
- B: `TypeError`
- C: `"12"`
- D: `3`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: C

Javascript **dinamik tipli dil**dir: değişkenlerin hangi tip olduğunu belirtmeyiz. Değerler otomatik olarak diğer tipi bilmeden dönüştürebilir, bu _implicit type coercion_, _örtük tip dönüşümü_, olarak adlandırılır. **Coercion** bir tipi diğer bir tipe dönüştürmektir.

Bu örnekte, JavaScript sayı olan `1`'i string'e dönüştürüyor, fonksiyonun mantıklı olması ve değer döndürmesi için. Sayısal tip (`1`) ve string tip (`2`)'nin toplanması sırasında, sayıya string olarak muamele edilir. Stringleri `"Hello" + "World"` şeklinde birleştirebiliriz, yani burada olan `"1" + "2"` ki bu da `"12"` döndürür.

</p>
</details>

---

###### 16. Çıktısı Nedir?

```javascript
let number = 0;
console.log(number++);
console.log(++number);
console.log(number);
```

- A: `1` `1` `2`
- B: `1` `2` `2`
- C: `0` `2` `2`
- D: `0` `1` `2`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: C

**son ek**, **postfix**, `++` unary operatörü:


1. Değer döndürür (`0` döndürür)
2. Değeri arttırır (sayı şimdi `1`)

**ön ek**, **prefix**, `++` unary operatörü:

1. Değeri arttırır (sayı şimdi `2`)
2. Değeri döndürür (`2` döndürür)

Burada `0 2 2` döner.

</p>
</details>

---
###### 17. Çıktısı Nedir?

```javascript
function getPersonInfo(one, two, three) {
  console.log(one);
  console.log(two);
  console.log(three);
}

const person = "Lydia";
const age = 21;

getPersonInfo`${person} is ${age} years old`;
```

- A: `"Lydia"` `21` `["", " is ", " years old"]`
- B: `["", " is ", " years old"]` `"Lydia"` `21`
- C: `"Lydia"` `["", " is ", " years old"]` `21`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: B

Eğer "tagged template literals" kullanırsanoz, ilk argumanın değeri her zaman string değerler dizisidir. Geriye kalan argumanlar, ifadeye geçilen değerleri alır.

</p>
</details>

---

###### 18. Çıktısı Nedir?

```javascript
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log("You are an adult!");
  } else if (data == { age: 18 }) {
    console.log("You are still an adult.");
  } else {
    console.log(`Hmm.. You don't have an age I guess`);
  }
}

checkAge({ age: 18 });
```

- A: `You are an adult!`
- B: `You are still an adult.`
- C: `Hmm.. You don't have an age I guess`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: C

Eşitliği test ederken, nesneler _referanslarına_ göre kıyaslanırken primitifler _değerlerine_ göre kıyaslanır. Javascript, nesnelerin referanslarının hafızada aynı konumda olup olmadığını kontrol eder.

Kıyasladığımız iki nesne bu şekilde değilse: parametre olarak geçtiğimiz nesne, hafızada eşitliğini kontrol ettiğimiz nesneden farklı bir konumu referans gösterir.

Bu sebepten, `{ age: 18 } === { age: 18 }` ve `{ age: 18 } == { age: 18 }`, ikisi de `false` döndürür.

</p>
</details>

---

###### 19. Çıktısı Nedir?

```javascript
function getAge(...args) {
  console.log(typeof args);
}

getAge(21);
```

- A: `"number"`
- B: `"array"`
- C: `"object"`
- D: `"NaN"`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: C

Yayma operatorü, "spread operator", (`...args`) argumanların dahil olduğu bir dizi döndürür. Dizi bir nesnedir, bu yüzden `typeof args` `"object"` döndürür.

</p>
</details>

---

###### 20. Çıktısı Nedir?

```javascript
function getAge() {
  "use strict";
  age = 21;
  console.log(age);
}

getAge();
```

- A: `21`
- B: `undefined`
- C: `ReferenceError`
- D: `TypeError`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: C

`"use strict"` ile, yanlışlıkla global değişkenler tanımlamadığınızdan emin olabilirsiniz. `age` değişkenini hiç tanımlamadık ve `"use strict"` kullandığımızdan, referans hatası fırlatacaktır. Eğer `"use strict"` kullanmasaydık, `age` özelliği global nesneye eklenmiş olacağından, çalışmış olacaktı.

</p>
</details>

---
###### 21. `sum`'ın değeri nedir?

```javascript
const sum = eval("10*10+5");
```

- A: `105`
- B: `"105"`
- C: `TypeError`
- D: `"10*10+5"`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: A

`eval`, string olarak geçilen kodu çalıştırır. Eğer bir ifadeyse, bu durumdaki gibi, ifadeyi çalıştırır. İfade `10 * 10 + 5`. Bu `105` sayısını döndürür.

</p>
</details>

---
###### 22. cool_secret ne kadar süre boyunca erişebilinir?

```javascript
sessionStorage.setItem("cool_secret", 123);
```

- A: Sonsuza kadar, veri kaybolmaz.
- B: Kullanıcı sekmeyi kapatınca.
- C: Kullanıcı tamamen tarayıcıyı kapattığı zaman, sadece sekmeyi değil.
- D: Kullanıcı bilgisayarını kapattığı zaman.

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: B

`sessionStorage` içinde saklanan veri _sekme_ kapatıldıktan sonra kaldırılır.

Eğer `localStorage` kullandıysanız, veri sonsuza kadar orada olacaktır, örnek olarak `localStorage.clear()` çalıştırılmadığı sürece. 

</p>
</details>

---
###### 23. Çıktısı Nedir?

```javascript
var num = 8;
var num = 10;

console.log(num);
```

- A: `8`
- B: `10`
- C: `SyntaxError`
- D: `ReferenceError`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: B

`var` anahtar kelimesi ile aynı isme sahip birden çok değişken tanımlayabilirsiniz. O halde değişken son değeri tutacak.

Bunu `let` ya da `const` ile yapamazsınız, "block-scoped" olduklarından dolayı.

</p>
</details>

---

###### 24. Çıktısı Nedir?

```javascript
const obj = { 1: "a", 2: "b", 3: "c" };
const set = new Set([1, 2, 3, 4, 5]);

obj.hasOwnProperty("1");
obj.hasOwnProperty(1);
set.has("1");
set.has(1);
```

- A: `false` `true` `false` `true`
- B: `false` `true` `true` `true`
- C: `true` `true` `false` `true`
- D: `true` `true` `true` `true`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: C

Tüm nesne anahtarları (Symbol'ler hariç) arka planda string'dir, kendiniz string olarak yazmasanız bile. Bu sebepten `obj.hasOwnProperty('1')` da `true` döndürür.

Set için bu şekilde çalışmaz. Setimizde `'1'` yok: `set.has('1')` `false` döndürür. Setimiz sayısal tip `1`'e sahip, `set.has(1)` `true` döndürür.

</p>
</details>

---

###### 25. Çıktısı Nedir?

```javascript
const obj = { a: "one", b: "two", a: "three" };
console.log(obj);
```

- A: `{ a: "one", b: "two" }`
- B: `{ b: "two", a: "three" }`
- C: `{ a: "three", b: "two" }`
- D: `SyntaxError`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: C

Eğer aynı ada sahip iki tane anahtarınız, "key", varsa, anahtar değiştirilecektir. Hala ilk konumunda olacaktır ama son belirtilen değerle birlikte.

</p>
</details>

---

###### 26. JavaScript global yürütme konteksti, "global execution context", sizin için iki şey oluşturur: global nesne, ve "this" anahtar kelimesi.

- A: doğru
- B: yanlış
- C: değişir

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: A

Temel yürütme konteksti, global yürütme kontekstidir: kodunuzda her yerde erişilebilir olan şeydir.

</p>
</details>

---

###### 27. Çıktısı Nedir?

```javascript
for (let i = 1; i < 5; i++) {
  if (i === 3) continue;
  console.log(i);
}
```

- A: `1` `2`
- B: `1` `2` `3`
- C: `1` `2` `4`
- D: `1` `3` `4`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: C

Eğer belirli şart `true` döndürürse, `continue` ifadesi yinelemeyi, "iteration", atlar.

</p>
</details>

---
###### 28. Çıktısı Nedir?

```javascript
String.prototype.giveLydiaPizza = () => {
  return "Just give Lydia pizza already!";
};

const name = "Lydia";

console.log(name.giveLydiaPizza())
```

- A: `"Just give Lydia pizza already!"`
- B: `TypeError: not a function`
- C: `SyntaxError`
- D: `undefined`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: A

`String`, özellikler de ekleyebileceğimiz tümleşik bir yapıcıdır. Prototipine bir method ekledim sadece. Primitif stringler, string prototip fonksiyonu tarafından üretilen string nesnesine otomatik olarak dönüştürülür. Bu yüzden, tüm stringler (string nesneleri) bu methoda erişebilir!

</p>
</details>

---
###### 29. Çıktısı Nedir?

```javascript
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]);
```

- A: `123`
- B: `456`
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: B

Nesne keyleri otomatik olarak stringe dönüştürülür. `a` nesnesine değeri `123` olacak şekilde, bir nesneyi key olarak atamaya çalışıyoruz.

Ancak, bir nesnesi string hale getirince, `"[object Object]"` olur. Dolayısıyla burada söylediğimiz, `a["object Object"] = 123`. Sonra, aynı şeyi tekrar yapmayı deniyoruz. `c`, dolaylı olarak string hale getirdiğimiz başka bir nesne. O halde, `a["object Object"] = 456`.

Sonra, `a[b]`'yi logluyoruz, ki aslında o da `a["object Object"]`. Onu da `456` olarak atamıştık, o yüzden `456` döndürür.

</p>
</details>

---

###### 30. Çıktısı Nedir?

```javascript
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"));
const baz = () => console.log("Third");

bar();
foo();
baz();
```

- A: `First` `Second` `Third`
- B: `First` `Third` `Second`
- C: `Second` `First` `Third`
- D: `Second` `Third` `First`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: B

Bir `setTimeout` fonksiyonumuz var ve ilk onu çalıştırdık. Yine de en son loglandı.

Bunun nedeni tarayıcılarda, "runtime engine"'a sahip olmamamızdan, `WebAPI` denilen bir şeye sahibiz. `WebAPI`, örneğin DOM ile çalışması için bize, `setTimeout` fonksiyonunu verir.

_callback_ WebAPI'a eklendikten sonra, `setTimeout` fonksiyonun kendisi (callback hariç!) hafıza bloğundan atılır, "popped off the stack".

<img src="https://i.imgur.com/X5wsHOg.png" width="200">

Şimdi, `foo` çalıştı ve `"First"` loglandı.

<img src="https://i.imgur.com/Pvc0dGq.png" width="200">

`foo` hafıza bloğundan atıldı ve `baz` çalıştı. `"Third"` loglandı.

<img src="https://i.imgur.com/WhA2bCP.png" width="200">

WebAPI, her ne zaman bir şeyler hazırsa hafıza bloğuna öylece ekleyemez. Onun yerine callback fonksiyonunu, _queue_ diye adlandıralan bir şeye ekler.

<img src="https://i.imgur.com/NSnDZmU.png" width="200">

Burası olay döngüsünün çalışmaya başlayacapı yerdir. **olay döngüsü**, **event loop**, hafıza bloğuna ve iş kuyruğuna, "task queue", bakar. Eğer hafıza bloğu boşsa, kuyruktaki ilk şeyi alır ve hafıza bloğuna ekler.

<img src="https://i.imgur.com/uyiScAI.png" width="200">

`bar` çalıştı, `"Second"` loglandı ve hafıza bloğundan atıldı.

</p>
</details>

---
###### 31. Butona tıklandığında, event.target nedir?

```html
<div onclick="console.log('first div')">
  <div onclick="console.log('second div')">
    <button onclick="console.log('button')">
      Click!
    </button>
  </div>
</div>
```

- A: Dıştaki `div`
- B: İçteki `div`
- C: `button`
- D: İç içe olan tüm elemanlar listesi.

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: C

Olaya sebep olan en derindeki iç eleman, olayın hedefidir ("event.target"). `event.stopPropagation` kullanarak "bubbling"'i durdurabilirsiniz.

</p>
</details>

---

###### 32. Paragrafa tıkladığınız zaman, çıktı olarak ne loglanır?

```html
<div onclick="console.log('div')">
  <p onclick="console.log('p')">
    Click here!
  </p>
</div>
```

- A: `p` `div`
- B: `div` `p`
- C: `p`
- D: `div`

<details><summary><b>Cevap</b></summary>
<p>

#### Cevap: A

Eğer `p`'ye tıklarsak, iki log görürüz: `p` ve `div`. Olay silsilesi, "event propagation", sırasında, 3 aşama vardır: "capturing", "target", ve "bubbling". Varsayılan olarak, olay işleyiciler, "event handlers", "bubbling" aşamasında (`useCapture`'ı `true` olarak ayarlanmadığı müddetçe) çalıştırılır. En derin elemandan dışa doğru gider.

</p>
</details>

---





