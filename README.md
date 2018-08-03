# Задание 1 — найди ошибки

В этом репозитории находятся материалы тестового задания "Найди ошибки" для [14-й Школы разработки интерфейсов](https://academy.yandex.ru/events/frontend/shri_msk-2018-2) (осень 2018, Москва, Санкт-Петербург, Симферополь).

Для работы тестового приложения нужен Node.JS v9. В проекте используются [Yandex Maps API](https://tech.yandex.ru/maps/doc/jsapi/2.1/quick-start/index-docpage/) и [ChartJS](http://www.chartjs.org).

## Задание

Код содержит ошибки разной степени критичности. Некоторые из них — стилистические, а другие — даже не позволят вам запустить приложение. Вам нужно найти все ошибки и исправить их.

Пункты для самопроверки:

1. Приложение должно успешно запускаться.
1. По адресу http://localhost:9000 должна открываться карта с метками.
1. Должна правильно работать вся функциональность, перечисленная в условиях задания.
1. Не должно быть лишнего кода.
1. Все должно быть в едином codestyle.

## Запуск

```
npm i
npm start
```

При каждом запуске тестовые данные генерируются заново случайным образом.


# Проделанная работа

## Ошибки, влияющие на работу

Список исправленных функциональных ошибок, примерно, в порядке их исправления:

- в index.js обернул import в скобки: <pre><code>import <b>{</b>initMap<b>}</b> from './map'</code></pre> Другое решение - в map.js сделать экспорт по умолчанию: <pre><code>export <b>default</b> initMap</code></pre>

- установил высоту и ширину карты на весь экран (как требуется в условии задания):

		html,
		body,
		#map {
    		width: 100%;
    		height: 100%;
    		margin: 0;
    		padding: 0;
		}

- в map.js loadList должен выглядеть так, иначе наши объекты не добавляются на карту

        loadList().then(data => { 
            objectManager.add(data); 
            myMap.geoObjects.add(objectManager); // этой строчки не было
        });

- в details.js заменил arrow-функции на обычные, т.к. важен контекст выполнения

- следующую строчку в map.js нужно удалить, иначе она сбивает настройки цветов кластеров

        objectManager.clusters.options.set('preset', 'islands#greenClusterIcons');

- в chart.js из строчки:

        yAxes: [{ ticks: { beginAtZero: true, max: 0 } }]

    необходимо убрать ```max: 0``` иначе график строится только в отрицательной полуоси

- стандартный формат времени, открывающийся при наведении на точки графика - некрасивый, и не влезает в границы графика. Я сделал свой формат, ~~с блэкдже~~

- для того, чтобы после приближения карты, её можно было отдалять - я вернул элементы управления картой

- файл popup.js удалил, т.к. он нигде не используется. Надеюсь, это подходит под описание "лишнего" кода

## Стилистические ошибки

Я провёл небольшую разведку и обнаружил https://github.com/ymaps/codestyle

Надеюсь, я не промахнулся, и это действительно code-style Яндекса (или по крайней мере code-style js yandex карт). В любом случае, иметь единый прописанный стиль кода - хороший подход

Решил использовать его + eslint, чтобы ничего не упустить (и иногда исправлял ошибки автоматически, с помощью eslint)

### Основые исправления

- 2 space-indentation --> 4 space-indentation
- double-quotes (```"```) --> single-quotes(```'```)
- { smth } --> {smth}
- ternary operator
	    
	from
    
	    	return isActive
	    		? `rgba(54, 162, 235, ${alpha})`
	    		: `rgba(255, 99, 132, ${alpha})`;
    
	to
	
	    	return isActive ?
	    		`rgba(54, 162, 235, ${alpha})` :
	    		`rgba(255, 99, 132, ${alpha})`;

- в arrow-функциях обернул аргументы круглыми скобками : ```arg => {body}``` --> ```(arg) => {body}```
- изменил, где это возможно, обычные функции в arrow-функции
- var --> const/let
