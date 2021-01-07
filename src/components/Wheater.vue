<template>
  <div>
    <div class="container">
      <h3>練習作業：Ajax 資料內容呈現</h3>
      <p>一定要用到元件。</p>
      <div class="alert alert-secondary">
        <p>提示：</p>
        <ol class="mb-0">
          <li>將資料內容透過 "元件" 呈現</li>
          <li>製作城市過濾選項</li>
          <li>將內層資料透過 emit 傳遞到外層 (作為另一個關注城市的呈現)</li>
          <li>依據不同污染呈現不同色彩</li>
          <!-- <li>加分題：透過 localStorage 儲存上次關注的城市</li> -->
        </ol>
      </div>

      <select name="" id="" class="form-control mb-3" @change="getCounty">
        <!--選擇城市-->
        <option value="">--- 請選擇城市 ---</option>
        <option :value="item" v-for="(item, key) in location" :key="key">{{
          item
        }}</option>
      </select>
      <div>
        <h4>關注城市</h4>
        <div class="row">
          <div class="col-4 mb-4" v-for="item in stared" :key="item.SiteId" x>
            <infor :deta="item" @father="getfocus"></infor>
            <!--getfous 取得內層資料-->
          </div>
        </div>
      </div>

      <hr />
      <div class="row">
        <div class="col-4 mb-4" v-for="item in filterData" :key="item.SiteId">
          <infor :deta="item" @father="getfocus"></infor>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import infor from "./infor";
import $ from "jquery";
export default {
  components: {
    infor
  },
  data() {
    return {
      data: [],
      meta: [],
      location: [],
      stared: [],
      filter: "",
      filterData: ""
    };
  },
  // 請在此撰寫 JavaScript
  methods: {
    getData() {
      const vm = this;
      const api = "http://opendata2.epa.gov.tw/AQI.json";

      // 使用 jQuery ajax
      $.get(api).then(function(response) {
        vm.data = response;
        console.log(response);
        let locationArray = [];
        for (let i = 0; i < vm.data.length; i++) {
          locationArray.push(vm.data[i].County);
        }
        vm.location = locationArray.filter(function(element, index, array) {
          //使用 filter 把重複的縣市過濾掉

          return array.indexOf(element) === index;
          //比對資料
        });
      });
    },
    getCounty(e) {
      //把emit 的資料 傳進去

      const vm = this;
      vm.filterData = []; //清除過濾資料
      for (let i = 0; i < vm.data.length; i++) {
        if (e.target.value == vm.data[i].County) {
          //當下拉式選單選取的縣市等於資料中的縣市時
          vm.filter = e.target.value;
          vm.filterData.push(vm.data[i]); //將資料中的縣市，加入到 filterData 裡面
        }
      }
    },
    getfocus(e) {
      if (this.stared.indexOf(e) === -1) {
        this.stared.push(e);
      } else {
        this.stared.splice(this.stared.indexOf(e), 1);
      }

      //先去搜尋關注名單裡面有沒有這筆資料
    }
  },

  created() {
    this.getData();
  }
};
</script>
