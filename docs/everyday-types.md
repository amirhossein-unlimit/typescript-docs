# Everyday Types

<p align="right">&#x202b;در این بخش از مستندات، به انواع داده‌های پرکاربرد در TypeScript می‌پردازیم.</p>

## The Primitives

<p align="right">&#x202b;سه تایپ <code>string</code>، <code>number</code> و <code>boolean</code> از همه متداول‌تر هستند و در TypeScript و JavaScript یکسان هستند.</p>

## Array

<p align="right">&#x202b;برای تعریف آرایه‌ای از هر نوع مقدار استفاده می‌شود. انواع روش‌های تعریف تایپ از نوع آرایه:</p>

```typescript
// --- syntax 1
// ex: 1. array of numbers
let numbers: number[] = [1, 2, 3];
// ex: 2. array of strings
let names: string[] = ["Ali", "Reza"];

// --- syntax 2
// ex: 1. array of numbers
let numbers: Array<number> = [1, 2, 3];
// ex: 2. array of strings
let names: Array<string> = ["Ali", "Reza"];
```

## Any

<p align="right">&#x202b;هرگز پیشنهاد نمی‌شود از این نوع استفاده کنید زیرا هر نوع داده‌ای را مجاز می‌کند و عملاً حضور TypeScript را بی‌تأثیر می‌کند. این نوع زمانی استفاده می‌شود که TypeScript نوع داده‌ای را تشخیص نمی‌دهد یا نوع داده مدنظر نامشخص است.</p>

```typescript
const chartConfig: any = {
  title: "Sales Chart",
  data: [10, 20, 30],
  colors: ["red", "green", "blue"],
  animation: true
};

oldChartLibrary(chartConfig);
```

## Type Annotations

<p align="right">&#x202b;زمانی که از <code>var</code>، <code>const</code> یا <code>let</code> برای تعریف متغیر استفاده می‌کنیم، می‌توانیم یک توضیح نوع به آن اضافه کنیم تا نوع متغیر به صورت کاملاً صریح مشخص شود و خوانایی کد بالاتر برود.</p>

```typescript
// ex: 1. variable with number type
let age: number = 25;

// ex: 2. variable with string type
let name: string = "Sara";

// ex: 3. variable with boolean type
let isActive: boolean = true;

// ex: 4
let data: null = null;

// ex: 5
let value: undefined = undefined;
```

{% hint style="success" %}
<p align="right">&#x202b;زمانی که متغیر را مقداردهی می‌کنیم، نوع آن به صورت کاملاً صریح برای TypeScript مشخص است. بنابراین در این مواقع نیازی به نوشتن توضیح نوع نیست و انجام این کار باعث طولانی‌تر شدن کد می‌شود.</p>
{% endhint %}

## Function

<p align="right">&#x202b;در TypeScript می‌توانیم نوع پارامترهای ورودی تابع را مشخص کنیم. همچنین می‌توان نوع خروجی تابع را هم مشخص کرد.</p>

```typescript
function greetUser(name: string): string {
  return `سلام ${name}! خوش آمدی.`;
}
```

### Functions Which Return Promises

<p align="right">&#x202b;مشخص کردن نوع توابعی که خروجی آن‌ها به صورت یک Promise است:</p>

```typescript
function fetchData(data: string): Promise<string> {
  return new Promise((resolve, reject) => {
    // Simulate a 2-second delay
    setTimeout(() => {
      if (data) {
        resolve(`داده دریافت شد: ${data}`);
      } else {
        reject(new Error("داده ورودی خالی است!"));
      }
    }, 2000);
  });
}
```

### Anonymous Function

<p align="right">&#x202b;TypeScript می‌تواند نوع ورودی‌های تابع ناشناس را بسته به موقعیت مورد استفاده تشخیص دهد.</p>

<p align="right">&#x202b;در مثال زیر، TypeScript نوع ورودی <code>name</code> را به دلیل اینکه <code>names</code> آرایه‌ای از رشته‌ها است و متد <code>forEach</code> هم قرار است که تک تک این رشته‌ها را به تابع ناشناس ارسال کند، تشخیص می‌دهد:</p>

```typescript
const names = ["Alice", "Bob", "Eve"];

// ex: 1
// Contextual typing for function - parameter s inferred to have type string
names.forEach(function(name) {
  console.log(name.toUpperCase());
});

// ex: 2
// Contextual typing also applies to arrow functions
names.forEach((name) => {
  console.log(name.toUpperCase());
});
```

## Object Types

<p align="right">&#x202b;برای تعریف نوع یک Object در برنامه باید از کلمه <code>interface</code> استفاده کنیم. می‌توانیم پراپرتی‌ها را با <code>,</code> یا <code>;</code> از یکدیگر جدا کنیم.</p>

<p align="right">&#x202b;در مثال زیر، نوع ورودی <code>pt</code> که یک Object است، در همان زمان تعریف نوشته شده است:</p>

```typescript
// --- syntax 1
function printCoord(pt: { x: number; y: number }) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```

```typescript
// --- syntax 2
interface Pt {
  x: number;
  y: number;
}

function printCoord(pt: Pt) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```

### Optional Property

<p align="right">&#x202b;برای ایجاد یک پراپرتی اختیاری، باید بعد از نام پراپرتی و قبل از <code>:</code>، علامت <code>?</code> را قرار دهیم:</p>

```typescript
function printName(obj: { first: string; last?: string }) {
  // ...
}

// ex: 1
printName({ first: "Bob" });
// ex: 2
printName({ first: "Alice", last: "Alisson" });
```

```typescript
function printName(obj: { first: string; last?: string }) {
  console.log(obj.last?.toUpperCase());
}
```

{% hint style="success" %}
<p align="right">&#x202b;برای استفاده ایمن‌تر از پارامتر های اختیاری باید آن ها را بررسی کنیم که <code>undefined</code> نباشند.</p>
{% endhint %}

## Union Type

<p align="right">&#x202b;به ما این امکان را میدهد که یک ورودی یا متغیر را به گونه ای تعریف کنیم که شامل دو یا چند نوع باشد. وقتی یک متغیر از نوع <code>union</code> تعریف می شود، فقط میتوان عملیات هایی روی آن انجام داد که برای هر دو نوع تعریف شده، قابل انجام باشد.</p>

<p align="right">&#x202b;در مثال پایین ورودی <code>id</code> میتواند رشته ای یا عددی باشد.از متد هایی میتوان استفاده کرد که هم برای نوع <code>number</code> و هم <code>string</code> در دسترس باشند.</p>

<p align="right">&#x202b;برای اینکه بتوانیم از متد های مخصوص به هر کدام استفاده کنیم و از تایپ اسکریپت هم خطا دریافت نکنیم، باید استفاده از آنها را مشروط به نوع آن کنیم. به این کار <code>Type Narrowing</code> گفته می شود. در این مثال متد های مربوط به رشته در شرط <code>id === "string"</code> و متد های مربوط به اعداد در <code>else</code> در دسترس خواهند بود.</p>

```typescript
function printId(id: number | string) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id);
  }
}

```
