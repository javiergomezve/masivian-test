<template>
    <article class="comic">
        <h2 class="comic__title">{{title}}</h2>
        <img class="comic__image" v-bind:src="imageUrl" v-bind:alt="imageAlt" />
        <div class="comic__meta">
            <p class="comic__number">Comic #{{ comicNumber }}</p>
            <p class="comic__date">Date {{ date }}</p>
        </div>
        <div class="comic_classification">
            <star-rating
                v-bind:star-size="20"
                inactive-color="#e3e3e3"
                v-bind:rating="rating"
                @rating-selected ="setRating"
            />
        </div>
    </article>
</template>

<script>
import StarRating from 'vue-star-rating'

export default {
    name: 'Comic',
    components: {
        StarRating
    },
    props: {
        title: String,
        imageUrl: String,
        imageAlt: String,
        comicNumber: Number,
        date: String,
        rating: Number,
        setRating: Function,
    }
}
</script>

<style lang="scss">
.comic {
    display: flex;
    flex-direction: column;
    align-items: center;
    background: #70E2BF;
    padding: 10px 20px;

    &__title {
        margin: 0 0 10px;
    }

    &__image {
        width: 100%;
    }

    &__meta {
        display: flex;
        flex-direction: row;
        justify-content: space-around;
        align-items: center;
        width: 100%;
    }

    &__number, &__date {
        margin: 10px 0 0;
        flex: 1;
        text-align: center;
    }

    &_classification {
        .vue-star-rating {
            margin: 10px 0 5px;
            height: 1rem;
        }
    }
}
</style>
