# Installation

```bash
npm install vue vuemit --save
```

# Usage

**1-** register the component

- we use ***Bulma*** for styling the notification cards.

```js
window.Vue = require('vue')
window.EventHub = require('vuemit')

Vue.component('MyNotification', require('vue-notif'))
```

**2-** now call it either

- from html

    ```html
    // for example when the backend is redirecting back with a msg
    <div class="notif-container">
        @if (session('status'))
            <my-notification
                icon="false"
                title="title"
                body="body"
                type="success/error/primary/warning"
                duration="1/2/etc..">
            </my-notification>
        @endif
    </div>
    ```

- or from js

    ```js
    EventHub.fire('showNotif',{
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

    ```html
    <div class="notif-container">
        <my-notification></my-notification>
    </div>
    ```

> - type > `default: info`
> - duration > `default: null` if not present, card will remain visible
> - icon > `default: true`
