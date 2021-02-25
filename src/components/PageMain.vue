<template>
    <the-header />
    <main>
        <div class="pageMain_inputs container">
            <div class="d-flex py-3">
                <h5 class="m-auto font-weight-bold text-primary">
                    <span><BIconSearch /></span>
                    地点入力
                    <span><BIconSearch /></span>
                </h5>
                <button class="btn btn-info btn-sm px-3" id="search">
                    <span><BIconSearch /></span>
                    検索
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
</template>

<script>
import GoogleMapsApiLoader from 'google-maps-api-loader'
import { BIconSearch } from 'bootstrap-icons-vue'
import TheHeader from './TheHeader'
import TextInput from './TextInput'

export default {
    components: {
       TheHeader, TextInput, BIconSearch
    },
    data () {
        return {
            google: null,
            map: null
        }
    },
    async mounted () {
        this.google = await GoogleMapsApiLoader({
            apiKey: ''
        })
        this.map = new this.google.maps.Map(document.getElementById('map'), {
            center: {
                lat: 35.680959,
                lng: 139.767306
            },
            zoom: 10
        })
        this.initMap()
    },
    methods: {
        initMap () {
            let locations = []
            const geocoder = new this.google.maps.Geocoder()
            // 検索クリック後
            document.getElementById('search').addEventListener('click', function () {
                // 配列初期化
                locations = []
                // 取得した地点の値をgeocodingしlocationsに収納
                function getLocation (inputId) {
                    let place = document.getElementById(inputId).value
                    geocoder.geocode({address: place}, function (results, status) {
                        if (status === this.google.maps.GeocoderStatus.OK) {
                            let location = []
                            // 入力値を取得
                            location.push(place)
                            // 緯度経度を取得
                            let latlng = results[0].geometry.location
                            location.push(latlng)
                            // 住所を取得
                            let address = results[0].formatted_address
                            location.push(address)
                            locations.push(location)
                        }
                    })
                }
                // input1~4のgeocoding
                getLocation('input1')
                getLocation('input2')
                getLocation('input3')
                getLocation('input4')

                // マーカー設置処理
                setTimeout(function () {
                    let marker
                    let infoWindow
                    let latSum = 0
                    let lngSum = 0
                    if (locations.length >= 2) {
                        // 検索数に応じ、中間地点の緯度・経度を算出し、赤マーカーで表示
                        for (let i = 0; i < locations.length; i++) {
                        // 検索地点の各緯度・経度の合計値を取得
                            latSum += parseFloat(locations[i][1].lat())
                            lngSum += parseFloat(locations[i][1].lng())
                        }
                        // 緯度・経度の各平均値を算出
                        let middleLat = parseFloat(latSum) / locations.length
                        let middleLng = parseFloat(lngSum) / locations.length
                        // 平均値を中間地点とし、地図の中央にマーカー表示
                        this.map = new this.google.maps.Map(document.getElementById('map'), {
                            center: {lat: middleLat, lng: middleLng}
                        })
                        marker = new this.google.maps.Marker({
                            position: {lat: middleLat, lng: middleLng},
                            map: this.map
                        })
                        infoWindow = new this.google.maps.InfoWindow({
                            content: '<span style="color: red; font-size: 15px; font-weight: bold;">中間地点</span>'
                        })
                        infoWindow.open(this.map, marker)
                        marker.addListener('click', function() {
                            infoWindow.open(this.map, marker)
                        });
                        // 検索地点を青マーカーで表示し、画面内に収める
                        let bounds = new this.google.maps.LatLngBounds()
                        for (let i = 0; i < locations.length; i++) {
                            let iconUrl = 'http://maps.google.com/mapfiles/ms/icons/blue-dot.png'
                            let locationsMarker = new this.google.maps.Marker({
                                position: new this.google.maps.LatLng(locations[i][1].lat(), locations[i][1].lng()),
                                map: this.map,
                                icon: iconUrl
                            })
                            bounds.extend(locationsMarker.position)
                            // 検索地点の詳細をマーカークリック時に表示
                            let locationsInfoWindow = new this.google.maps.InfoWindow()
                            this.google.maps.event.addListener(locationsMarker, 'click', (function (locationsMarker, i) {
                                return function () {
                                    locationsInfoWindow.setContent('<a href=http://www.google.com/search?q=' + locations[i][0] + ' target="_blank">' + locations[i][0] + '</a><br><br>' + locations[i][2])
                                    locationsInfoWindow.open(this.map, locationsMarker)
                                }
                            })(locationsMarker, i))
                        }
                        this.map.fitBounds(bounds)
                    } else {
                        alert('検索できませんでした。\n\n場所の名前は正しいか、２つ以上入力しているか確認して下さい。')
                    }
                }, 700)
            })
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