project_path: /web/_project.yaml
book_path: /web/fundamentals/_book.yaml
description: С помощью API геолокации можно определить, в каком месте находится пользователь, и отслеживать его перемещение – всегда с его согласия

{# wf_review_required #}
{# wf_updated_on: 2014-10-20 #}
{# wf_published_on: 2000-01-01 #}

# Отслеживание местоположения пользователя {: .page-title }

{% include "web/_shared/contributors/TODO.html" %}



"С помощью API геолокации можно определить, в каком месте находится пользователь, и отслеживать его перемещение – всегда с его согласия


Использование API не зависит от устройства; способ определения местоположения
браузером не имеет значения, поскольку клиенты могут запрашивать и получать данные
обычным способом. Для этого может использоваться GPS или Wi-Fi. Поскольку на выполнение любого 
такого запроса требуется некоторое время, API работает асинхронно; метод обратного вызова отправляется ему
 каждый раз, когда вы запрашиваете данные о местоположении.

## TL;DR {: .hide-from-toc }
- 'Перед тем как использовать API, всегда проверяйте совместимость.'
- Сведите к минимуму отслеживание местоположения пользователя для экономии ресурса аккумулятора устройства.
- Всегда обрабатывайте ошибки.


## Ситуации, в которых следует использовать геолокацию для определения местоположения пользователя

* Необходимо получить более точные данные о местоположении пользователя.
* Необходимо обновить пользовательский интерфейс приложения на основе данных 
о новом местоположении пользователя.
* Необходимо обновить программный код приложения, реализующий его функциональные возможности, когда пользователь находится
 в определенной зоне.

## Отслеживание местоположения пользователя

С помощью API геолокации можно отслеживать местоположение пользователя (предварительно 
получив на это согласие пользователя), вызвав один раз метод `getCurrentPosition()`.  

Для непрерывного отслеживания местоположения пользователя в 
API геолокации предусмотрен метод `watchPosition()`. Он аналогичен методу 
`getCurrentPosition()`, только вызывается несколько раз, по мере того как:


1.  Программное обеспечение для определения местоположения получает точные данные о местонахождении пользователя.
2.  Местоположение пользователя изменяется.
 

    var watchId = navigator.geolocation.watchPosition(function(position) {
      document.getElementById('currentLat').innerHTML = position.coords.latitude;
      document.getElementById('currentLon').innerHTML = position.coords.longitude;
    });
    

## Обязательная экономия энергии аккумулятора устройства

Отслеживание местоположения — это довольно ресурсоемкая операция.  В операционных 
системах могут быть реализованы функции
привязки приложения к подсистеме геопозиционирования, но вы как веб-разработчик не знаете, какая
 на устройстве пользователя имеется поддержка определения местоположения. В результате, когда вы определяете местоположение пользователя,
устройство вынуждено выполнять много излишней работы.

Когда отслеживание местоположения больше не требуется, вызовите метод `clearWatch`, чтобы 
отключить системы геопозиционирования.

## Всегда обрабатывайте ошибки

К сожалению, не все попытки определить местоположение венчаются успехом. Причиной может послужить невозможность подключиться к системе GPS
или внезапное отключение пользователем функций определения местоположения. В случае ошибки вызывается второй,
дополнительный аргумент для метода getCurrentPosition(),
и вы можете добавить в обратный вызов соответствующее уведомление для пользователя:


    window.onload = function() {
      var startPos;
      var geoSuccess = function(position) {
        startPos = position;
        document.getElementById('startLat').innerHTML = startPos.coords.latitude;
        document.getElementById('startLon').innerHTML = startPos.coords.longitude;
      };
      var geoError = function(position) {
        console.log('Error occurred. Error code: ' + error.code);
        // error.code can be:
        //   0: unknown error
        //   1: permission denied
        //   2: position unavailable (error response from location provider)
        //   3: timed out
      };
      navigator.geolocation.watchPosition(geoSuccess, geoError);
    };
    

