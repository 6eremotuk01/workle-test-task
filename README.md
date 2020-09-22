# Тестовое задание для Workle

### Задание

1. Сверстать страницу по [макету](https://www.figma.com/file/TSike5vJs4oQSbAAh13oZ0/WTZ).
2. Добавить функционал: 
	1. Брать данные через API сайта [Unsplash.com](api.unsplash.com).
	2. При клике по автору в отдельной вкладке открывается его профиль.
	3. При клике по фото ничего не происходит.

### Инструменты

Для исподнения данного задания необходимо использовать фреймворк [Vue.js](https://vuejs.org/)

### Структура

```bash
public
├─── favicon.ico
└─── index.html
src
├─── App.vue
├─── main.js
├─── assets
│    ├─── eye.png
│    └─── loading.png
└───components
    ├─── Error
    │    └─── Error.vue
    ├─── Loading
    │    └───Loading.vue
    ├─── Pagenation
    │    └─── Pagenation.vue
    └─── Post
         ├─── Autor.vue
         ├─── Post.vue
         └─── Views.vue
```