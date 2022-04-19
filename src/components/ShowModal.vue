<template>
    <div
        class="modal"
        id="modal"
        :style="{
            background: `linear-gradient(0deg, rgb(0, 0, 0,0.95) 0%, rgba(0, 0, 0, 0) 70%),url(https://image.tmdb.org/t/p/original${this.movie.backdrop_path})`,
            'background-size': 'cover',
            'background-position': 'center',
        }"
    >
                <button
                    @click="$emit('close')"
                >
                    <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 0 24 24" width="24px" fill="#000000"><path d="M0 0h24v24H0V0z" fill="none" opacity=".87"/><path d="M17.51 3.87L15.73 2.1 5.84 12l9.9 9.9 1.77-1.77L9.38 12l8.13-8.13z"/></svg>
                </button>
        <div class="movie_card" id="bright">
            <div class="movie_header">
                <img
                    class="locandina"
                    :src="
                        'https://image.tmdb.org/t/p/original' +
                        movie.poster_path
                    "
                />
            </div>
            <div class="movie_desc">
                <h1>{{ movie.title }}</h1>
                <h4>{{ movie.release_date }}</h4>
                <span class="minutes">{{ movie.runtime }}minutes || {{ this.type }}</span>
                <p class="text">
                    {{ movie.overview }}
                </p>

            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        type: {
            type: String,
            default: "",
            required: true,
        },
        movie: {
            type: Object,
            required: true,
            default: {},
        },
    },
    computed: {
        cssProps() {
            return {
                "--back":
                    "https://image.tmdb.org/t/p/original" +
                    this.movie.backdrop_path,
            };
        },
    },
};
</script>

<style lang="scss">
button{
    background: rgb(45, 45, 45);
    border: none;

top:0;
    left: 0;
    border-radius: 50px;
    z-index: 100;
    margin:15px;
    width: 50px;
    height: 50px;
    position: absolute;
    svg{
        width: 25px;
        transform: translateX(-2px);
        height: 25px;
        fill: rgb(180, 180, 180);
    }
}

.modal {
    display: flex;
    justify-content: center;
    align-items: flex-end;

    position: fixed; /* Stay in place */
    z-index: 3; /* Sit on top */

    left: 0;
    top: 0;
    width: 100%; /* Full width */
    height: 100%; /* Full height */

    animation: spawn 0.3s;
}
.movie_header{
    img{
        border-radius: 10px; ;
    }
}

.movie_card {
    display: flex;
    padding: 30px;
    width: 100vw;
    flex-direction: row;
    justify-content: flex-start;
}

img.locandina {
    height: 400px;
}
.movie_desc {
    margin-left: 20px;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    width: 400px;
    color: white;
    h1 {
        font-size: 5rem;
        margin: 0;
    }
    h4 {
        font-size: 2rem;
    }
    span.minutes{
        font-size: 1.5rem;
        color: rgba(255, 255, 255, 0.825);
    }
    p {
        font-size: 1.5rem;
    }
}

@keyframes spawn {
    0% {
        opacity: 0;
    }
    100% {
        opacity: 1;
    }
}
</style>
