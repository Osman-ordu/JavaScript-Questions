# JavaScript Sorular Listesi

Temelden ileri düzeye: Javascript'i ne kadar iyi bildiğinizi test edin, bilginizi biraz tazeleyin ya da mülakatanıza hazırlanın! :muscle: :rocket: 

Cevaplar, soruların altında gizlenmiştir. Görmek için sadece tıklayın. İyi şanslar





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


