<template>
    <the-header />
    <main>
        <div class="pageMain_inputs container">
            <div class="d-flex py-3">
                <h6 class="m-auto font-weight-bold text-primary">
                    <span>
                        <BIconPencilFill />
                        地名・住所を入力
                    </span>
                </h6>
                <button class="btn btn-info btn-sm px-3" @click="showResult()">
                    <span>
                        <BIconSearch />
                        検索
                    </span>
                </button>
            </div>
            <div class="row">
                <text-input id="input1" placeholder="地点①" />
                <text-input id="input2" placeholder="地点②" />
                <text-input id="input3" placeholder="地点③" />
                <text-input id="input4" placeholder="地点④" />
            </div>
        </div>
        <div class="pageMain_map container">
            <div id="map" />
        </div>
    </main>
    <app-loading v-if="isLoading" />
</template>

<script>
import GoogleMapsApiLoader from 'google-maps-api-loader'
import AppLoading from './AppLoading'
import TheHeader from './TheHeader'
import TextInput from './TextInput'
import { googleApiKey } from '../google/config'
import { BIconPencilFill, BIconSearch } from 'bootstrap-icons-vue'

export default {
    components: { AppLoading, BIconPencilFill, BIconSearch, TheHeader, TextInput },

    data () {
        return {
            google: null,
            map: null,
            isLoading: false
        }
    },
    async mounted () {
        this.google = await GoogleMapsApiLoader({
            apiKey: googleApiKey
        })
        this.map = new this.google.maps.Map(document.getElementById('map'), {
            center: {
                lat: 35.680959,
                lng: 139.767306
            },
            zoom: 10
        })
    },
    methods: {
        closeLoading () {
            this.isLoading = false
        },

        getInputPlaces () {
            const inputPlaces = []
            const inputs = ['input1', 'input2', 'input3', 'input4']
            for (let i = 0; i < inputs.length; i++) {
                const place = document.getElementById(inputs[i]).value
                if (place) {
                    inputPlaces.push(place)
                }
            }
            return inputPlaces
        },

        geocodePlaces (showResult, closeLoading) {
            const places = this.getInputPlaces()
            if (places.length < 2) {
                alert("２つ以上入力して下さい。")
                return false
            }
            const locations = []
            const geocoder = new this.google.maps.Geocoder()
            this.isLoading = true
            for (let i = 0; i < places.length; i++) {
                geocoder.geocode({address: places[i]}, function (results, status) {
                    if (status === 'OK') {
                        const locationData = {
                            place: places[i],
                            address: results[0].formatted_address,
                            lat: results[0].geometry.location.lat(),
                            lng: results[0].geometry.location.lng()
                        }
                        locations.push(locationData)
                        showResult(locations)
                    } else {
                        alert("「" + places[i] + "」" + "は正しい地名ではありません。")
                        closeLoading()
                        return false
                    }
                })
            }
        },

        getMiddleLatLng (locations) {
            let latSum = 0
            let lngSum = 0

            // 検索地点の各緯度・経度の合計値取得
            for (let i = 0; i < locations.length; i++) {
                latSum += locations[i].lat
                lngSum += locations[i].lng
            }

            // 緯度・経度の各平均値を設定
            const middleLatLng = {
                lat: latSum / locations.length,
                lng: lngSum / locations.length
            }
            return middleLatLng
        },

        showResultMarker (middleLatLng) {
            const googleMap = this.google.maps

            // 地図の中央に中間地点をセット
            this.map = new googleMap.Map(document.getElementById('map'), {
                center: middleLatLng
            })
            // 中間地点にマーカーをセット
            const resultMarker = new googleMap.Marker({
                position: middleLatLng,
                animation: googleMap.Animation.DROP,
                map: this.map,
            })

            // 中間地点にウィンドウを表示
            const resultInfoWindow = new googleMap.InfoWindow({
                content: '<span style="color: red; font-size: 15px; font-weight: bold;">中間地点</span>'
            })
            resultInfoWindow.open(this.map, resultMarker)
            resultMarker.addListener('click', function () {
                resultInfoWindow.open(this.map, resultMarker)
            })
        },

        showInputMarkers (locations) {
        // 検索地点を青マーカーで表示し、画面内に収める
            const googleMap = this.google.maps
            const bounds = new googleMap.LatLngBounds()
            const blueIcon = 'http://maps.google.com/mapfiles/ms/icons/blue-dot.png'
            for (let i = 0; i < locations.length; i++) {
                const inputMarker = new googleMap.Marker({
                    position: {lat: locations[i].lat, lng: locations[i].lng},
                    animation: googleMap.Animation.DROP,
                    map: this.map,
                    icon: blueIcon
                })
                bounds.extend(inputMarker.position)

                // 青マーカークリック時に検索地点名表示
                const inputsInfoWindow = new googleMap.InfoWindow()
                inputMarker.addListener('click', function () {
                    inputsInfoWindow.setContent(
                        '<a href=http://www.google.com/search?q=' + locations[i].place + ' target="_blank" style="font-weight: bold">' + locations[i].place
                    )
                    inputsInfoWindow.open(this.map, inputMarker)
                })
            }
            this.map.fitBounds(bounds)
        },

        showResultMap (locations) {
            const middleLatLng = this.getMiddleLatLng(locations)
            this.showResultMarker(middleLatLng)
            this.showInputMarkers(locations)
            this.closeLoading()
        },

        showResult () {
            this.geocodePlaces(this.showResultMap, this.closeLoading)
        }
    }
}
</script>

<style scoped>
main {
    max-width: 900px;
    margin: auto;
    padding: 5vh 20px 0;
    text-align: center;
}

.btn {
    width: 150px;
}

.pageMain_inputs {
    margin: 5vh auto;
    box-shadow: 0px 0px 5px 0px #555;
    background: aliceblue;
}

.pageMain_map {
    height: 60vh;
    margin: 15px auto;
    padding: 10px;
    background: aquamarine;
    border-radius: 10px;
    box-shadow: 0px 0px 5px #555;
}

#map {
    min-height: -webkit-fill-available;
}

@media screen and (max-width: 767px) {
    .btn {
        width: auto;
    }
    .pageMain_inputs {
        margin: 4vh auto;
    }
}

@media screen and (max-width: 400px) {
    h5 {
        font-size: unset;
    }
}
</style>