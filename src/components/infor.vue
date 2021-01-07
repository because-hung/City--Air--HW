<template>
  <div>
    <div class="card" style="width:250px;border:3px black solid;">
      <div class="card-header">
        {{ deta.County }} - {{ deta.SiteName }}
        <button class="p-2 ml-3" @click.prevent="mylove">最愛</button>
      </div>
      <div class="card-body " :class="statusColor">
        <!--加入顏色-->
        <ul class="list-unstyled">
          <li>AQI: {{ deta.AQI }}</li>
          <li>PM2.5: {{ deta["PM2.5"] }}</li>
          <li>說明: {{ deta.Status }}</li>
        </ul>
        {{ deta.PublishTime }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["deta"], //傳上層data 資料
  methods: {
    mylove() {
      this.$emit("father", this.deta);
      // mylove 使用 emit 觸發 father 跟 外層的getfocus ,同時將 deta 的資料傳出去
    }
  },
  computed: {
    statusColor: function() {
      let level = [
        "良好",
        "普通",
        "對敏感族群不健康",
        "對所有族群不健康",
        "非常不健康",
        "危害"
      ];
      let cssColor = [
        "status-aqi1",
        "status-aqi2",
        "status-aqi3",
        "status-aqi4",
        "status-aqi5",
        "status-aqi6"
      ];
      return cssColor[level.indexOf(this.deta.Status)];
      //  使用 indexOf 去找到資料的狀態，對應 level 的 index，去選擇要對應哪一個顏色
    }
    // 方法2 使用Swtich

    // aqiColor() {
    // switch(this.place.Status){
    //   case '良好':
    //     return 'status-aqi1';
    //     break;
    //   case '普通':
    //     return 'status-aqi2';
    //     break;
    //   case '對敏感族群不健康':
    //     return 'status-aqi3';
    //     break;
    //   case '對所有族群不健康':
    //     return 'status-aqi4';
    //     break;
    //   case '非常不健康':
    //     return 'status-aqi5';
    //     break;
    //   case '危害':
    //     return 'status-aqi6';
    //     break;
    // }
  }
};
</script>

<style>
.status-aqi1 {
  background-color: lightgreen;
}
.status-aqi2 {
  background-color: #ffff00;
}
.status-aqi3 {
  background-color: #ff7e00;
}
.status-aqi4 {
  background-color: #ff0000;
}
.status-aqi5 {
  background-color: #8f3f97;
}
.status-aqi6 {
  background-color: #7e0023;
}
</style>
