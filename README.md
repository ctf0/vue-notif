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
        onClose(){
            // what happen when notification close button is clicked
        }
    });
    ```

    ```html
    <div class="notif-container">
        <my-notification></my-notification>
    </div>
    ```

> - type > default: info
> - duration > if not present, card will remain visible

**3-** add the css
```css
.notif-container {
    padding: 2rem 0;
    position: fixed;
    top: 0;
    right: 1rem;
    bottom: 0;
    z-index: 100;
    overflow: scroll;
}
```

# ToDo

* [ ] Turn into Package.
