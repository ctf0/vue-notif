# Requirement

[vuemit](https://github.com/gocanto/vuemit)

# Usage

**1-** register the component

- we use ***Bulma*** for styling the notification cards, but you can change it by editing the component file.

```js
window.Vue = require('vue');
window.EventHub = require('vuemit')

Vue.component('MyNotification', require('./path/to/Notification.vue'))
```

**2-** you can call it either

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

**3-** add the css

```css
.notif-container {
    padding: 0 0 4rem;
    position: fixed;
    top: 4rem;
    right: 1rem;
    bottom: 0;
    z-index: 100;
    overflow: scroll;
}
```

# ToDo

* Turn into Package.
