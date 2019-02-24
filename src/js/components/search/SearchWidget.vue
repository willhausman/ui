<template>
    <div class="search">
        <input type="text" class="search-input" :value="searchText" />
    </div>
</template>

<script>
import { mapGetters } from 'vuex';
// import ResultsList from '@/js/components/search/ResultsList';

export default {
    data() {
        return {
            isLoading: false,
        };
    },
    // components: {
    //     ResultsList,
    // },
    computed: {
        ...mapGetters('hero', { heroList: 'list' }), // this.heroList
        ...mapGetters('artifact', { artifactList: 'list' }), // this.artifactList
    },

    mounted() {
        const listsWaiting = [];

        if (!this.heroList || !this.heroList.length) {
            this.isLoading = true;
            listsWaiting.push(this.$store.dispatch('hero/getList'));
        }

        if (!this.artifactList || !this.artifactList.length) {
            this.isLoading = true;
            listsWaiting.push(this.$store.dispatch('artifact/getList'));
        }

        Promise.all(listsWaiting).then(() => {
            this.isLoading = false;
        });
    },
};
</script>
