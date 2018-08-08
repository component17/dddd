<template>
    <v-layout justify-space-around row fill-height>
        <v-flex xs6 style="background: red">
            <webview class="ya-map" ref="yaMap" src="https://yandex.ru/pogoda/moscow/maps/nowcast?le_Lightning=0&ll=37.669420_55.769536&z=8" :preload="yaMapPreloader"></webview>
        </v-flex>
        <v-flex xs6>
            <div class="pa-2">


                <v-layout row wrap>

                    <v-flex xs12 class="ccc">
                        <clock/>
                    </v-flex>

                    <v-flex xs6 class="pr-1">
                        <v-card color="blue-grey darken-2" class="white--text">
                            <v-card-media
                                    src="http://img2.ntv.ru/home/news/20170213/euro_vs.jpg"
                                    height="200px"
                            ></v-card-media>
                            <h1 class="text-xs-center" style="font-size: 72px">{{curEUR}}</h1>
                        </v-card>
                    </v-flex>

                    <v-flex xs6 class="pl-1">
                        <v-card color="blue-grey darken-2" class="white--text">
                            <v-card-media
                                    src="https://tengrinews.kz/userdata/news/2018/news_350718/thumb_xm/photo_256911.jpg"
                                    height="200px"
                            ></v-card-media>
                            <h1 class="text-xs-center" style="font-size: 72px">{{curUSD}}</h1>
                        </v-card>
                    </v-flex>

                    <v-flex xs12 class="mt-2">
                        <v-card color="blue-grey darken-2" class="white--text pa-2">
                            <v-layout row wrap>
                                <v-flex xs6>
                                    <h1 style="font-size: 56px">Баланс Jelastic: </h1>
                                </v-flex>

                                <v-flex xs6 style="display: flex; justify-content: flex-end">
                                    <h1 style="font-size: 56px">6 800.00 руб.</h1>
                                </v-flex>
                            </v-layout>
                        </v-card>
                    </v-flex>

                    <v-flex xs12 class="mt-2">
                        <v-card color="blue-grey darken-2" class="white--text">
                            <v-card-media
                                    :src="currentNews.urlToImage"
                                    height="390px"
                            >
                                <v-container
                                        fluid
                                        pa-2
                                >
                                    <v-layout>
                                        <v-flex xs12 align-end flexbox class="pa-2" style="background: rgba(0,0,0,0.71); border-radius: 5px">
                                            <span class="headline white--text">{{currentNews.title}}</span>
                                            <p class="white--text mt-1">{{currentNews.description}}</p>
                                            <p class="grey--text"><hr>{{currentNews.source.name}}</p>
                                        </v-flex>
                                    </v-layout>
                                </v-container>
                            </v-card-media>
                        </v-card>
                    </v-flex>

                </v-layout>

            </div>
        </v-flex>
    </v-layout>
</template>

<script>
    import axios from "axios"
    import clock from './clock'

    const path = require('path');

    export default {
        name: 'landing-page',
        components: {
            clock
        },
        data(){
            return {
                yaMapPreloader: 'file:///Users/evgenijspilka/Documents/dev/upoint/dashboard/static/yaMapPreloader.js',
                curEUR: 0,
                curUSD: 0,
                currentNews: null,
                currentNewsIndex: 1,
                news: [],
            }
        },
        watch: {
            currentNewsIndex(state){
                this.currentNews = this.news[state];
            }
        },
        created(){
            this.getCurrenty();
            this.getNews();

            setInterval(() => {
                if(this.currentNewsIndex > 19){
                    this.getNews();
                }
                else{
                    this.currentNewsIndex = this.currentNewsIndex + 1;
                }
            }, 1000 * 60 * 5)
        },
        mounted(){

            setInterval(() => {
                this.$refs.yaMap.reload();
                this.getCurrenty();
            }, 1000 * 60 * 30);

        },
        methods: {
            getCurrenty(){
                axios.get('https://www.cbr-xml-daily.ru/daily_json.js').then((res) => {
                    this.curEUR = res.data.Valute["EUR"].Value.toFixed(2);
                    this.curUSD = res.data.Valute["USD"].Value.toFixed(2);
                })
            },
            getNews(){
                axios.get('https://newsapi.org/v2/top-headlines?country=ru&apiKey=0df1684aea9c48499d10cbd6cb1075ff').then((res) => {
                    this.news = res.data.articles;
                    this.currentNewsIndex = 0;
                })
            }
        }
    }
</script>

<style>
    .ccc {
        display: flex;
        justify-content: flex-end;
    }
    .ya-map{
        height: 100%;
    }
</style>
