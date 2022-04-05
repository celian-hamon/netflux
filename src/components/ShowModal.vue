<template>
    <div class="modal">
        <div class="movie_card" id="bright">
            <div class="info_section">
                <div class="movie_header">
                    <img class="locandina" :src="img" />
                    <h1>{{ title }}</h1>
                    <h4>{{ date }}</h4>
                    <span class="minutes">{{ duration }}minutes</span>
                    <p class="type">{{ type }}</p>
                </div>
                <div class="movie_desc">
                    <p class="text">
                        {{ text }}
                    </p>
                </div>
                <button
                    style="float: right; margin: 0 5% 10% 0"
                    @click="$emit('close')"
                >
                    close
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        title: {
            type: String,
            default: "",
            required: true,
        },
        img: {
            type: String,
            default: "",
            required: true,
        },
        text: {
            type: String,
            default: "",
            required: true,
        },
        duration: {
            type: Number,
            default: "",
            required: true,
        },
        type: {
            type: String,
            default: "",
            required: true,
        },
        date: {
            type: String,
            default: "",
            required: true,
        },
    },
};
</script>

<style lang="scss">
.modal {
    display: flex;
    justify-content: center;
    align-items: center;

    position: fixed; /* Stay in place */
    z-index: 30000000000; /* Sit on top */

    left: 0;
    top: 0;
    width: 100%; /* Full width */
    height: 100%; /* Full height */

    animation: spawn 0.3s;
    background: rgba(0, 0, 0, 0.353);
}

.link {
    display: block;
    text-align: center;
    color: #777;
    text-decoration: none;
    padding: 10px;
}

.movie_card {
    position: relative;
    display: block;
    background: rgba(0, 0, 0, 0.5);
    width: 50vw;
    height: 50%;
    margin: 80px auto;
    overflow: hidden;
    border-radius: 10px;
    transition: all 0.4s;
    box-shadow: 0px 0px 120px -25px rgba(0, 0, 0, 0.5);
    &:hover {
        transform: scale(1.02);
        box-shadow: 0px 0px 80px -25px rgba(0, 0, 0, 0.5);
        transition: all 0.4s;
    }
    .info_section {
        position: relative;
        width: 100%;
        height: 100%;
        background-blend-mode: multiply;
        z-index: 2;
        border-radius: 10px;
        .movie_header {
            position: relative;
            padding: 25px 25px 0 25px;
            height: 45%;
            h1 {
                font-size: 4rem;
                font-weight: bold;
                color: rgb(255, 255, 255);
            }
            h4 {
                font-size: 2rem;
                color: rgb(209, 209, 209);
            }
            .minutes {
                display: inline-block;
                margin-top: 15px;
                font-size: 2rem;
                color: rgb(209, 209, 209);
            }
            .type {
                font-size: 2rem;
                display: inline-block;
                margin-left: 10px;
                color: rgb(209, 209, 209);
            }
            .locandina {
                position: relative;
                float: left;
                margin-right: 20px;
                height: 120px;
                box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.5);
            }
        }
        .movie_desc {
            padding: 0 25px 25px 25px;
            height: 50%;
            .text {
                font-size: 2.7rem;
                color: #f7f7f7;
            }
        }
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
