# Basics

## Static type-checking

<p align="right">تایپ‌اسکریپت به عنوان یک type-checker استاتیک (Static Type Checker) ساختار و رفتار متغیرها را قبل از اجرای برنامه بررسی می‌کند. اگر مشکلی ببیند (مثلاً فراخوانی تابعی که وجود ندارد، یا مقدار اشتباه در یک متغیر)، به ما در لحظه‌ی کدنویسی هشدار می‌دهد.</p>

## Non-exception Failures

<p align="right">تایپ‌اسکریپت با وجود اینکه از روی جاوا اسکریپت ساخته شده است، اما دارای یک سیستم تایپ استاتیک (static type system) است که قوانین سختگیرانه تری دارد.</p>

<p align="right">در مثال زیر، تایپ‌اسکریپت خطایی برای استفاده پراپرتی <code>location</code> به دلیل وجود نداشتن آن در object <code>user</code> ارسال می‌کند</p>

<pre class="language-typescript"><code class="lang-typescript">const user = {
    name: "Daniel",
    age: 26
}

<strong>user.<a data-footnote-ref href="#user-content-fn-1">location</a>;
</strong></code></pre>

<p align="right">در مثال زیر، متغیر <code>value</code> فقط می‌تواند <code>"a"</code> یا <code>"b"</code> باشد. اگر شرط <code>if (value !== "a")</code> درست باشد، قطعاً مقدار <code>value</code> <code>"b"</code> است. پس در بخش <code>else</code> هم، مقدار <code>value</code> فقط میتواند <code>"a"</code> باشد. حالا اگر شرط <code>else if (value === "b")</code> را قرار دهیم (که یک عمل بیهوده است)، تایپ اسکریپت هشدار می‌دهد:</p>

```typescript
const value = Math.random() < 0.5 ? "a" : "b";

if (value !== "a") {
    // value = "b"
} else {
    // value = a 
} else if (value === "b") {
    // ❌ This part is unreachable
}
```

## Types for Tooling

<p align="right">تایپ اسکریپت فقط یک سیستم بررسی نوع و نمایش خطا نیست. اگر خطایی رخ دهد ، پیشنهاد هایی را برای اصلاح آن ارائه می دهد.</p>

* 🔄 **Refactoring (بازآرایی کد)**
* 🧭 **Navigation (ناوبری سریع در کد)**
* 🧰 auto completion
* `tsc` , the TypeScript compiler

<p align="right">نکته: <code>tsc</code> کامپایلر مربوط به تایپ‌اسکریپت است که کدهای تایپ‌اسکریپت را به جاوا‌ اسکریپت خام تبدیل می کند. کامپایلر <code>tsc</code> فایل <code>.ts</code> را به یک فایل <code>.js</code> تبدیل می‌کند.</p>

<p align="right">اگر خطایی در فایل <code>.ts</code> وجود داشته باشد ، باز هم کامپایلر فایل خروجی <code>.js</code> را تولید میکند. به این دلیل که شاید شما بخواهید از پروژه جاوا اسکریپتی خود به تایپ‌اسکریپت مهاجرت کنید.</p>

<p align="right">اگر قصد غیر فعال کردن این کار را دارید، باید تنظیمات زیر را به فایل <code>tsconfig.json</code> خود، اضافه کنید.</p>

```json
{
    "compilerOptions": {
        "noEmitOnError": true
    }
}
```

{% hint style="info" %}
<p align="right">فایل <code>tsconfig.json</code> تنظیمات کامپایلر تایپ‌اسکریپت است که نحوه تبدیل کد به جاوا اسکریپت را کنترل می‌کند.</p>
{% endhint %}

## Explicit Types

<p align="right">تایپ‌اسکریپت خودش به صورت اتوماتیک نوع متغیر هایی که به آنها مقداری پاس داده شده را تشخیص می‌دهد، با این حال می‌توان نوع تمام متغیر ها، ورودی و خروجی توابع را تعیین کرد</p>

```typescript
const name: string = "amirhossein"
```

## Erased Types

<p align="right">در خروجی هایی که تایپ‌اسکریپت تولید می‌کند، هیچ کدام از نوع هایی که برای متغیر ها تعریف کرده ایم وجود ندارند زیرا مرورگر نمیتواند تایپ‌اسکریپت را اجرا کند . پس نتیجه می‌گیریم که تایپ‌اسکریپت فقط برای <strong>بررسی ایمنی در زمان توسعه</strong> است.</p>

## Downleveling

<p align="right">تایپ‌اسکریپت کدی که به با <code>Es 6</code> یا بالاتر نوشته شده باشد را به <code>Es 5</code> تبدیل میکند تا جاوا اسکریپت نوشته شده ، در مرورگرهای قدیمی هم قابل اجرا باشد</p>

<p align="right">کد زیر با <code>Es 6</code> نوشته شده است</p>

```typescript
`Hello ${person}, today is ${date.toDateString()}!`
```

<p align="right">در فایل خروجی، تایپ اسکریپت آن را به <code>Es 5</code> باز نویسی کرده است</p>

```javascript
"Hello ".concat(person, ", today is ").concat(date.toDateString(), "!");
```

## strictness

<p align="right">تایپ اسکریپت به صورت پیش فرض سعی می‌کند خیلی سخت گیر نباشد تا برنامه نویسانی که تازه وارد تایپ اسکریپت شده اند، بتوانند به راحتی با آن تعامل پیدا کنند. اما اگر می‌خواهید کد ایمن تری داشته باشید و خطای های پنهان هم نداشته باشید، میتوانید در فایل <code>tsconfig.json</code> خود ، این حالت را فعال کنید.</p>

* ```js
  {
      "compilerOption": {
          "strict": true
      }
  }
  ```

[^1]: Property 'location' does not exist on type '{ name: string; age: number; }'.
