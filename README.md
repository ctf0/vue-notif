<h1 align="center" style="border: none">
    Vue Notif
    <br>
    <a href="https://www.npmjs.com/package/vue-notif"><img src="https://img.shields.io/npm/v/vue-notif.svg?style=for-the-badge" alt="npm" /></a> <a href="https://www.npmjs.com/package/vue-notif"><img src="https://img.shields.io/npm/dt/vue-notif.svg?style=for-the-badge" alt="npm" /></a>
</h1>

## Installation

```bash
npm install vue-notif --save
```

<br>

## Usage

- register the component "we use ***Bulma*** for styling the notification cards".

    ```js
    window.Vue = require('vue')
    window.EventHub = require('vuemit')

    Vue.component('MyNotification', require('vue-notif'))
    ```

- add the component to your page

    ```html
    <div class="notif-container">
        <my-notification></my-notification>
    </div>
    ```

- now call it either

    + from html

        ```html
        <div class="notif-container">
            <my-notification
                title="title"
                body="body"
                type="success/error/primary/warning"
                :icon="false"
                :duration="1/2/etc.."
                :on-close="somefunction()">
            </my-notification>
        </div>
        ```

    + or from js

        ```js
        EventHub.fire('showNotif', {
            title: 'title',
            body: 'body',
            type: 'success',
            duration: 1,
            icon: false,
            onClose(){
                // what happen when notification is closed
            }
        });
        ```

    |   prop   | required |   type   |                   default                   |
    |----------|----------|----------|---------------------------------------------|
    | title    | :x:      | string   | null                                        |
    | body     | :x:      | string   | null                                        |
    | type     | :x:      | string   | info                                        |
    | duration | :x:      | number   | in seconds / null "card will remain visible"|
    | icon     | :x:      | bool     | true                                        |
    | onClose  | :x:      | function | null                                        |

- clicking the card itself will dismiss the notification as well.
