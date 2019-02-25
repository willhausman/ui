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

    methods: {
        dispatch(list, action) {
            if (!list || !list.length) {
                this.isLoading = true;
                return this.$store.dispatch(action);
            }
            return undefined;
        },
    },

    mounted() {
        Promise.all([
            this.dispatch(this.heroList, 'hero/getList'),
            this.dispatch(this.artifactList, 'artifact/getList'),
        ]).then(() => {
            this.isLoading = false;
        });
    },
};
</script>
