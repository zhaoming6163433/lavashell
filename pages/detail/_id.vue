<template>
    <div class="detail-wrapper">
        <v-layout row wrap>
            <v-flex xs10 offset-xs1>
                <article class="detail-content text-xs-center" v-html="detail.content">

                </article >
            </v-flex>
        </v-layout>
    </div>
</template>

<script>
import { health_details } from '@/model/api.js'
let state = {
    appHeaderState: {
        show: true,
        title: 'Lavas',
        showMenu: false,
        showBack: true,
        showLogo: false,
        actions: [
            {
                icon: 'home',
                route: {
                    name: 'index'
                }
            }
        ]
    }
};

function setState(store) {
    store.dispatch('appShell/appHeader/setAppHeader', state.appHeaderState);
}

export default {
    name: 'detailId',
    data() {
        return {
            detail: {}
        }
    },
    metaInfo() {
        return {
            title: `Detail ${this.$route.params.id}`,
            titleTemplate: '%s - Lavas',
            meta: [
                { name: 'keywords', content: 'lavas PWA' },
                { name: 'description', content: '基于 Vue 的 PWA 解决方案，帮助开发者快速搭建 PWA 应用，解决接入 PWA 的各种问题' }
            ]
        }
    },
    methods: {
        async typedetail(id) {
            try {
                let res = await health_details({id:this.$route.params.id});
                this.detail = res;
                console.log(this.detail)
                //this.list = res.list;
                //util.vueEvent.$emit("disloadingpage");
            } catch (err) {
                //util.vueEvent.$emit("disloadingpage");
                console.log(err)
            }
        }
    },
    async asyncData({ store, route }) {
        setState(store);
        await new Promise((resolve, reject) => {
            setTimeout(resolve, 500);
        });
    },
    mounted() {
        this.typedetail();
    },
    activated() {
        setState(this.$store);
    }
};
</script>

<style lang="stylus" scoped>
.detail-content {
    font-size: 16px;
    line-height: 30px;
    margin-top: 30px;

    .detail-title {
        margin-bottom: 20px;
        padding: 10px 0;
        font-size: 36px;
        font-weight: bold;
    }
}
</style>
