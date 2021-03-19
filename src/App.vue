<template>
    <div id="app">
        <header class="header">
            <h1 class="header__title">Comics</h1>
        </header>

        <section class="content">
            <p v-if="isLoading">Cargando datos...</p>
            <Comic
                v-else
                :title="title"
                :image-url="imageUrl"
                :image-alt="imageAlt"
                :comic-number="comicNumber"
                :date="date"
                :rating="rating"
                :set-rating="setRating"
            />
        </section>

        <footer class="footer">
            <p class="footer_content">
                Build by <a href="https://github.com/javiergomezve" target="_blank">javiergomezve</a> for <a target="_blank" href="https://masiv.com/">masivian</a>
            </p>
        </footer>
    </div>
</template>

<script>
import Comic from './components/Comic.vue'

export default {
    name: 'App',
    components: {
        Comic
    },
    data() {
        return {
            isLoading: false,
            title: '',
            imageUrl: '',
            imageAlt: '',
            comicNumber: 0,
            date: '',
            rating: 0,
        };
    },
    computed: {
        comic_storage: function () {
            return `masivian_comic_${this.comicNumber}`;
        }
    },
    mounted() {
        this.fetchComic();
    },
    methods: {
        setRating(rating) {
            const vm = this;
            localStorage.setItem(vm.comic_storage, rating);
        },

        getRating() {
            const vm = this;
            const rate = localStorage.getItem(vm.comic_storage);

            if (rate) {
                vm.rating = parseInt(rate);
            }
        },

        fetchComic() {
            const vm = this;

            vm.isLoading = true;
            vm.axios.get('https://masivia-back-test.herokuapp.com/api/comic')
                .then(function(response) {
                    const comic = response.data;
                    vm.title = comic.title;
                    vm.imageAlt = comic.alt;
                    vm.imageUrl = comic.img;
                    vm.comicNumber = comic.num;
                    vm.date = `${comic.month}/${comic.day}/${comic.year}`;

                    vm.getRating();

                    vm.isLoading = false;
                })
                .catch(function (error) {
                    console.error(error);
                });
        },
    }
}
</script>

<style lang="scss">
html {
    height: 100%;
}

body {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    background-color: #e3e3e3;
    color: #fff;
    height: 100%;
    font-family: 'Comic Neue', cursive;
}

a {
    color: #00AFF4;
    text-decoration: none;

    &:hover {
        text-decoration: underline;
    }
}

#app {
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;

    .header, .footer {
        padding: 10px 0;
        width: 100%;
        background: #0C1F55;
        text-align: center;
    }

    .content {
        flex: 1;
        display: flex;
        justify-content: center;
        align-items: center;
    }
}
</style>
