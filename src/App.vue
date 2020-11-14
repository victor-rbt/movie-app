<template>
    <div class="app">
        <div class="search-field">
            <input type="text" placeholder="Search by title of movie.." v-model="movie_name">
            <button type="search" @click="search">Search</button>
        </div>

        <div class="errors" v-if="dados.Response == 'False'">
            <p style="color: red;">{{ dados.Error }}</p>
        </div>

        <div class="box-results" v-if="dados.Response == 'True'">
            <figure class="poster">
                <img :src="dados.Poster" alt="Poster" :title="dados.Title" width="150px" height="200px">
            </figure>
            <div class="data">
                <div class="title">
                    <strong>{{ dados.Title }}</strong>
                </div>

                <div class="description">
                    <p>Descript: <span>{{ dados.Plot }}</span></p>
                </div>

                <div class="release-date">
                    <div class="date">
                        <p>Release Date: <span>{{ dados.Released }}</span></p>
                    </div>
                    <div class="count" v-if="countDown">
                        <p>Countdown: <span>{{ countDown }}</span></p>                     
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data(){
        return {
        'api_url': 'http://www.omdbapi.com/',
        'api_key': 'aaf0f6ce',
        'movie_name': '',
        'dados': {},
        'countDown': ''
        }
    },
    methods: {
        search() {
            axios.get(`${this.api_url}?t=${this.movie_name}&plot=full&apikey=${this.api_key}`)
            .then(result => {return result.data})
            .then(this.setData)
            .catch((err) => {console.log('Error: ' + err)})
        },
        setData(result) {
            this.setCount(result.Released)
            this.dados = result
        },
        setCount(date) {
            const currentDate = new Date()
            const newDate = new Date(date)
            const difference = newDate - currentDate
            const days = Math.floor(difference / 1000 / 60 / 60 / 24)
            this.countDown = days < 0 ? '' : days + ' days'
        }
    }
}
</script>

<style>
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    list-style: none;
    text-decoration: none;
    outline: none;
    font-family: 'Roboto', sans-serif;
}
body{
    width: 100%;
    height: 100vh;
    background: url(./media/bg.jpg);
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
}
.app{
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: rgba(0, 0, 0, 0.3);
}
.search-field{
    width: 100%;
    padding: 3%;
    display: flex;
    justify-content: center;
}
.search-field input, button{
    padding: 10px;
    border: None;
    box-shadow: 0 3px 3px rgba(0, 0, 0, 0.2.5);
}
.search-field input{
    width: 350px;
    border-radius: 5px 0 0 5px;
}
.search-field button{
    border-radius: 0 5px 5px 0;
    cursor: pointer;
    background-color: #111;
    color: #FFF;
}
.box-results{
    width: 60%;
    display: flex;
    justify-content: center;
    background-color: #FFF;
    padding: 2%;
    border-radius: 5px;

}
.poster{
    flex-basis: 15%;
}
.poster img{
    box-shadow: 1px 3px 4px rgba(0, 0, 0, 0.3);
    border-radius: 5px;
}
.data{
    margin-left: 10px;
    flex-basis: 83%;
    display: flex;
    flex-direction: column;
}
.data p{
    color: red;
}
.data span{
    color: #111;
}
.title{
    margin: 10px 5px;
    font-size: 24px;
    letter-spacing: 2px;
    color: #111;
}
.description{
    font-size: 13px;
    margin: 5px 0;
}
.release-date{
    display: flex;
    margin-top: 5px;
    font-size: 12px;
}
.date{
    margin-right: 12px;
}

@media screen and (max-width: 768px){
    .search-field input, button{
        padding: 18px;
    }
    .box-results{
        width: 90%;
        align-items: center;
        flex-direction: column;
    }
}

</style>
