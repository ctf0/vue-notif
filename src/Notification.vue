<template>
    <div>
        <!-- single -->
        <transition v-if="self_show" name="slide-fade">
            <div :class="classObj(self_type)" class="item" @click="closeSelf()">
                <button class="delete" @click="closeSelf()"/>
                <div class="media">
                    <div v-if="self_icon" class="media-left">
                        <figure class="icon is-large">
                            <img :src="getIcon(self_type)">
                        </figure>
                    </div>
                    <div class="media-content">
                        <h4 class="title">
                            <strong>{{ self_title }}</strong>
                        </h4>
                        <p class="subtitle">{{ self_body }}</p>
                    </div>
                </div>
            </div>
        </transition>

        <!-- events -->
        <template v-if="!self_title">
            <span v-if="checkForGroup()"
                  id="close_all"
                  class="tag is-rounded is-dark is-medium" @click="closeAll()">
                Close All
                <button class="delete"/>
            </span>

            <transition-group name="slide-fade" tag="ul">
                <li v-for="(one,index) in notif_group"
                    v-if="IsVisible(index)"
                    :key="`${one.type}_${index}`"
                    :class="classObj(one.type)"
                    class="item"
                    @click="closeNotif(index)">

                    <button class="delete" @click="closeNotif(index)"/>
                    <div class="media">
                        <div v-if="one.icon" class="media-left">
                            <figure class="icon is-large">
                                <img :src="getIcon(one.type)">
                            </figure>
                        </div>
                        <div class="media-content">
                            <h4 class="title">
                                <strong>{{ one.title }}</strong>
                            </h4>
                            <p class="subtitle">{{ one.body }}</p>
                        </div>
                    </div>

                </li>
            </transition-group>
        </template>
    </div>
</template>

<style>
    .notif-container {
        padding: 0;
        position: fixed;
        top: 4rem;
        right: 1rem;
        z-index: 10000;
    }
</style>

<style scoped>
    .icon img {
        height: 3rem;
        filter:invert(100%);
    }

    /*animation*/
    .slide-fade-enter-active,
    .slide-fade-leave-active {
        transition: all 0.3s ease;
    }
    .slide-fade-enter,
    .slide-fade-leave-to {
        opacity: 0;
        transform: translateX(10px);
    }

    /*notiifcation card*/
    .item {
        width: 330px;
    }
    .media-left {
        align-self: center;
        position: relative;
        margin-right: 1.25rem;
    }

    .has-shadow {
        box-shadow: 0 2px 4px rgba(0,0,0,0.12), 0 0 6px rgba(0,0,0,0.04);
    }
    .notification {
        padding: 1.25rem;
        margin-bottom: 10px;
    }

    .notification .delete {
        right: 0.5rem;
        top: 0.5rem;
    }

    #close_all {
        background-color: rgba(54, 54, 54, 0.9);
        cursor: pointer;
        position: fixed;
        z-index: 1;
        top: 1rem;
        right: 1rem;
    }

    #close_all:hover{
        background-color: rgb(54, 54, 54);
    }

    #close_all .delete {
        margin-left: 0.25rem;
        margin-right: -0.375rem;
    }
</style>

<script>
export default {
    props: {
        title: {
            type: String,
            required: false,
            default: ''
        },
        body: {
            type: String,
            required: false,
            default: ''
        },
        icon: {
            type: Boolean,
            required: false,
            default: true
        },
        type: {
            type: String,
            required: false,
            default: 'info'
        },
        duration: {
            type: Number,
            required: false,
            default: null
        },
        onClose: {
            type: Function,
            required: false,
            default: undefined
        }
    },

    data() {
        return {
            notif_group: [],
            self_title: this.title,
            self_body: this.body,
            self_type: this.type,
            self_icon: Boolean(this.icon),
            self_duration: this.duration,
            self_show: false,
            self_onClose: this.onClose
        }
    },

    created() {
        this.checkProp()

        EventHub.listen('showNotif', (data) => {
            this.collectData(data)
        })
    },

    methods: {
        // single
        checkProp() {
            if (this.self_title) {
                this.self_show = true
            }

            if (this.self_duration != (undefined || null)) {
                setTimeout(() => {
                    this.closeSelf()
                }, this.self_duration * 1000)
            }
        },
        closeSelf() {
            this.self_show = false

            if (typeof this.self_onClose !== 'undefined' && typeof this.self_onClose === 'function') {
                this.self_onClose()
            }
        },

        // group
        checkForGroup() {
            return this.notif_group.length > 1 &&
                    this.notif_group.filter((item) => item.show == true).length > 1
        },
        closeAll() {
            this.notif_group.map((item) => {
                item.show = false
                item.duration = null
            })
        },
        collectData(data) {
            this.notif_group.push({
                title: data.title,
                body: data.body,
                type: data.type,
                icon: data.icon == null ? true : false,
                duration: data.duration,
                onClose: data.onClose,
                show: true
            })
        },
        IsVisible(index) {
            let dur = this.notif_group[index].duration

            if (dur != (undefined || null)) {
                setTimeout(() => {
                    this.closeNotif(index)
                }, dur * 1000)
            }

            return this.notif_group[index].show
        },
        closeNotif(index) {
            let item = this.notif_group[index]
            item.show = false

            if (typeof item.onClose !== 'undefined' && typeof item.onClose === 'function') {
                item.onClose()
            }
        },

        // helpers
        classObj(type) {
            return `notification has-shadow is-${type}`
        },
        getIcon(type) {
            switch (type) {
                case 'primary':
                    return require('./icons/baseline-track_changes-24px.svg')
                case 'success':
                    return require('./icons/baseline-check_circle-24px.svg')
                case 'info':
                    return require('./icons/baseline-live_help-24px.svg')
                case 'warning':
                    return require('./icons/baseline-power_settings_new-24px.svg')
                case 'danger':
                    return require('./icons/baseline-add_alert-24px.svg')
                default:
                    return require('./icons/baseline-error-24px.svg')
            }
        }
    }
}
</script>
