# Mantiqiy operatorlar

JavaScript-da uchta mantiqiy operator mavjud: `||` (YOKI), `&&` (VA), `!` (YO'Q).

Garchi ular "mantiqiy" deb nomlangan bo'lsa-da, ularni faqat mantiqiy emas, balki har qanday turdagi qiymatlarga qo'llash mumkin. Ularning natijasi ham har qanday turdagi bo'lishi mumkin.

Keling, tafsilotlarni ko'rib chiqamiz.

## || (YOKI)

"YOKI" operatori ikkita vertikal chiziq belgisi bilan ifodalanadi:

```js
result = a || b;
```

Klassik dasturlashda mantiqiy YOKI faqat mantiqiy qiymatlarni boshqarish uchun mo'ljallangan. Agar uning biron bir argumenti `true` bo'lsa, u `true` ni qaytaradi, aks holda `false` ni qaytaradi.

JavaScript-da operator biroz ayyorroq va kuchliroq. Avvalo, mantiqiy qiymatlar bilan nima sodir bo'lishini ko'rib chiqamiz.

To'rt mantiqiy kombinatsiya mavjud:

```js run
alert( true || true );   // true
alert( false || true );  // true
alert( true || false );  // true
alert( false || false ); // false
```

Ko'rib turganimizdek, natija har doim `true` bo'ladi, faqat ikkala operand ham `false` bo'lgan holat bundan mustasno.

Agar operand mantiqiy bo'lmasa, uni baholash uchun mantiqiy qiymatga aylantiriladi.

Masalan, `1` raqami `true`, `0` raqami `false` sifatida qabul qilinadi:

```js run
if (1 || 0) { // xuddi if(true || false) kabi ishlaydi
  alert( "to'g'ri!" );
}
```

Ko'pincha, OR `||` berilgan shartlarning *birortasi* `to'g'ri` ekanligini tekshirish uchun `if` ifodasida ishlatiladi.

Masalan:

```js run
let hour = 9;

*!*
if (hour < 10 || hour > 18) {
*/!*
  alert( 'Ofis yopiq.' );
}
```

Biz ko'proq shartlardan o'tishimiz mumkin:

```js run
let hour = 12;
let isWeekend = true;

if (hour < 10 || hour > 18 || isWeekend) {
  alert( 'Ofis yopiq.' ); // dam olish kun
}
```

## YOKI(||) birinchi aniq qiymatni topadi

Yuqorida tavsiflangan mantiq biroz klassikdir. Endi, JavaScript-ning "qo'shimcha" xususiyatlarini keltiramiz.

Kengaytirilgan algoritm quyidagicha ishlaydi.

Bir nechta YOKI(||) bir nechta qiymatlar bilan:

```js
result = value1 || value2 || value3;
```

YOKI `||` operatori quyidagilarni bajaradi:

- Operandlarni chapdan o'ngga baholaydi.
- Har bir operandni mantiqiy qiymatga o'zgartiradi. Agar natija `true` bo'lsa, to'xtaydi va shu operandning asl qiymatini qaytaradi.
- Agar barcha operandlar baholangan bo'lsa (ya'ni barchasi `false` bo'lsa), oxirgi operandni qaytaradi.

Qiymat konvertatsiya qilinmasdan asl shaklida qaytariladi.

Boshqacha qilib aytganda, YOKI `"||"` zanjiri birinchi to'g'ri qiymatni yoki agar bunday qiymat topilmasa, oxirgisini qaytaradi.

Masalan:

```js run
alert( 1 || 0 ); // 1 (1 bu to'g'ri)
alert( true || "nima bo'lganda ham" ); // (true bu to'g'ri)

alert( null || 1 ); // 1 (1 bu birinchi to'g'ri qiymat)
alert( null || 0 || 1 ); // 1 (birinchi to'g'ri qiymat)
alert( undefined || null || 0 ); // 0 (barchasi noto'g'ri, ohirgi qiymat qaytariladi)
```

Bu "toza, mumtoz, faqat mantiqiy YOKI" bilan taqqoslaganda ba'zi qiziqarli foydalanishga olib keladi.

1. **O'zgaruvchanlar yoki ifodalar ro'yxatidan birinchi aniq qiymatni olish.**

    Tasavvur qiling, bizda ma'lumotlni o'z ichiga olgan yoki `null/undefined` bo'lishi mumkin bo'lgan bir nechta o'zgaruvchanlar mavjud. Biz qanday birinchi ma'lumotli o'zgaruvchani topishimiz mumkin?

    YOKI `||` dan foydalanishimiz mumkin:

    ```js run
    let currentUser = null;
    let defaultUser = "John";

    *!*
    let name = currentUser || defaultUser || "unnamed";
    */!*

    alert( name ); // "John" ni tanlaydi - bu birinchi to'g'ri qiymat
    ```

    Agar `currentUser` va `defaultUser` noto'g'ri bo'lsa, `"unnamed"` natijasi qaytariladi.

2. **Qisqartirilgan hisoblash.**

    Operandlar nafaqat qiymatlar, balki ixtiyoriy ifodalar ham bo'lishi mumkin. YOKI ularni chapdan o'ngga qarab baholaydi va sinovdan o'tkazadi. To'g'ri qiymatga erishilganda baholash to'xtatiladi va qiymat qaytariladi. Ushbu jarayon "isqartirilgan hisoblash" deb nomlanadi, chunki u chapdan o'ngga iloji boricha qisqa davom etadi.

    Bu ikkinchi argument sifatida berilgan ifoda tayinlash kabi yon ta'sirga ega bo'lganda aniq ko'rinadi.

    Quyidagi misolda `x` tayinlanmaydi:

    ```js run no-beautify
    let x;

    *!*true*/!* || (x = 1);

    alert(x); // undefined, chunki (x = 1) baholanmagan
    ```

    Agar buning o'rniga birinchi argument `false` bo'lsa, `||` ikkinchisini baholaydi va shu bilan tayinlashni bajaradi:

    ```js run no-beautify
    let x;

    *!*false*/!* || (x = 1);

    alert(x); // 1
    ```

    Tayinlash bu oddiy hol. Boshqa yon ta'sirlar ham jalb qilinishi mumkin.

    Ko'rib turganimizdek, bunday foydalanish holati "`if` ni qisqartirish usuli" hisoblanadi. Birinchi operand mantiqiy qiymatga aylantiriladi. Agar u not'og'ri bo'lsa, ikkinchisi baholanadi.

    Ko'pincha, kodni tushunishni osonlashtirish uchun "muntazam" `if` dan foydalanish yaxshiroq, lekin ba'zida bu qulay bo'lishi mumkin.

## && (VA)

VA operatori ikkita ampersand bilan ifodalanadi `&&`:

```js
result = a && b;
```

Klassik dasturlashda AND har ikkala operand to'g'ri bo'lsa `true` ni qaytaradi, aks holda `false`:

```js run
alert( true && true );   // true
alert( false && true );  // false
alert( true && false );  // false
alert( false && false ); // false
```

`if` bilan misol:

```js run
let hour = 12;
let minute = 30;

if (hour == 12 && minute == 30) {
  alert( 'Vaqt 12:30' );
}
```

Xuddi YOKI kabi, har qanday qiymatni VA operandi sifatida ishlatsa bo'ladi:

```js run
if (1 && 0) { // true && false deb baholandi
  alert( "ishlamaydi, chunki natija noto'g'ri" );
}
```


## VA birinchi soxta qiymatni topadi

Bir nechta VA qiymatlari berilgan:

```js
result = value1 && value2 && value3;
```

AND `&&` operatori quyidagilarni amalga oshiradi:

- Operandlarni chapdan o'ngga baholaydi.
- Har bir operand uchun uni mantiqiy qiymatga o'zgartiradi. Agar natija `false` bo'lsa, to'xtaydi va shu operandning asl qiymatini qaytaradi.
- Agar barcha operandlar baholangan bo'lsa (ya'ni barchasi to'g'ri bo'lsa), oxirgi operandni qaytaradi.

Boshqacha qilib aytganda, VA birinchi soxta qiymatni yoki topilmasa oxirgi qiymatni qaytaradi.

Yuqoridagi qoidalar YOKI ga o'xshaydi. Farqi shundaki, VA birinchi *noto'g'ri* qiymatini qaytaradi, YOKI esa birinchi *to'g'ri* birini qaytaradi.

Misollar:

```js run
// agar birinchi operand to'g'ri bo'lsa,
// VA ikkinchi operandni qaytaradi:
alert( 1 && 0 ); // 0
alert( 1 && 5 ); // 5

// agar birinchi operand noto'g'ri bo'lsa,
// VA uni qaytaradi. Ikkinchi operand e'tiborga olinmaydi
alert( null && 5 ); // null
alert( 0 && "nima bo'lganda ham" ); // 0
```

Bundan tashqari, ketma-ket bir nechta qiymatlarni berishimiz mumkin. Birinchi noto'g'ri operand qanday qaytarilishini ko'ring:

```js run
alert( 1 && 2 && null && 3 ); // null
```

Barcha qiymatlar to'g'ri bo'lsa, oxirgi qiymat qaytariladi:

```js run
alert( 1 && 2 && 3 ); // 3, oxirgisi
```

````smart header="VA `&&` ning ustunligi YOKI `||` dan yuqori"
VA `&&` operatorining ustunligi YOKI `||` dan yuqori.

Shunday qilib `a && b || kodi c && d` ifodasi qavs ichida `&&` joylashgani bilan bir xil: `(a && b) || (c && d)`.
````

Xuddi YOKI kabi VA va `&&` operatori ba'zida "if" o'rnini bosishi mumkin.

Masalan:

```js run
let x = 1;

(x > 0) && alert( 'Noldan kattaroq!' );
```

`&&` ning o'ng qismidagi harakat faqatgina baholash unga yetganida amalga oshiriladi. Ya'ni, faqat `(x > 0)` to'g'ri bo'lsa.

Shunday qilib, biz asosan analogga egamiz:

```js run
let x = 1;

if (x > 0) {
  alert( 'Noldan kattaroq!' );
}
```

`&&` varianti qisqaroq ko'rinadi. Ammo `if` aniqroq va biroz o'qilishi oson.

Shuning uchun har bir konstruktsiyani o'z maqsadi uchun ishlatishni tavsiya etamiz: agarning xohlasak `if` dan foydalaning agar VA xohlasak `&&` dan foydalaning.

## ! (YO'Q)

Mantiqiy YO'Q operatori undov belgisi bilan ifodalanadi "!".

Sintaksis juda oddiy:

```js
result = !value;
```

Operator bitta argumentni qabul qiladi va quyidagilarni bajaradi:

1. Operandni mantiqiy turga o'zgartiradi: `true/false`.
2. Teskari qiymatni qaytaradi.

Masalan:

```js run
alert( !true ); // false
alert( !0 ); // true
```

Ba'zan qiymatni mantiqiy turga aylantirish uchun ikkilamchi YO'Q `!!` ishlatiladi:

```js run
alert( !!"bo'sh bo'lmagan matn" ); // true
alert( !!null ); // false
```

Ya'ni, birinchisi YO'Q qiymatini mantiqiy turga o'zgartiradi va teskari qiymatni qaytaradi, ikkinchi YO'Q yana uni teskariga o'zgartiradi. Oxir-oqibat, biz oddiy qiymatdan mantiqiy konvertatsiyaga egamiz.

Xuddi shu narsani qilishning biroz aniq usuli mavjud -- o'rnatilgan `Boolean` funktsiyasi:

```js run
alert( Boolean("bo'sh bo'lmagan matn") ); // true
alert( Boolean(null) ); // false
```

YO'Q `!` ning ustunligi barcha mantiqiy operatorlarning eng yuqori ko'rsatkichidir, shuning uchun u har doim birinchi bo'lib `&&` yoki `||` dan oldin bajariladi.
