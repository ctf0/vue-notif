<template>
    <transition name="slide-fade">
        <div class="notification is-stacked has-shadow" :class="classObj" v-if="show">
            <h4 class="title is-4">{{ self_title }}</h4>
            <button class="delete" @click="dismiss"></button>
            <p>{{ self_body }}</p>
        </div>
    </transition>
</template>
<style scoped>
    /*animation*/
    .slide-fade-enter-active,
    .slide-fade-leave-active {
        transition: all 0.3s ease;
    }
    .slide-fade-enter,
    .slide-fade-leave-active {
        opacity: 0;
        transform: translateX(10px);
    }

    /*notiifcation card*/
    .is-stacked {
        width: 330px;
        float: right;
        clear: right;
    }
    .has-shadow {
        box-shadow: 0 2px 4px rgba(0,0,0,0.12), 0 0 6px rgba(0,0,0,0.04);
    }
</style>
<script>
    export default {
        props: {
            title: {type: String},
            body: {type: String},
            type: {default: 'info'},
            duration: {required: false}
        },

        data() {
            return {
                show: true,
                onClose: null,
                self_title: this.title,
                self_body: this.body,
                self_type: this.type,
                self_duration: this.duration,
            };
        },

        created() {
            this.isEmpty();

            EventHub.listen('showNotif', (data)=>{
                this.collectData(data)
            })
        },

        methods: {
            dismiss() {
                this.show = false;
                if (typeof this.onClose === 'function') {
                    this.onClose();
                }
            },

            collectData(data){
                this.self_title = data.title
                this.self_body = data.body
                this.self_type = data.type
                this.self_duration = data.duration
                this.onClose = data.onClose
                this.show = true;

                setTimeout(()=>{
                    this.show = false;
                    if (typeof this.onClose === 'function') {
                        this.onClose();
                    }
                }, this.timer);
            },

            isEmpty(){
                // hide notification if its empty
                if (!this.self_title) {
                    this.show = false;
                }
            },
        },

        computed: {
            timer() {
                return this.self_duration * 1000;
            },
            classObj(){
                return `is-${this.self_type}`
            }
        }
    }
</script>
