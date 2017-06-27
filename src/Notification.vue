<template>
    <div>
        <!-- single -->
        <transition name="slide-fade" v-if="self_show">
            <ul>
                <li :class="classObj(self_type)">
                    <h4 class="title is-4">{{ self_title }}</h4>
                    <button class="delete" @click="self_show = false"></button>
                    <p>{{ self_body }}</p>
                </li>
            </ul>
        </transition>

        <!-- events -->
        <transition-group name="slide-fade" tag="ul" v-if="!self_title">
            <li v-for="(one,index) in notif_group"
                :key="one"
                :class="classObj(one.type)"
                v-if="IsVisible(index)">
                <h4 class="title is-4">{{ one.title }}</h4>
                <button class="delete" @click="closeNotif(index)"></button>
                <p>{{ one.body }}</p>
            </li>
        </transition-group>
    </div>
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
    li {
        width: 330px;
    }
    .notification {
        margin-bottom: 10px;
    }
    .has-shadow {
        box-shadow: 0 2px 4px rgba(0,0,0,0.12), 0 0 6px rgba(0,0,0,0.04);
    }

    p {
        word-break: break-all;
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
                notif_group: [],
                self_title: this.title,
                self_body: this.body,
                self_type: this.type,
                self_duration: this.duration,
                self_show: false
            };
        },

        created() {
            this.checkProp();

            EventHub.listen('showNotif', (data)=>{
                this.collectData(data)
            })
        },

        methods: {
            checkProp(){
                if (this.self_title) {
                    this.self_show = true;
                }

                if (this.self_duration !== undefined) {
                    setTimeout(()=>{
                        this.self_show = false;
                    }, this.self_duration * 1000);
                }
            },

            collectData(data){
                this.notif_group.push({
                    title: data.title,
                    body: data.body,
                    type: data.type,
                    duration: data.duration,
                    onClose: data.onClose,
                    show: true
                })
            },

            IsVisible(index){
                const dur = this.notif_group[index].duration;

                if (dur !== undefined) {
                    setTimeout(()=>{
                        this.closeNotif(index);
                    }, dur * 1000);
                }

                return this.notif_group[index].show;
            },

            closeNotif(index) {
                if (this.notif_group[index].onClose !== undefined && typeof this.notif_group[index].onClose === 'function') {
                    this.notif_group[index].onClose();
                }

                this.notif_group[index].show = false;
                this.$delete(this.notif_group,index);
            },

            classObj(type){
                return `notification has-shadow is-${type}`
            }
        }
    }
</script>
