<template>
    <div class="search">
        <input type="text" class="search-input" v-model="searchText" />
        <ResultsList />
    </div>
</template>

<script>
import { mapGetters } from 'vuex';
import ResultsList from '@/js/components/search/ResultsList';

export default {
    data() {
        return {
            isLoading: false,
            searchText: '',
        };
    },
    components: {
        ResultsList,
    },
    computed: {
        ...mapGetters('hero', { heroList: 'list' }), // this.heroList
        ...mapGetters('artifact', { artifactList: 'list' }), // this.artifactList
        filteredHeroes() {
            return this.heroList && this.heroList.filter((h) => h.name.indexOf(this.searchText) > -1);
        },
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
