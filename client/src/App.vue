<template>
    <v-app>
        <v-app-bar app color="primary" dark>
            <v-toolbar-title>{{ this.$route.name }}</v-toolbar-title>
            <v-spacer />
            <v-btn v-if="$route.name === 'Charts'" @click="downloadCSV()" color="white" text>
                Download csv
            </v-btn>
        </v-app-bar>

        <navigation-drawer :items="items" />

        <v-main>
            <v-container fluid>
                <vue-page-transition name="fade">
                    <router-view />
                </vue-page-transition>
            </v-container>
        </v-main>
    </v-app>
</template>

<script lang="ts">
/* eslint-disable */
import { Component, Vue } from 'vue-property-decorator';
import { NavItems } from './types';
import NavigationDrawer from './components/NavigationDrawer.vue';

@Component({
    components: {
        NavigationDrawer,
    },
})
export default class App extends Vue {
    items: NavItems[] = [
        { title: 'Charts', icon: 'mdi-poll', link: '/' },
        { title: 'Map', icon: 'mdi-map', link: '/map' },
        { title: 'ATT - Orange', icon: 'mdi-web', link: '/att' },
    ];

    // @ts-ignore
    public downloadCSV = () => window.open(`${this.$REST_URL}/api/download`, '_blank');
}
</script>

<style lang="scss">
body {
    padding: 0;
    margin: 0;
}
html,
body {
    height: 100%;
    width: 100%;
}
</style>
