# Краткий гайд Thymeleaf
# [![XMAHZ](https://i.ibb.co/dkk2MMC/image.png)](https://www.youtube.com/channel/UCPhAmLPS9kw8yPooW7J6KSg)  *powered by* [**XMAHZ**](https://www.youtube.com/channel/UCPhAmLPS9kw8yPooW7J6KSg)
# [**Полная документация**](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html)
## Maven
**With Spring**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
**Without Spring**
```xml
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf</artifactId>
    <version>3.0.11.RELEASE</version>
</dependency>
```

## Gradle
```groovy
// https://mavenlibs.com/maven/dependency/org.thymeleaf/thymeleaf
implementation group: 'org.thymeleaf', name: 'thymeleaf', version: '3.1.1.RELEASE'
```

>  Перед началом надо в аргументах вашего запроса получить **Model**
```java
import org.springframework.ui.Model;

class Any {

  @GetMapping("/main")
  public String getMAin(Model model) {
  }
}
```
Именно с **Model** мы и будем работать в аргументах

Необходимые параметры для работы с **HTML**
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Thymeleaf Variables</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
    <main>
        ...
    </main>
</body>
</html>
```

# Типы данных 
## @{...}

```thymeleafexpressions
@{/local/main.css} - ссылочные переменные типа href="/local/main.css"
обычно для внутренних файлов используются, хотя можно и любые ссылки использовать
href="htttps://ewrey.site" -> @{htttps://ewrey.site}
```
## ${...}
```thymeleafexpressions
${attribute} - переменные для использования в тексте HTML документа, могу инициализироваться например с помощью:
<p th:field="${attribute}"> </p> 
И далее можно использовать только attribute
Именно к этому виду можно приминять операторы "+ - %" и т.д
${user.sex} ? 'мужчина':'женщина'
```
## *{...}
```thymeleafexpressions
<h2 th:object="${user}">
    <p>FirstName: <span th:text="*{name.split(' ')[0]}">Jack</span>.</p>
    <p>LastName: <span th:text="*{name.split(' ')[1]}">Li</span>.</p>
</h2>
Идет сначала инициализация user, а далее к этому атрибуту могут применяться методы из Java
```
> есть еще, **НО**, это краткий гайд же :)

# Утильные объекты
+ **#dates**	
 > + Инструментальный объект, который обрабатывает `java.util.date`
+ **#calendars**	
> +  Объект инструмента `java.util.calendar`
+ **#numbers**	
> + Метод, используемый для форматирования чисел
+ **#strings**	
> + Методы, используемые для обработки строк
+ **#bools**	
> +  Метод, используемый для определения логических значений
+ **#arrays**	
> +  Методы ухода за массивами
+ **#lists**	
> +  Методы, используемые для обработки списков коллекций
+ **#sets**	
> +  Метод, используемый для обработки набора множеств
+ **#maps**
> +  Методы, используемые для обработки коллекций карт

```thymeleafexpressions
Пример:
<p>Сегодня это: <span th:text="${#dates.format(today,'yyyy-MM-dd')}"></span></p>
```

# Типы XML приставок к тегам HTML
## th:text

```thymeleafexpressions
<span th:text="'Добро пожаловать:' + ${user.name} + '!'"></span>
Добавляет текст без ParseMode, т.е текст '<h1>Малый текст</h1>' останется таковым без перевода в HTML
```
## th:utext

```thymeleafexpressions
<span th:utext="'<strong>Добро пожаловать:' + ${user.name} + '!</strong>'"></span>
Добавляет текст с ParseMode, т.е текст '<h1>Малый текст</h1>' будет переведен в HTML
```
## th:if
```thymeleafexpressions
<span th:if="${attr}">Отображение этого текста будет, если attr == true </span>
Логический оператор, может внутри себя функции использовать или работать с атрибутами
```
## th:each
```thymeleafexpressions
<tr th:each="user : ${users}">
    <td th:text="${user.name}"></td>
    <td th:text="${user.age}"></td>
</tr>
Типичный итератор (foreach)
```
## th:switch && th:case
```thymeleafexpressions
<div th:switch="${user.role}">
  <p th:case="'admin'">Пользователь является администратором</p>
  <p th:case="'manager'">Пользователь менеджер</p>
  <p th:case="*">Пользователь - это что-то еще</p>
</div>
Если user.role обрабатывается и его "value" = admin и т.д. то показывается текст из th:case="'admin'"
default case = "*" 
```
## th:with
```thymeleafexpressions
<div th:each="article : ${articles}", th:with="articleName=${article.name}">
    <a th:text="${articleName}" th:href="${article.url}"></a>
</div>
Использование переменных или, если точнее, присваивание ссылок на внутренние переменные
```
## th:errors
```thymeleafexpressions
<div th:errors="*{name}" class="error"/>
Этот тег покажется при ошибке
```
## Использование в \<script>
```javascript
<script th:inline="javascript">
    const user = /*[[${user.name}]]*/ Ewrey;
    const age = /*[[${user.age}]]*/ 228;
    console.log(user.name);
    console.log(age)
</script>
```
