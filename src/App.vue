<template>
    <div>
        <ShowModal
            v-if="showModal"
            @close="showModal = false"
            v-bind:title="this.modal.title"
            v-bind:img="this.modal.img"
            v-bind:text="this.modal.text"
            v-bind:duration="this.modal.duration"
            v-bind:type="this.modal.type"
            v-bind:date="this.modal.date"
        />

        <!-- display movie started -->
        <div id="seen">
            <div class="row">
                <NFTCard
                    v-for="item in seen"
                    v-bind:key="item"
                    :img="item.img"
                    :name="item.name"
                    v-on:click="addToSeen(item.id)"
                />
            </div>
        </div>

        <!-- display most popular movies on tmdb -->
        <h2 id="popular-title">popular</h2>
        <div id="popular">
            <div class="row">
                <NFTCard
                    v-for="item in items_popular"
                    v-bind:key="item"
                    :img="item.img"
                    :name="item.name"
                    v-on:click="addToSeen(item.id)"
                />
            </div>
        </div>

        <!-- display movies that are actually in theaters -->
        <div id="now">
            <h2 id="now-title">now on theater</h2>
            <div class="row">
                <NFTCard
                    v-for="item in now"
                    v-bind:key="item"
                    :img="item.img"
                    :name="item.name"
                    v-on:click="addToSeen(item.id)"
                />
            </div>
        </div>

        <!-- display similar movies to fight club -->
        <div id="similar">
            <h2 id="similar-title">because you liked "Fight Club"</h2>
            <div class="row">
                <NFTCard
                    v-for="item in similar"
                    v-bind:key="item"
                    :img="item.img"
                    :name="item.name"
                    v-on:click="addToSeen(item.id)"
                />
            </div>
        </div>
    </div>
</template>

<script>
import NFTCard from "./components/NFTCard.vue";
import ShowModal from "./components/ShowModal.vue";

export default {
    name: "App",
    data: function () {
        return {
            modal: {
                title: "",
                text: "",
                img: "",
                duration: "",
                types: "",
                date: "",
            },

            modalTitle: "",
            modalText: "",
            modalImage: "",
            modalDuration: "",
            modalTypes: "",
            modalDate: "",
            showModal: false,

            seen: [],
            items_popular: [],
            now: [],
            similar: [],
        };
    },
    components: {
        NFTCard,
        ShowModal,
    },
    methods: {
        async addToSeen(id) {
            let response = await fetch(
                `https://api.themoviedb.org/3/movie/${id}?api_key=f79fcaf37fe592421bb365d473efc3ef`
            );
            let data = await response.json();

            this.modal.title = data.title;
            this.modal.text = data.overview;
            this.modal.img = `https://image.tmdb.org/t/p/original${data.poster_path}`;
            this.modal.duration = data.runtime;
            this.modal.date = data.release_date;

            this.modal.types = "";
            for (let i = 0; i < data.genres.length; i++) {
                this.modal.types += data.genres[i].name + " ";
            }

            console.log(this.modalDuration);

            this.showModal = true;
        },
        async load() {
            //retreive last seen movies
            //last seen ids
            let last_seen = [89, 550, 258509];
            //for each id get the movie
            for (let i = 0; i < last_seen.length; i++) {
                let response = await fetch(
                    `https://api.themoviedb.org/3/movie/${last_seen[i]}?api_key=f79fcaf37fe592421bb365d473efc3ef`
                );
                let data = await response.json();
                if (i == 0) {
                    document.getElementById(
                        "seen"
                    ).style.background = `linear-gradient(0deg, rgb(0, 0, 0,0.70) 10%, rgba(0, 0, 0, 0) 50%),url(https://image.tmdb.org/t/p/original${data.backdrop_path})`;
                }
                this.seen.push({
                    img: `https://image.tmdb.org/t/p/original${data.poster_path}`,
                    name: data.title,
                    id: data.id,
                });
            }

            //retreive popular movies
            let response = await fetch(
                "https://api.themoviedb.org/3/movie/popular?api_key=f79fcaf37fe592421bb365d473efc3ef"
            );
            let data = await response.json();
            data = data.results;
            for (let i = 0; i < data.length; i++) {
                this.items_popular.push({
                    img: `https://image.tmdb.org/t/p/original${data[i].poster_path}`,
                    name: data[i].title,
                    id: data[i].id,
                });
            }

            //retreive movies from box office
            response = await fetch(
                "https://api.themoviedb.org/3/movie/upcoming?api_key=f79fcaf37fe592421bb365d473efc3ef"
            );
            data = await response.json();
            data = data.results;
            for (let i = 0; i < data.length; i++) {
                if (i == 0) {
                    let div = document.getElementById("now");
                    div.style.background = `radial-gradient(at top right,rgba(0,0,0,0),rgba(0,0,0,1)75%),url(https://image.tmdb.org/t/p/original${data[i].backdrop_path})`;
                    div.style.backgroundSize = "cover";
                    div.style.backgroundPosition = "center";
                    div.style.height = "80vh";
                    div.style.display = "flex";
                }
                this.now.push({
                    img: `https://image.tmdb.org/t/p/original${data[i].poster_path}`,
                    name: data[i].title,
                    id: data[i].id,
                });
            }

            //retreive similar movies to fight club
            let similarId = 550;
            response = await fetch(
                `https://api.themoviedb.org/3/movie/${similarId}/similar?api_key=f79fcaf37fe592421bb365d473efc3ef`
            );
            data = await response.json();
            data = data.results;
            for (let i = 0; i < data.length; i++) {
                this.similar.push({
                    img: `https://image.tmdb.org/t/p/original${data[i].poster_path}`,
                    name: data[i].title,
                    id: data[i].id,
                });
            }
        },
    },
    //before dom loading retreive data
    async created() {
        await this.load();
    },
};
</script>

<style lang="css">
@import url("https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap");
::-webkit-scrollbar {
    width: 0px;
    background: transparent;
}
html {
    font-size: 62.5%;
}
* {
    transition: all 0.3s;
    margin: 0;
    overflow-x: auto;
    box-sizing: border-box;
}

body {
    background-color: #151515;
    font-family: "Outfit", sans-serif;
}

.row {
    width: 100vw;
    display: flex;
    flex-flow: column wrap;

    overflow-x: auto;
    max-height: 22vh;
}

#seen {
    background: #fff;
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: flex-end;
    align-items: flex-end;
    padding: 15px;
}
#seen > .row {
    cursor: pointer;
    margin: 15px;
    max-height: 25vh;
    flex-flow: row;
}
#popular > .row {
    margin: 0 5% 5% 5%;
    width: 90vw;
}

#popular-title {
    font-size: 3rem;
    color: #fff;
    margin: 2% 0% 2% 5%;
}
#now-title {
    font-size: 3rem;
    color: #fff;
    margin: 0% 0% 25% 5%;
}
div#now {
    display: flex;
    flex-flow: column;
    align-content: flex-start;
    height: 40vh;
    background: #fff;
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: flex-end;
    align-items: flex-start;
    padding: 15px;
    background-size: cover;
    background-position: center;
}
div#now > .row {
    width: 99%;
}
#similar {
    background: #fff;
}

#similar-title {
    font-size: 3rem;
    color: rgb(0, 0, 0);
    margin: 2% 0% 2% 2%;
}
</style>
