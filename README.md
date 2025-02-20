# clean-code-javascript

## Table of Contents

1. [مقدمه](#مقدمه)
2. [متغییرها](#متغییرها)
3. [توابع](#توابع)
4. [اشیا و ساختار داده ها](#اشیا-و-ساختار-داده-ها)
5. [Classes](#classes)
6. [SOLID](#solid)
7. [Testing](#testing)
8. [Concurrency](#concurrency)
9. [Error Handling](#error-handling)
10. [Formatting](#formatting)
11. [Comments](#comments)
12. [Translation](#translation)




## مقدمه

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

 اصول طراحی نرم افزار، برگرفته از کتاب 
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
نوشته ی Robert C. Martin، ویرایش شده برای جاوااسکریپت.
این راهنما برای استایل دهی به کد نیست، برای نوشتن کدی .
[خوانا، قابل استفاده مجدد و توسعه پذیر](https://github.com/ryanmcdermott/3rs-of-software-architecture) در زبان جاوااسکریپت می باشد.

نیازی نیست به طور حتمی از تمامی قواعد مطرح شده در این راهنما پیروری نمایید، و برخی از قواعد مطرح شده مورد تایید همه نیست.
اصول مطرح شده در اینجا صرفا جهت ترسیم خط مشی در کدنویسی می باشند، نه چیزی بیشتر از آن، اما اصول مطرح شده، حاصل سالها تجربه نویسندگان _Clean Code_ می باشد که برایتان گردآوری شده است. 

سابقه ما، در مهندسی نرم افزار کمی بیش از 50 سال می باشد، و همچنان در حال یادگیری می باشیم. شاید زمانی که معماری نرم افزار به قدمت خود معماری شود، ما نیز ملزم به رعایت قواعدی سختگیرانه تر باشیم. اما در حال حاضر قواعد زیر به شما یا تیمتان کمک می کند که اطمینان داشته باشید، کدی که تولید می نمایید ، از کیفیت لازم برخوردار است.

یک مورد دیگر: دانستن این قواعد شما فورا شما را به توسعه دهنده بهتری تبدیل نمی کند، و همچنین بدین معنی نیست که اگر سالهاست از این قواعد پیروری می کنید، دیگر دچار اشتباه نمی شوید. هر قطعه کدی که می نویسید، یک پیش نویس از چیزی است که قصد دارید به آن برسید، درست همانند خاک رس مرطوب، که به آن حالتی داده می شود، تا به فرم نهایی خود درآید. در نهایت ما با بازخوانی کدهایمان با همکارانمان، آنها را بهبود می دهیم. به خاطز وجود ایراد در رونوشت های اولیه تان که نیاز به بهبود دارند، خودتان را گاز نگیرید، در عوض کدتان را گاز بگیرید!


## **متغییرها**

### از اسامی خوانا و بامعنی استفاده نمایید


**بد:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**خوب:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ بالا](#table-of-contents)**

### از واژگان مشابه برای متغییرهای همنوع استفاده نمایید

**بد:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**خوب:**

```javascript
getUser();
```

**[⬆ بالا](#table-of-contents)**

### از اسامی قابل جستجو استفاده نمایید

ما بیشتر از اینکه کد بنویسیم، کد میخوانیم. بسیار مهم است که کدی که می نویسیم قابل خواندن و جستجو باشد. با عدم استفاده از اسامی با معنی برای متغییرهایمان، خوانندگان کد خود را به زحمت می اندازید. اسامیتان را قابل جستجو نمایید. ابزارهایی مانند [buddy.js](https://github.com/danielstjules/buddy.js) و [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) به شما کمک می کنند تا ثابت های بی نام را شناسایی نمایید. 

**بد:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**خوب:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ بالا](#table-of-contents)**

### از متغییر برای توضیح استفاده نمایید

**بد:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**خوب:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ بالا](#table-of-contents)**

### Avoid Mental Mapping
### از مپ کردن در ذهنتان خودداری نمایید

اسامی واضح بهتر از اسامی غیر واضح است.

**بد:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**خوب:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

**[⬆ بالا](#table-of-contents)**

### از افزودن کلمات غیر ضروری خودداری نمایید

 اگر نام کلاس / شی (آبجکت) شما مفهومی را میرساند، همان مفهوم را در نام متغییرهایتان تکرار نکنید. 

**بد:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**خوب:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car, color) {
  car.color = color;
}
```

**[⬆ بالا](#table-of-contents)**

### به جای اتصال کوتاه یا شرطی از آرگومان های پیشفرض استفاده نمایید

آرگومان های دارای مقدار پیشفرض معمولا بهتر از short circuiting می باشد.
باید بدانید که اگر از آنها استفاده نمایید، در این صورت فانکشن شما تنها برای آرگومان های با مقدار
 `undefined`
 یک مقدار پیشفرض خواهد داشت .
 دیگر مقادیر
 "falsy"
 مانند
 `''`و `""`و `false`و `null`و `0`و `NaN` 
با مقادیر پیشفرضتان جایگزین نخواهند شد

**بد:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**خوب:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ بالا](#table-of-contents)**

## **توابع**

### آرگومان های توابع (2 یا کمتر بهتر است)

محدود کردن تعداد پارامترهای تابع بسیار مهم است، چرا که تست تابع تان را آسان تر می کند. داشتن بیشتر از 3 آرگومان منجر به انفجار ترکیبی 
(combinatorial explosion)
می شود.

که بدین معنی است که برای تست آن باید، حالت های بسیار زیادی را به ازای هر آرگومان آن تست کنید. 

یک یا دو آرگومان حالت ایده آل می باشد، و از 3 ارگومان تا جای ممکن باید خودداری شود.
هرچیزی بیشتر از آن باید تجمیع شود. 
معمولا در صورتی که فانکشن شما به بیشتر از 3 آرگومان نیاز داشته باشد، نشان دهنده آن است که دارد بیش از یک کار انجام می دهد (کارهای زیادی انجام می دهد).
در صورتی که اینچنین نیست، معمولا آبجکت والد فانکشن شما به عنوان آرگومان برای فانکشنتان کافی است.

از آنجا که در جاوااسکریپت می توانید یه راحتی یک آبجکت بسازید، در صورتی که نیاز دیدید که تعداد زیادی آرگومان داشته باشید، می توانید به جای آن یک آبجکت بسازید.

برای واضح کردن اینکه فانکشن شما چه آرگومان هایی دریافت می کند، می توانید از قابلیت
destructuring syntax
در
ES2015/ES6
استفاده نمایید. که مزایای زیر را دارد:
1. وقتی کسی به تعریف فانکشنتان نگاه می اندازد، به سرعت متوجه می شود چه پروپرتی هایی مورد استفاده قرار گرفته اند. 
2. می توان از آن برایشبیه سازی پارامترهای دارای نام استفاده نمود.
3. قابلیت 
 Destructuring
همچنین مقادیر موجود پروپرتی های ابجکتتان را در متغییرهای جدید 
clone
می کند، که مانع از عوارض جانبی پیش بینی نشده، ناشی از ویرایش احتمالی مقدار پروپرتی های آبجکت می شود. نکته: دقت داشته باشید که آبجکت و آرایه هایی که از آبجکت آرگومان 
destructure
 مشوند، مقدارشان
 clone 
 نمی شود
 4. ابزارهای 
 Linter 
 کد، شما را از پراپرتی های بلااستفاده آبجکتتان اگاه می کنند، که بدون قابلیت
 destructuring
 غیر ممکن می باشد.

**بد:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**خوب:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

**[⬆ بالا](#table-of-contents)**

### فانکشن ها باید یک کار انجام دهند

این مورد با فاصله بسیار زیاد، مهم ترین قاعده در مهندسی نرم افزار است.
وقتی فانکشن ها بیش از یک کار انجام دهند، به سختی قابل ساختن، تست بوده و فهم عملکردشان با یک نگاه ساده غیر ممکن می باشد .
وقتی که فانکشنی به یک کار محدود شده باشد، به راحتی قابل بازنویسی (ریفکتور) می باشد، و کد شما بسیار خوانا می شود.
اگر فقط همین یک مورد را شما عملا از این راهنما یادبگیرید، از دولوپرهای دیگر بسیار جلوتر خواهید بود.

**بد:**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**خوب:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ بالا](#table-of-contents)**

### نام توابع باید گویای کاری باشد که انجام می دهند

**بد:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**خوب:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ بالا](#table-of-contents)**

### توابع باید تنها دارای یک سطح انتزاعی باشند

وقتی شما بیش از یک سطح انتزاعی داشته باشید، فانکشن شما دارد کارهای زیادی انجام می دهد. 
تقسیم کردن توابع منجر به قابلیت استفاده مجدد سادگی در تست می شود.

**بد:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**خوب:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**[⬆ بالا](#table-of-contents)**

### کد تکراری را حذف کنید

تمام تلاشتان را بکنید که از نوشتن کد تکراری خودداری نمایید. کد تکراری بد است، چرا که در صورت نیاز به تغییر بیش از یک جا را باید تغییر دهید.

تصور کنید که شما یک رستوران دار هستید و جزئیات را غذاها را در فهرستتان درج می کنید. همه جزئیات مانند، گوجه، پیاز، سیر، ادویه ها و غیره...
در این صورت اگر چندین لیست داشته باشید که در آن این جزئیات را درج کرده باشید، همه آنها را باید آپدیت کنید.
اما اگر تنها یک بیست منو داشته باشید، فقط نیاز به بروزرسانی یکجا را دارید!

معمولا شما کد تکراری را به این دلیل دارید که هر کدامشان در حال انجام کاری کمی متفاوت از دیگری است، گرچه کدهایتان کاری یکسان انجام می دهند، اما به خاطر تفاوت های جزئیشان مجبورید دو یا چند تابع جداگانه داشته باشید که کارهایشان تا حد زیادی مشابه یکدیگر است.
حذف کد تکراری به معنای ایجاد یک انتزاع است که بتواند یک ست از چیزهای متفاوت را تنها با یک فانکشن / ماژول / کلاس انجام دهد.

درست ایجاد کردن یک انتزاع بسیار مهم است، به این دلیل است که توصیه می شود از اصول سخت گیرانه مطرح شده در بخش 
_Classes_
پیروی نمایید.
انتزاع نامناسب می تواند بدتر از کد تکراری باشد، پس مراقب باشید! 
با توجه به آنچه گفته شد، اگر می توانید یک انتزاع مناسب ایجاد نمایید، آن را انجام دهید، در غیر این صورت خود را به زحمت دوباره کاری می اندازید و متوجه می شوید که برای یک تغییر ساده باید چندین جا را ویرایش نمایید.

**بد:**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**خوب:**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

**[⬆ بالا](#table-of-contents)**

### آبجکت های پیشفرض را با Object.assign ست کنید

**بد:**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**خوب:**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );
  return finalConfig
  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ بالا](#table-of-contents)**

### از flag ها به عنوان پارامتر فانکشن استفاده نکنید

فلگ ها به کاربر شما می گوید، که این تابع بیش از یک کار انجام می دهد. توابع باید یک کار انجام دهند. در صورتی که توابع تان توسط فلگ کارهای متفاوتی انجام می دهند، آنها را تقسیم نمایید.

**بد:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**خوب:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ بالا](#table-of-contents)**

### از عوارض جانبی پشگیری کنید (بخش 1)

یک تابع باعث عوارض جانبی می شود، اگر کاری بیش از دریافت یک مقدار و بازگرداندن یک یا چند مقدار انجام دهد. 
یک عارضه جانبی می تواند، نوشتن در یک فایل، ویرایش متغییرهای گلوبال، و یا اشتباها قرار دادن پول تان در دسترس یک غریبه است.

گاهی اوقات ممکن است، در یک نرم افزار نیاز داشته باشید که تغییراتی کنترل نشده داشته باشید. مانند مثال قبل، ممکن است نیاز داشته باشید که در فایل چیزی را بنویسید. کاری که باید انجام دهید این است که عملیات مورد نظرتان را در یک مکان متمرکز کنید و از داشتن چندین تابع که همین کار را انجام می دهند خودداری کنید. به عنوان مثال می توانید فقط یک سرویس برای درج دیتا ها در آن فایل داشته باشید و در هر جایی از کدتان از همان سرویس استفاده نمایید.

امتیاز اصلی این عمل این است که از دردسرهای متداول پیشگیری می کنید. مانند: شیر کردن وضعیت بین آبجکت ها بدون هیچ ساختار منظمی، استفاده از متعییرهای قابل تغییر که ممکن است در هر بخشی از کدها مقدارشان دستکاری شده باشد، و متمرکز نبودن نقطه ای که عملیات جانبی شما اتفاق می افتد. 
اگر این کار را انجام دهید، از بسیاری برنامه نویسان دیگر خوشحال تر خواهید بود. 


**بد:**

```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**خوب:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ بالا](#table-of-contents)**

### از عوارض جانبی پشگیری کنید (بخش 2)

در جاوااسکریپت، برخی متغییرها، مقدارشان قابل تغییر و برخی دیگر غیرقابل تغییر است. آبجکت ها و ارایه ها مقدارشان قابل تغییر است. پس باید بسیار دقت داشته باشید، وقتی که آنها را به عنوان پارامتر به یک تابع ارسال می کنید، چرا که ممکن است در آنجا پروپرتی های ابجکت دستکاری شوند و یا محتوای آرایه تغییر کند، که به راحتی موجب بروز باگ در جای دیگری از کدتان می شود.

تصور کنید که تابعی دارید که یک آرایه را به عنوان پارامتر دریافت می کند که حاوی اطلاعات کارت خرید مشتری است. 
اگر تابع شما تغییری در آن ارایه ایجاد کند، به عنوان مثال یک آیتم به آن اضافه نماید - در این صورت هر تابع دیگری که از آن آرایه استفاده می کند، تحت تاثیر این تغییر قرار می گیرد. شاید به نظر این اتفاق خیلی خوبی باشد، اما باید بدانید که می تواند بد هم باشد. بیایید که حالت بد را تصویر کنیم:

کاربر بر روی دکمه خرید کلیک می کند، که این دکمه نیز فانکشن 
purchase
را فراخوانی کند و درنهایت یک درخواست برای سرور ارسال می شود که حاوی آرایه ی موارد انتخابی برای خرید است.
اما به دلیل اختلال در شبکه فانکشن 
purchase
مرتب به خطا مواجه می شود و نیاز پیدا می کند که درخواست را مرتبا تکرار نماید.
حالا چه اتفاقی می افتد اگر کاربر به صورت اتفاقی، دکمه افزودن به سبد را مجددا کلیک کند ؟
اگر این اتفاق بیوفتد و درخواست برای سرور مجددا ارسال شود، آیتم جدید که اتفاقی اضافه  شده بود برای سرور ارسال می شود. چرا که این آیتم به آرایه ما اضافه شده است.

یک راه حل خیلی خوب این است که فانکشن `addItemToCart` همیشه یک نسخه کپی از `cart` تهیع، آن را ویرایش کرده و برگرداند. این موضوع باعث می شود که فانکشن های دیگری که در حال استفاده از این فانکشن هستند، تحت تاثیر این تغییر قرار نگیرند.

دو هشدار برای استفاده از این روش:
1- ممکن است که حالتی پیش آید که شما بخواهید آبجکت ورودی را دستکاری کنید، اما وقتی این روش را پیاده سازی کنید، خواهید دید که این موارد بسیار نادر اتفاق می افتند. اکثر بخش ها قابل باز نویسی هستند، تا ساید افکت نداشته باشید!

2- کپی گرفتن ابجکت های خیلی بزرگ ممکن است به لحاظ پرفورمنسی شما را دچار مشکل کنند. خوشبختانه این مساله مشکل حادی نیست، چرا که  
[کتابخانه های بسیار خوبی](https://facebook.github.io/immutable-js/)
 برای این مشکل توسعه داده شده اند که باعث می شوند این تکنینک کدنویسی، تا جای ممکن سریع باشد و حافظه را به خوبی مدیریت کند و عملکردشان بسیار بهتر از آن است که خودتان این کار را انجام دهید.


**بد:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**خوب:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ بالا](#table-of-contents)**

### Don't write to global functions
### فانکشن گلوبال ننویسید

یک تمرین خیلی بد در جاوااسکریپت آلوده کردن فضای گلوبال به فانکشن هایی است که می نویسید. این فانکشن ها ممکن است به راحتی با فانکشن های یک کتابخانه ی دیگر تداخل ایجاد کنند. و ممکن است کاربر شما حتی متوجه این ایراد نشود، تا زمانی که به یک خطا در هنگام دیپلوی بر روی نسخه پابلیک بربخورد.  
فرض کنید شما در کتابخانه تان یک متود با نام `diff` به متود Array که پیشفرض جاوااسکریت است اضافه می کنید، که کاربردش نماش تفاوت میان دو آرایه است. برای این کار شما به  
`Array.prototype`
 یک متود اضافه می کنید، چه اتفاقی میافتد اگر کتابخانه دیگری سعی کند متود مشابهی را برای نمایش تفاوت دو المان اول و آخر یک آرایه نوشته باشد؟
 
 به این دلیل بهتر است که از کلاس ها که در ES2015/ES6 اضافه شده اند استفاده نمایید و اکستند کردن `Array` کلاس خودتان را ایجاد و متود جدیدتان را به آن اضافه نمایید.  
 

**بد:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**خوب:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ بالا](#table-of-contents)**

### Favor functional programming over imperative programming

### ترجیحا از برنامه نویسی فانکشنال استفاده نمایید

جاوااسکریپت زبان فانکشنال، به صورتی که در هسکل می بینیم، نیست، اما قابلیت فانکشنال بودن را دارد. زبان های فانکشنال تمیزتر و قابل تست تر هستند. 
هرجا ممکن است از این استایل کدنویسی استفاده کنید.


**بد:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**خوب:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ بالا](#table-of-contents)**

### Encapsulate conditionals

### شروط را کپسوله (خلاصه) کنید


**بد:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**خوب:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ بالا](#table-of-contents)**

### Avoid negative conditionals

### از نوشتن شروط منفی خودداری کنید


**بد:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**خوب:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ بالا](#table-of-contents)**

### Avoid conditionals

### از نوشتن شرط ها خودداری کنید

این موضوع نشدنی به نظر میرسد. به محض شنیدن این جمله اکثر مردم می گویند چطور می شود بدون نوشتن یک شرط if کاری را انجام داد؟ پاسخ این است که در بسیاری از موارد می توانید از 
polymorphism 
استفاده نمایید. سوال دوم معمولا این است که خیلی هم عالی، اما چرا باید این کار را بکنم؟ پاسخ این است که در راهکار کدنویسی تمیز که قبلا یادگرفتیم، گفتیم که: یک تابع فقط باید یک کار انجام دهد، وقتی فانکشنی مینویسید که در آن از دستور if استفاده شده، به کاربر خود می گویید که فانکشن شما بیش از یک کار را انجام میدهد. این را به خاطر بسپارید: فقط یک کار انجام دهید!



**بد:**

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**خوب:**

```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ بالا](#table-of-contents)**

### Avoid type-checking (part 1)

### از چک کردن تایپ ها خودداری کنید (بخش 1)

جاوااسکریپت untyped است. بدین معنی که فانکشن های می توانند هر نوع آرگومانی دریافت کنند. گاهی اوقات ممکن است این موضوع شما را فریب بدهد و باعث شود تلاش کنید در فانکشنتان تایپ ورودی ها را چک کنید. 
راه های زیادی برای خودداری از این کار وجود دارد. 
اولین آن داشتن API های تطابق پذیر است.


**بد:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**خوب:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ بالا](#table-of-contents)**

### Avoid type-checking (part 2)

### از چک کردن تایپ ها خودداری کنید (بخش 2)

اگر با مقادیر از نوع داده اولیه (primitive) مانند: integer , string سر و کار دارید و نمی توانید از polymorphism استفاده نمایید، اما همچنان احساس می کنید، نیاز به بررسی نوع دارید، می توانید از typescript استفاده نمایید.
 که یک جایگزین عالی برای جاوااسکریپت معمولی است، چرا که قابلیت بررسی تایپ ایستا (static type checking) را با سینکتکسی مشابه جاوااسکریپت استاندارد را به شما میدهد.
 مشکل بررسی تایپ به صورت دستی در جاوااسکریپت معمولی، این است که درست انجام دادنش، باید مقادیر زیادی کد بی مورد به کد اصلیتان اضافه کنید، که باعث ناخوانا و کثیف شدن کدها می شود. 
کد تمیز و تست های خوب بنویسید و ریویوی خوب دریافت کنید. در غیر این صورت بهتر است از typescript استفاده کنید.


**بد:**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
}
```

**خوب:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ بالا](#table-of-contents)**

### Don't over-optimize

### بیش از حد بهینه نکنید

بسیاری از مرورگرها در runtime و در پسزمینه، بهینه سازی های بسیاری انجام میدهند.
[منابع خوبی وجود دارند](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
 برای مشاهده اینکه بهینه سازی در کجا ضعف دارد. فقط همان ها راانجام دهید، تا زمانی که این بهینه سازی ها انجام شوند (اگر شدنی باشند)


**بد:**

```javascript
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**خوب:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ بالا](#table-of-contents)**

### Remove dead code

### کد بلا استفاده را حذف کنید

کد بلااستفاده (مرده) به بدی کد تکراری است. دلیلی برای نگه داشتن آن در سورس کدهایتان وجود ندارد. اگر صدا زده نمی شود، از شرش خلاص شوید! 
اگر فکر می کنید، ممکن است در آینده به آن احتیاج پیدا کنید، بدانید که جایش در تاریخچه ورژن هایتان امن است (مثلا git)، نگران نباشید!


**بد:**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**خوب:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ بالا](#table-of-contents)**

## **اشیا و ساختار داده ها**

### Use getters and setters

### از getter و setter ها استفاده کنید

استفاده از getter و setter ها برای دسترسی به مقادیر موجود در یک آبجکت می تواند بهتر از دسترسی مستقیم به این مقادیر باشد. چرا؟ به این دلایل نامرتب:

- اگر در زمان خواندن پراپرتی ابجکت، نیاز به انجام کار خاصی پیدا کنید، مجبور نخواهید بود هر جایی که در کدتان به آن ارجاع داده اید را پیدا کرده و ویرایش نمایید.
- زمانی که `set` انجام می دهید، خیلی راحت تر می توانید ورودی ها را اعتبارسنجی (validate) کنید.
- بخش داخلی را از دسترسی خارجی کپسوله می کنید.
- لاگ گذاری و رسیدگی به خطا را آسان تر می کنید.
- اگر فرضا قرار است از سرور مقادیرتان لود شود، میتوانید مقادیر آبجکتتان را بارگذاری در زمان نیاز lazy load کنید.


**بد:**

```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**خوب:**

```javascript
function makeBankAccount() {
  // this one is private
  let balance = 0;

  // a "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

**[⬆ بالا](#table-of-contents)**

### Make objects have private members

### پراپرتی های خصوصی برای آبجکت ها ایجاد کنید

این مساله با استفاده از closure ها برای سینتکس قبل از ES5 امکان پذیر است.

**بد:**

```javascript
const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**خوب:**

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

**[⬆ بالا](#table-of-contents)**

## **Classes**

## **کلاس ها**



### Prefer ES2015/ES6 classes over ES5 plain functions

### ترجیحا از کلاس های ES2015/ES6 به جای فانکشن های ES5 استفاده نمایید

It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer ES2015/ES6 classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

ساختن چیزی مشابه کلاس های ES6 با استفاده از سینتکس ES5 کمی سخت است. مسائلی مانند ارث بری، ساخت نسخه اولیه و تعریف متود برای در کلاس های ES6 بسیار تسهیل و مفهومی تر شده اند. 
گرچه توصیه می شود، ترجیحا از فانکشن های کوچک به جای کلاس هااستفاده نمایید، مگر اینکه به یک شی پیچیده و خیلی کامل احتیاج داشته باشید.

**بد:**

```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**خوب:**

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

**[⬆ بالا](#table-of-contents)**

### Use method chaining

### از قابلیت صدا زدن زنجیره ای متودها استفاده کنید.


این ساختار بسیار کفید است در JavaScript و آن را در بسیاری از کتابخانه ها مانند jQuery و loadash می بینید.
این ساختار باعث می سود که کدتان خودتوصیف گر و خلاصه شود.
به همین دلیل توصیه می کنیم که از قابلیت صدازدن زنجیره ای استفاده کنید و ببینید چقدر کد نهاییتان تمیز و خوانا می شود. در متودهای کلاس تان، ابجکت this را در انتها برگردانید و این قابلیت را به همین سادگی فراهم کنید.

**بد:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

**خوب:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ بالا](#table-of-contents)**

### Prefer composition over inheritance

طبق توصیه های معروف مشابه 
[_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns)
 بهتر است ترجیحا از مفهوم composition (ترکیب کردن) به جای ارث بری، هر جایی که امکان پذیر است استفاده نمایید.
 دلایل خوب زیادی برای هر دو ساختار ترکیب کردن و ارث بری وجود دارد.
 قاعده کلی برای تشخیص اینکه کدا یک بهتر است، این است که اگر ذهن تان ناخودآگاه به سمت استفاده از ارث بری میرود، سعی کنید به این مساله فکر کنید که آیا ترکیب کردن مشکلم را بهتر حل می کند یا نه ؟ در برخی موراد پاسخ مثبت است.
 
 ممکن است برایتان سوال پیش آید، که: چه موقع باید از ارث بری استفاده کنم؟ پاسخ این است که این موضوع به مشکلی که در دست دارید بر میگردد. 
 اما یک لیست خلاصه و ترتمیز از برخی مواردی که ارث بری بر ترکیب کردن ترجیح دارد را در زیر آورده ایم:
 
 1. ارث بری شما دارای ساختار رابطه ای "is-a" می باشد نه "has-a" 
(مثلا Human->Anima و User->UserDetails)
2. می توانید از کدهای کلاس اصلی مجددا استفاده نمایید (انسان، مانند حیوانات می تواند حرکت کند)
3. قصد دارید با یک تغییر در کلاس اصلی، تغییری در همه کلاس های ارث برنده ایجاد نمایید. (مثلا میزان مصرف کالری بر اثر حرکت برای همه حیوانات)


**بد:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**خوب:**

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

**[⬆ بالا](#table-of-contents)**

## **SOLID**

### Single Responsibility Principle (SRP)


همانطور که در قواعد کدنویسی تمیز گفته شده، "نباید بیش از یک دلیل برای تغییر در یک کلاس وجود داشته باشد".
داشتن یک کلاس با کلی قابلیت بسیار وسوسه انگیز است، مثل اینکه برای پروازتان فقط یه چمدان داشته باشید. 
مشکل این است که کلاستان از نظر ساختاری منسجم نخواهد بود و همین موضوع بسیاری دلایل برای تغییرش ایجاد خواهد کرد.
به همین دلیل کاهش تعداد دفعاتی که نیاز است کلاستان را تغییر دهید، بسیار مهم است.
این مساله مهم است، چراکه اگر قابلیت های متعددی در یک کلاس باشد و شما سعی کنید بخضی از آن را تغییر دهید، به سختی می تواند حدس زد، چه ماژول های وابسته ای در کدهایتان تحت تاثیر این تغییر قرار خواهند گرفت.


**بد:**

```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**خوب:**

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ بالا](#table-of-contents)**

### Open/Closed Principle (OCP)

همانطور که 
Bertrand Meyer 
اشاره می کند، موجودیت های نرم افزاری (کلاس ها، ماژول ها، توابع و غیره.) باید قابل گسترش و غیرقابل ویرایش باشند. این یعنی چی ؟ 
این قاعده می گوید، کد شما باید اجازه افزودن قابلیت جدید بدون نیاز به تغییر در کد موجود را بدهد.


**بد:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // transform response and return
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**خوب:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```

**[⬆ بالا](#table-of-contents)**

### Liskov Substitution Principle (LSP)

This is a scary term for a very simple concept. It's formally defined as "If S
is a subtype of T, then objects of type T may be replaced with objects of type S
(i.e., objects of type S may substitute objects of type T) without altering any
of the desirable properties of that program (correctness, task performed,
etc.)." That's an even scarier definition.

The best explanation for this is if you have a parent class and a child class,
then the base class and child class can be used interchangeably without getting
incorrect results. This might still be confusing, so let's take a look at the
classic Square-Rectangle example. Mathematically, a square is a rectangle, but
if you model it using the "is-a" relationship via inheritance, you quickly
get into trouble.

**بد:**

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**خوب:**

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ بالا](#table-of-contents)**

### Interface Segregation Principle (ISP)

JavaScript doesn't have interfaces so this principle doesn't apply as strictly
as others. However, it's important and relevant even with JavaScript's lack of
type system.

ISP states that "Clients should not be forced to depend upon interfaces that
they do not use." Interfaces are implicit contracts in JavaScript because of
duck typing.

A good example to look at that demonstrates this principle in JavaScript is for
classes that require large settings objects. Not requiring clients to setup
huge amounts of options is beneficial, because most of the time they won't need
all of the settings. Making them optional helps prevent having a
"fat interface".

**بد:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.settings.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

**خوب:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {}
  }
});
```

**[⬆ بالا](#table-of-contents)**

### Dependency Inversion Principle (DIP)

This principle states two essential things:

1. High-level modules should not depend on low-level modules. Both should
   depend on abstractions.
2. Abstractions should not depend upon details. Details should depend on
   abstractions.

This can be hard to understand at first, but if you've worked with AngularJS,
you've seen an implementation of this principle in the form of Dependency
Injection (DI). While they are not identical concepts, DIP keeps high-level
modules from knowing the details of its low-level modules and setting them up.
It can accomplish this through DI. A huge benefit of this is that it reduces
the coupling between modules. Coupling is a very bad development pattern because
it makes your code hard to refactor.

As stated previously, JavaScript doesn't have interfaces so the abstractions
that are depended upon are implicit contracts. That is to say, the methods
and properties that an object/class exposes to another object/class. In the
example below, the implicit contract is that any Request module for an
`InventoryTracker` will have a `requestItems` method.

**بد:**

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // BAD: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**خوب:**

```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ بالا](#table-of-contents)**

## **Testing**

Testing is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you
didn't break anything. Deciding on what constitutes an adequate amount is up
to your team, but having 100% coverage (all statements and branches) is how
you achieve very high confidence and developer peace of mind. This means that
in addition to having a great testing framework, you also need to use a
[good coverage tool](https://gotwarlost.github.io/istanbul/).

There's no excuse to not write tests. There are [plenty of good JS test frameworks](https://jstherightway.org/#testing-tools), so find one that your team prefers.
When you find one that works for your team, then aim to always write tests
for every new feature/module you introduce. If your preferred method is
Test Driven Development (TDD), that is great, but the main point is to just
make sure you are reaching your coverage goals before launching any feature,
or refactoring an existing one.

### Single concept per test

**بد:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles date boundaries", () => {
    let date;

    date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);

    date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);

    date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**خوب:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles 30-day months", () => {
    const date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);
  });

  it("handles leap year", () => {
    const date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);
  });

  it("handles non-leap year", () => {
    const date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**[⬆ بالا](#table-of-contents)**

## **Concurrency**

### Use Promises, not callbacks

Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6,
Promises are a built-in global type. Use them!

**بد:**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

**خوب:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ بالا](#table-of-contents)**

### Async/Await are even cleaner than Promises

Promises are a very clean alternative to callbacks, but ES2017/ES8 brings async and await
which offer an even cleaner solution. All you need is a function that is prefixed
in an `async` keyword, and then you can write your logic imperatively without
a `then` chain of functions. Use this if you can take advantage of ES2017/ES8 features
today!

**بد:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**خوب:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
  try {
    const body = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle()
```

**[⬆ بالا](#table-of-contents)**

## **Error Handling**

Thrown errors are a good thing! They mean the runtime has successfully
identified when something in your program has gone wrong and it's letting
you know by stopping function execution on the current stack, killing the
process (in Node), and notifying you in the console with a stack trace.

### Don't ignore caught errors

Doing nothing with a caught error doesn't give you the ability to ever fix
or react to said error. Logging the error to the console (`console.log`)
isn't much better as often times it can get lost in a sea of things printed
to the console. If you wrap any bit of code in a `try/catch` it means you
think an error may occur there and therefore you should have a plan,
or create a code path, for when it occurs.

**بد:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**خوب:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

### Don't ignore rejected promises

For the same reason you shouldn't ignore caught errors
from `try/catch`.

**بد:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**خوب:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

**[⬆ بالا](#table-of-contents)**

## **Formatting**

Formatting is subjective. Like many rules herein, there is no hard and fast
rule that you must follow. The main point is DO NOT ARGUE over formatting.
There are [tons of tools](https://standardjs.com/rules.html) to automate this.
Use one! It's a waste of time and money for engineers to argue over formatting.

For things that don't fall under the purview of automatic formatting
(indentation, tabs vs. spaces, double vs. single quotes, etc.) look here
for some guidance.

### Use consistent capitalization

JavaScript is untyped, so capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

**بد:**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**خوب:**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

**[⬆ بالا](#table-of-contents)**

### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

**بد:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**خوب:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ بالا](#table-of-contents)**

## **Comments**

### Only comment things that have business logic complexity.

Comments are an apology, not a requirement. Good code _mostly_ documents itself.

**بد:**

```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**خوب:**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**[⬆ بالا](#table-of-contents)**

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**بد:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**خوب:**

```javascript
doStuff();
```

**[⬆ بالا](#table-of-contents)**

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

**بد:**

```javascript
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**خوب:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ بالا](#table-of-contents)**

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the
proper indentation and formatting give the visual structure to your code.

**بد:**

```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar"
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**خوب:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ بالا](#table-of-contents)**

## Translation

This is also available in other languages:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **Armenian**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![rs](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Serbia.png) **Serbian**: [doskovicmilos/clean-code-javascript/](https://github.com/doskovicmilos/clean-code-javascript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [mindfr1k/clean-code-javascript-ua](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)

**[⬆ بالا](#table-of-contents)**
