<template>
  <div class="search-container" style="width: 2000px;">
    <div class="search" style="border: 2px solid  #7fac7d; border-radius: 1%;  margin-right: 20px;">
      <form v-on:submit.prevent="submitForm" @click="checkValue">
        <input
          type="text"
          placeholder="search"
          list="addressShow"
          v-model="search.address"
          style="height: 8%; font-size: medium"
        />

        <img
          src="https://s3.ap-northeast-2.amazonaws.com/cdn.wecode.co.kr/icon/search.png"
          id="searchimg"
          style="float: left; height: 4%; width: 4%"
          @click="submitForm"
        />
        <datalist id="addressShow" style="margin-bottom: 10px;">
          <option v-for="(address, idx) in search.addressList" :key="idx">
          <span style="font-size: 1.3em;"> {{ address }}</span> 
          </option>
        </datalist>

        <!-- 체크박스 창   -->
        <div
          style="height: 60px; padding-bottom: 1%; background-color:   #7fac7d;"
        >
          <div class="icon-box" style="margin-bottom: 5px;">
            <img
              src="@/assets/img/vegun-icon/vegetable.png"
              alt="Image"
              style="
                float: left;
                border-radius: 5%;
                height: 40px;
                margin-left: 7%;
                margin-top: 3%;
              "
            />
            <div class="form-check" style="margin-top: 6%; float: left; margin-left: 5px;">
              <input
                class="form-check-input"
                type="checkbox"
                style="margin-top: 7px; margin-right: 3px"
                v-model="flexCheckVegetable"
                id="flexCheckVegetable"
              />
              <label class="form-check-label" for="flexCheckVegetable"  style="padding-top: 3px;">
                채소
              </label>
            </div>
          </div>

          <div class="icon-box" style="margin-bottom: 5px; ">
            <img
              src="@/assets/img/vegun-icon/milk.png"
              alt="Image"
              style="
                float: left;
                border-radius: 5%;
                height: 40px;
                margin-left: 2%;
                margin-top: 3%;
               
              "
            />
            <div class="form-check" style="margin-top: 6%; float: left; margin-left: 5px;">
              <input
                class="form-check-input"
                type="checkbox"
                style="margin-top: 7px; margin-right: 3px"
                v-model="flexCheckMilk"
                id="flexCheckMilk"
              />
              <label class="form-check-label" for="flexCheckMilk" style="padding-top: 3px;"> 우유 </label>
            </div>
          </div>

          <div class="icon-box" style="margin-bottom: 5px">
            <img
              src="@/assets/img/vegun-icon/egg.png"
              alt="Image"
              style="
                float: left;
                border-radius: 5%;
                height: 40px;
                margin-left: 2%;
                margin-top: 3%;
              "
            />
            <div class="form-check" style="margin-top: 6%; float: left; margin-left: 5px;">
              <input
                class="form-check-input"
                type="checkbox"
                style="margin-top: 7px; margin-right: 3px"
                v-model="flexCheckEgg"
                id="flexCheckEgg"
              />
              <label class="form-check-label" for="flexCheckEgg" style="padding-top: 3px;"> 계란 </label>
            </div>
          </div>

          <div class="icon-box" style="margin-bottom: 5px">
            <img
              src="@/assets/img/vegun-icon/fish.png"
              height="40px%"
              style="
                float: left;
                border-radius: 5%;
                height: 40px;
                margin-left: 2%;
                margin-top: 3%;
              "
            />
            <div class="form-check" style="margin-top: 6%; float: left; margin-left: 5px;">
              <input
                class="form-check-input"
                type="checkbox"
                style="margin-top: 7px; margin-right: 3px;"
                v-model="flexCheckFish"
                id="flexCheckFish"
              />
              <label class="form-check-label" for="flexCheckFish" style="padding-top: 3px;"> 생선 </label>
            </div>
          </div>
        </div>
      </form>
      <!-- 조건 검색창 END -->

      <div class="container" style="height: 400px; font-size: small">
        <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
          <div v-for="(item, idx) in markers" :key="idx">
            <div class="col">
              <div
                class="card shadow-sm"
                style="
                  height: 170px;
                  width: 150px;
                  align-items: center;
                  justify-content: center;
                  background-color:  white;
                "
                @click="onClickButton(idx)"
              >
                <img
                  style="
                    width: 130px;
                    height: 100px;
                   
                    margin: 8px;
                  "
                  :src="item.imageSrc"
                  alt=""
                />
                <div
                  style="
                    font-size: xx-small;
                    width: 130px;
                    height: 40%;
                    margin: auto;
                    background-color: whitesmoke;
                    margin: 4px;
                    border-radius: 5%;
                    border-color: black;
                    border-width: 1px;
                  "
                >
                  <router-link
                    :to="`/Catchvegan/restaurant/detail/${item.restaurantIdx}`" style=" text-decoration: none;"
                  >
                    <span style="font-size: 1.6em; color: black; margin: 5px;"><b>{{ item.name }}</b></span>
                  </router-link>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Page Nation -->
      <div class="justify-content-center align-items-center" style="margin-top: 50px;">
        <nav aria-label="Page navigation example">
          <ul class="pagination" style="margin-left: 5%">
            <li class="page-item">
              <button
                class="page-link"
                v-if="cur_page != 1"
                @click="onSetMapMarkers(cur_page - 1)"
            >
                Previous
              </button>
            </li>
            <li class="page-item" v-for="(item, idx) in paging_list" :key="idx">
              <button
                class="page-link"
                :class="item === cur_page ? 'active' : ''"
                @click="onSetMapMarkers(item)"
              >
                {{ item }}
              </button>
            </li>
            <li class="page-item">
              <button
                class="page-link"
                v-if="cur_page != max_page"
                @click="onSetMapMarkers(cur_page + 1)"
              >
                Next
              </button>
            </li>
          </ul>
        </nav>
      </div>
      <!-- Page Nation END-->
    </div>
    <div style="width: 100%; height: 66vh; background-color: aqua">
      <naver-map
        style="width: 100%; height: 100%"
        :mapOptions="mapOptions"
        @onLoad="onLoadMap($event)"
      >
        <naver-marker :latitude="search.latitude" :longitude="search.longitude">
        </naver-marker>
        <div v-for="(item, idx) in markers" :key="idx" style="width: 200px;">
          <naver-marker
            @click="onClickButton(idx)"
            :latitude="item.latitude"
            :longitude="item.longitude"
            @onLoad="onLoadMarker($event, idx)"
          >
          </naver-marker>
          <naver-info-window
            :marker="item.marker"
            :open="item.isOpen"
            style="font-size: 10px; background-color: white; width: 160px;"
            @onLoad="onLoadInfoWindow($event, idx)"
          >
            <img
              style="
                width: 150px;
                height: 150px;
                margin: 5px;
               
              "
              :src="item.imageSrc"
              :alt="item.name"
            />
            <div
              style="
                margin: 10px;
                background-color: white;
                border-radius: 10%;
                height: 30px;
                margin-bottom: 30px;
              "
            >
              <router-link
                :to="`/Catchvegan/restaurant/detail/${item.restaurantIdx}`"
               style="text-decoration: none; font-size: 1.5em; color: black;">
               <b style="margin: auto;"> {{ item.name }}</b>
              
              </router-link>
              <p style="width: 100%;">{{ item.address }}</p>
            </div>
            <div class="infowindow-style"></div>
          </naver-info-window>
        </div>
      </naver-map>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";
import { NaverMap, NaverMarker, NaverInfoWindow } from "vue3-naver-maps";
import axios from "axios";
import { useSearch } from "@/stores/search";

const search = useSearch();

//네이버 맵 객체
const naver_map = ref();

//주소 리스트 저장(ex: 송파구, 금천구, 종로구)
const addressList = ref([]);

// 지도에 표시될 마커들의 배열
const markers = ref([]);

const checkValue = ref(); //Checkbox 변화 감지 함수
const submitForm = ref(); //Ajax 리스트 요청 함수

//지도에 표시될 아이콘 (야채, 계란, 생선, 우유)
const iconVegetable = ref(require("@/assets/img/vegun-icon/vegetable.png"));
const iconEgg = ref(require("@/assets/img/vegun-icon/egg.png"));
const iconFish = ref(require("@/assets/img/vegun-icon/fish.png"));
const iconMilk = ref(require("@/assets/img/vegun-icon/milk.png"));

//체크박스 인식 조건 (야채, 계란, 생선, 우유)
const flexCheckVegetable = ref();
const flexCheckMilk = ref();
const flexCheckEgg = ref();
const flexCheckFish = ref();

//Get 요청을 보낼 URL
const getUrl = ref();

//Get 요청의 갯수를 요청할 URl
const getCountUrl = ref();

const totalCnt = ref(0);
const max_page = ref(0);
const cur_page = ref(1);
const paging_list = ref([]);

const onLoadMap = (mapObject) => {
  naver_map.value = mapObject;
};

if (search.latitude === 0 || search.longitude) search.setLocation();

const onLoadMarker = (markerObject, idx) => {
  markers.value[idx] = Object.assign({}, markers.value[idx], {
    marker: markerObject,
  });

  if (markers.value[idx].veganType.indexOf("vegetable") !== -1)
    markers.value[idx].marker.setIcon(iconVegetable);

  if (markers.value[idx].veganType.indexOf("milk") !== -1)
    markers.value[idx].marker.setIcon(iconMilk);

  if (markers.value[idx].veganType.indexOf("egg") !== -1)
    markers.value[idx].marker.setIcon(iconEgg);

  if (markers.value[idx].veganType.indexOf("fish") !== -1)
    markers.value[idx].marker.setIcon(iconFish);
};

const onLoadInfoWindow = (infoWindowObject, idx) => {
  markers.value[idx] = Object.assign({}, markers.value[idx], {
    infoWindow: infoWindowObject,
  });
};

submitForm.value = async () => {
  checkValue.value(); //체크박스 검사
  onSetMapMarkers(); // Ajax 요청 및 지도 데이터 로드
};

/////////////////////////////////////
// 현재 위치 데이터 Load 함수        //
/////////////////////////////////////
var options = {
  enableHighAccuracy: true,
  timeout: 5000,
  maximumAge: 0,
};

//네이버 맵 객체 로딩 완료후 데이터 Loading
watch([naver_map], () => {
  checkValue.value();

  const latLng = new window.naver.maps.LatLng(
    search.latitude,
    search.longitude
  );

  naver_map.value.setCenter(latLng); // 현재 위치 중심으로 데이터 지도 중심 변경

  setTimeout(() => {
    onSetMapMarkers();
  }, 100);
});

const onSetMapMarkers = (page = 1) => {
  // 마커 초기화
  for (var i = 0; i < markers.value.length; i++) {
    markers.value[i].marker.setMap(null);
    console.log("marker");
  }
  markers.value = [];

  // alert(getCountUrl.value);

  axios
    .get(getCountUrl.value)
    .then((res) => {
      totalCnt.value = res.data;
    })
    .then(() => {
      max_page.value = Math.ceil(totalCnt.value / 6);
      cur_page.value = page;
      paging_list.value = [];

      let i = 1;
      if (cur_page.value >= 3) {
        i = cur_page.value - 2;
        if (cur_page.value - 2 >= max_page.value - 4 && max_page.value > 5)
          i = max_page.value - 4;
      }
      for (; i <= max_page.value; i++) {
        paging_list.value.push(i);
        if (paging_list.value.length == 5) {
          break;
        }
      }
    });

  checkValue.value();
  cur_page.value = page;
  getUrl.value += "&pageNum=" + cur_page.value;

  // alert(getUrl.value);
  //새로운 마커 정보 요청
  axios
    .get(getUrl.value)
    .then((res) => {
      markers.value = res.data;
    })
    .then(() => {
      for (let i = 0; i < markers.value.length; i++) {
        let imgUrl = require("@/assets/img/restaurant/" +
          markers.value[i].images);
        markers.value[i].imageSrc = imgUrl;
      }
    })
    .then(() => {
      if (markers.value.length >= 1) {
        const latLng = new window.naver.maps.LatLng(
          markers.value[0].latitude,
          markers.value[0].longitude
        );
        naver_map.value.setCenter(latLng); // Change Map Center
      }
    });
};

const Swal = require("sweetalert2");

const onClickButton = (idx) => {
  if (markers.value[idx].isOpen === true) markers.value[idx].isOpen = false;
  else {
    for (var i = 0; i < markers.value.length; i++)
      markers.value[i].isOpen = false;
    markers.value[idx].isOpen = true;
  }
};

checkValue.value = () => {
  getUrl.value = "http://localhost:8082/Catchvegan/search/";
  getCountUrl.value = "http://localhost:8082/Catchvegan/search/count/";

  let subStr = "";

  if (search.addressList.indexOf(search.address) !== -1)
    subStr += "address?keyword=" + search.address;
  else {
    if (search.address.length !== 0) {
      Swal.fire({
        title: "잘못된 주소 검색",
        text: "잘못된 입력 입니다. 현재 위도 경도 데이터를 기준으로 다시 검색합니다.",
        icon: "error",
        confirmButtonText: "확인",
      });
      search.address = "";
    }
    subStr +=
      "location?latitude=" +
      search.latitude +
      "&" +
      "longitude=" +
      search.longitude;
  }

  if (flexCheckVegetable.value === true) subStr += "&typeVege=vegetable";
  if (flexCheckMilk.value === true) subStr += "&typeMilk=milk";
  if (flexCheckEgg.value === true) subStr += "&typeEgg=egg";
  if (flexCheckFish.value === true) subStr += "&typeFish=fish";

  getUrl.value += subStr;
  getCountUrl.value += subStr;
};

if (search.addressList.length === 0) {
  axios
    .get("http://localhost:8082/Catchvegan/search/addressList")
    .then((res) => {
      addressList.value = res.data;
    })
    .then(() => {
      search.setAddressListClear();
      for (let i = 0; i < addressList.value.length; i++) {
        search.setAddressListPush(addressList.value[i]);
      }
    })
    .catch((err) => {
      console.log("Catchvegun address List Get Fail");

      if (!err.response) console.log("Please check your internet connection.");
      else console.log("Error Status Code : " + err.response.status);

      return Promise.reject(err);
    });
}

const mapOptions = {
  latitude: 37.51347, // 지도 중앙 위도
  longitude: 127.051722, // 지도 중앙 경도
  zoom: 13,
  zoomControl: false,
  zoomControlOptions: { position: "TOP_RIGHT" },
};
</script>

<style>
.search-container {
  display: flex; /* Flexbox를 사용 */
  justify-content: left; /* 가로 중앙 정렬 */
  align-items: left; /* 세로 중앙 정렬 */
  margin-top: 50px;
  margin-left: 40px;
}

.icon-box {
  width: 25%;
  height: 100%;
  background-color: #7fac7d;
  text-align: center;
  align-content: center;
  align-items: center;
  float: left;
  margin-top: 8px;
  font-weight: bolder;
  color: white;
}

.infowindow-style {
  color: black;
  background-color: white;
  text-align: center;
  font-weight: 600;
  padding: 6px 8px;
  border-radius: 10%;
}

.page-link {
  color: black;
 
}
.justify-content-center {
  display: flex;
  justify-content: center;
}

.align-items-center {
  display: flex;
  align-items: center;
}

/* 선택된 버튼에 대한 스타일 */
.page-item button.active
{
  background-color: #7fac7d;
  border:  #7fac7d;
  color: white;
}

/* Hover 시 버튼에 대한 스타일 */
.page-item button:hover {
  background-color:  #7fac7d;
  border:   #7fac7d;
  color: white;
}
</style>
