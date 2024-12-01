# Опис на проектот
Овој проект е едноставен систем за регистрација и најава со двофакторска автентикација (2FA). Корисниците можат да креираат профил со е-пошта, корисничко име и лозинка. На секоја најава, покрај лозинката, е потребно да внесат верификациски код, кој се генерира при регистрација.

## Функционалности
### Регистрација:

Корисникот внесува податоци: корисничко име, е-пошта и лозинка.
Лозинката се хашира со помош на CryptoJS пред зачувување.
Системот генерира 6-цифрен верификациски код.
#### Најава:

Корисникот внесува е-пошта, лозинка и верификациски код.
Системот проверува дали лозинката и кодот се точни.
#### Зачувување на податоци:

Сите податоци (корисничко име, е-пошта, хаширана лозинка и код) се зачувуваат локално во localStorage.
#### Успешна најава:

Доколку сите податоци се точни, корисникот е пренасочен на страната со порака „Добредојде“.
Изглед на апликацијата
#### Регистрација
Корисникот внесува податоци и добива верификациски код.
![image](https://github.com/user-attachments/assets/550ffe10-4ef5-4827-960d-fd3980a0cb78)

![Слика од екранот 2024-12-01 175048](https://github.com/user-attachments/assets/2ee642c9-20a2-45e1-b62d-4528337e1aeb)



Корисникот внесува е-пошта, лозинка и верификациски код.
#### Најава
![Слика од екранот 2024-12-01 175132](https://github.com/user-attachments/assets/4a289cf7-e454-4f00-89e0-9e42161c1625)


#### Успешна најава
Корисникот е пренасочен на страницата „Добредојде на страната“.
![image](https://github.com/user-attachments/assets/5d8a1e74-f18c-47f4-88bc-2133861c004f)


## Како да го користите проектот
### 1. Регистрација
Отворете ја датотеката register.html во вашиот прелистувач.
Внесете корисничко име, е-пошта и лозинка.
Кодот за верификација ќе биде генериран и зачуван.
Запишете го кодот за да го користите при најава.
### 2. Најава
Отворете ја датотеката login.html во вашиот прелистувач.
Внесете ја е-поштата и лозинката што ги користевте при регистрација.
Внесете го генерираниот верификациски код.
Ако податоците се точни, ќе бидете пренасочени на success.html.
Технички детали
#### Технологии
HTML: За структурата на страниците.

CSS: За стилизирање на корисничкиот интерфејс.

JavaScript: За логиката на апликацијата.

CryptoJS: За хаширање на лозинки.
#### Структура на проектот
register.html: Страница за регистрација.
login.html: Страница за најава.
success.html: Страница за успешно најавување.
#### Како работи
Податоците се зачувуваат во localStorage во формат:
json
```
{
  "username": "Име на корисникот",
  "password": "Хаширана лозинка",
  "verificationCode": "123456",
  "verified": false
}
```
При најава, лозинката се хашира повторно и се споредува со зачуваната хаширана вредност.
Верификацискиот код се проверува за точност.
Пример од кодот
#### Генерирање и зачувување на кориснички податоци
```

document.getElementById("registerForm").addEventListener("submit", function (event) {
  event.preventDefault();

  const username = document.getElementById("username").value;
  const email = document.getElementById("email").value;
  const password = document.getElementById("password").value;

  // Хаширање на лозинката
  const hashedPassword = CryptoJS.SHA256(password).toString();

  // Проверка дали корисникот веќе постои
  if (localStorage.getItem(email)) {
    alert("Корисникот веќе постои!");
    return;
  }

  // Генерирање верификациски код
  const verificationCode = Math.floor(100000 + Math.random() * 900000).toString();
  localStorage.setItem(email, JSON.stringify({
    username,
    password: hashedPassword,
    verificationCode,
    verified: false
  }));

  alert(`Успешно се регистриравте! Код за верификација: ${verificationCode}`);
});
```
#### Проверка на верификациски код
```
document.getElementById("loginForm").addEventListener("submit", function (event) {
  event.preventDefault();

  const email = document.getElementById("email").value;
  const password = document.getElementById("password").value;
  const code = document.getElementById("code").value;

  const user = JSON.parse(localStorage.getItem(email));
  if (!user) {
    alert("Корисникот не постои!");
    return;
  }

  const hashedPassword = CryptoJS.SHA256(password).toString();
  if (user.password !== hashedPassword) {
    alert("Погрешна лозинка!");
    return;
  }

  if (user.verificationCode !== code) {
    alert("Невалиден код за верификација!");
    return;
  }

  alert("Успешно се најавивте!");
  window.location.href = "success.html";
});
```
