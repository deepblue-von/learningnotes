```vue
<template>
  <div class="avue-map">  // 基础容器样式
    <el-input readonly    // 原生属性，是否只读,默认值为false
              v-model="text" // 
              :size="size"
              :placeholder="placeholder">
      <el-button @click="box=true"
                 slot="append">{{textTitle}}</el-button>
    </el-input>

    <el-dialog class="avue-map__dialog"
               width="80%"
               append-to-body
               modal-append-to-body
               :title="title"
               @close="handleClose"
               :visible.sync="box">
      <div class="avue-map__content"
           v-if="box">
        <el-input class="avue-map__content-input"
                  id="map__input"
                  :readonly="disabled"
                  v-model="address"
                  clearable
                  placeholder="输入关键字选取地点"></el-input>
        <div class="avue-map__content-box">
          <div id="map__container"
               class="avue-map__content-container"
               tabindex="0"></div>
          <div id="map__result"
               class="avue-map__content-result"></div>
        </div>
      </div>
    </el-dialog>
  </div>
</template>
<script>
export default {
  name: "AvueMap",
  props: {
    size: String,
    placeholder: String,
    disabled: {
      type: Boolean,
      default: false,
    },
    value: {
      type: Object,
      default: () => {
        return {};
      }
    }
  },
  data () {
    return {
      address: '',
      poi: {},
      marker: null,
      map: null,
      box: false
    };
  },
  watch: {
    value: {
      handler (val) {
        this.poi = val;
      },
      deep: true,
      immediate: true
    },
    poi: {
      handler (val) {
        this.$emit("input", val);
      },
      deep: true
    },
    text: {
      handler (val) {
        this.address = val;
      },
      deep: true,
      immediate: true
    },
    box: {
      handler () {
        if (this.box) {
          this.$nextTick(() =>
            this.init(() => {
              if (this.longitude && this.latitude) {
                this.addMarker(this.longitude, this.latitude);
                this.getAddress(this.longitude, this.latitude);
              }
            })
          );
        }
      },
      immediate: true
    }
  },
  computed: {
    longitude () {
      return this.poi.longitude
    },
    latitude () {
      return this.poi.latitude
    },
    text () {
      return this.poi.formattedAddress
    },
    title () {
      return this.disabled ? "查看坐标" : '选择坐标'
    },
    textTitle () {
      return this.disabled ? this.title : (this.poi.name === undefined ? "选择坐标" : "重新选择");
    }
  },
  methods: {
    //新增坐标
    addMarker (R, P) {
      this.clearMarker();
      this.marker = new window.AMap.Marker({
        position: [R, P]
      });
      this.marker.setMap(this.map);
    },
    //清空坐标
    clearMarker () {
      if (this.marker) {
        this.marker.setMap(null);
        this.marker = null;
      }
    },
    //获取坐标
    getAddress (R, P) {
      new window.AMap.service("AMap.Geocoder", () => {
        //回调函数
        let geocoder = new window.AMap.Geocoder({});
        geocoder.getAddress([R, P], (status, result) => {
          if (status === "complete" && result.info === "OK") {
            const regeocode = result.regeocode;
            this.poi = Object.assign(regeocode, {
              longitude: R,
              latitude: P
            });
            // 自定义点标记内容
            var markerContent = document.createElement("div");

            // 点标记中的图标
            var markerImg = document.createElement("img");
            markerImg.src =
              "//a.amap.com/jsapi_demos/static/demo-center/icons/poi-marker-default.png";
            markerContent.appendChild(markerImg);

            // 点标记中的文本
            var markerSpan = document.createElement("span");
            markerSpan.className = "avue-map__marker";
            markerSpan.innerHTML = this.text;
            markerContent.appendChild(markerSpan);
            this.marker.setContent(markerContent); //更新点标记内容
          }
        });
      });
    },
    handleClose () {
      window.poiPicker.clearSearchResults();
    },
    addClick () {
      this.map.on("click", e => {
        const lnglat = e.lnglat;
        const P = lnglat.P || lnglat.Q;
        const R = lnglat.R;
        this.addMarker(R, P);
        this.getAddress(R, P);
      });
    },
    init (callback) {
      this.map = new window.AMap.Map("map__container", {
        zoom: 13,
        center: (() => {
          if (this.longitude && this.latitude) {
            return [this.longitude, this.latitude];
          }
        })()
      });
      this.initPoip();
      this.addClick();
      callback();
    },
    initPoip () {
      window.AMapUI.loadUI(["misc/PoiPicker"], PoiPicker => {
        var poiPicker = new PoiPicker({
          input: "map__input",
          placeSearchOptions: {
            map: this.map,
            pageSize: 10
          },
          searchResultsContainer: "map__result"
        });
        //初始化poiPicker
        this.poiPickerReady(poiPicker);
      });
    },
    poiPickerReady (poiPicker) {
      window.poiPicker = poiPicker;
      //选取了某个POI
      poiPicker.on("poiPicked", poiResult => {
        this.clearMarker();
        var source = poiResult.source,
          poi = poiResult.item;
        this.poi = Object.assign(poi, {
          formattedAddress: poi.name,
          longitude: poi.location.R,
          latitude: poi.location.P
        });
        if (source !== "search") {
          poiPicker.searchByKeyword(poi.name);
        }
      });
    }
  }
};
</script>

<style lang="scss">
.amap-icon img,
.amap-marker-content img {
  width: 25px;
  height: 34px;
}
.avue-map {
  &__marker {
    position: absolute;
    top: -20px;
    right: -118px;
    color: #fff;
    padding: 4px 10px;
    box-shadow: 1px 1px 1px rgba(10, 10, 10, 0.2);
    white-space: nowrap;
    font-size: 12px;
    font-family: "";
    background-color: #25a5f7;
    border-radius: 3px;
  }
  &__content {
    &-input {
      margin-bottom: 10px;
    }
    &-box {
      position: relative;
    }
    &-container {
      width: 100%;
      height: 450px;
    }
    &-result {
      display: block !important;
      position: absolute;
      top: 0;
      right: -8px;
      width: 250px;
      height: 450px;
      overflow-y: auto;
    }
  }
}
</style>
```