<template>
  <div class="text-center mt-2" id="ProductList">
    <h5>Product Type</h5>
    <select v-model.number="selected" npm install vue2-filters>
      <option v-bind:value="0">Please select</option>
      <option v-for="pt in res" :key="pt" v-bind:value="pt.id">
        {{ pt.name }}
      </option>
    </select>
    <span>Selected: {{ selected }}</span>

    <h5>Product Sub-Type</h5>
    <select v-model="selected01">
      <option v-bind:value="0">Please select</option>
      <option v-for="st in toFilter" :key="st" v-bind:value="st.id">
        {{ st.name }}
      </option>
    </select>
    <span>Selected: {{ selected01 }}</span>

    <h5>Branch</h5>
    <select v-model="selectedBranch">
      <option v-bind:value="0">Please select</option>
      <option v-for="br in branch" :key="br" v-bind:value="br.id">
        {{ br.name }}
      </option>
    </select>
    <span>Selected: {{ selectedBranch }}</span>

    <h5>Gold type</h5>
    <select v-model="selectedGold">
      <option v-bind:value="0">Please select</option>
      <option v-for="gt in goldType" :key="gt" v-bind:value="gt.id">
        {{ gt.fullname }}
      </option>
    </select>
    <span>Selected: {{ selectedGold }}</span>

    <button v-on:click="getPModelFilter()">Search</button>
  </div>
</template>

<script>
//import Vue from "vue"
import axios from "axios";

export default {
  name: "fineJewelry",
  vuetify: new Vuetify(),
  data() {
    return {
      token: {},
      res: [],
      subtype: [],
      selected: 0,
      selected01: 0,
      selectedBranch: 0,
      selectedGold: 0,
      toFilter: [],
      branch: [],
      goldType: [],
      info: [],
    };
  },
  methods: {
    filter: function filter(selected) {
      console.log(selected);
      console.log(this.subtype.length);
      this.toFilter = [];
      var i = 0;
      while (i < this.subtype.length) {
        if (selected == this.subtype[i].product_type_id) {
          this.toFilter.push(this.subtype[i]);
          // console.log(selected +'='+this.subtype[i].product_type_id);
          // console.log(this.subtype[i].product_type_id);
        }

        i++;
      }
      // console.log(this.toFilter);
    },
    getPModelFilter: function getPModelFilter() {
      axios
        .get(
          "https://mercury.8gra.de:8885/API/0.2.3/getPModelFilter?accesstoken=" +
            this.token +
            "&modelCode&branch=" +
            this.selectedBranch +
            "&productType=" +
            this.selected +
            "&subType=" +
            this.selected01 +
            "&goldType=" +
            this.selectedGold +
            "&forDiamondFrom&forDiamondTo&ringSizeFrom&ringSizeTo&priceFrom&priceTo"
        )
        .then((resInfo) => {
          //this.info = resInfo.data.data;
          console.log(resInfo.data);
        });
    },

    begin: function () {
      axios
        .get(
          "https://mercury.8gra.de:8885/API/0.2.3/author/accessToken?user=JaiPoomIntern2021&password=kfBEwR3bN%LQ^yR"
        )
        .then((response) => {
          this.token = response.data.data.AccessToken;
          //console.log(response.data.data.AccessToken)

          axios
            .get(
              "https://mercury.8gra.de:8885/API/0.2.3/getProductTypeList?accesstoken=" +
                response.data.data.AccessToken
            )
            .then((resp) => {
              this.res = resp.data.data;
              //console.log(resp.data.data);
            });

          axios
            .get(
              "https://mercury.8gra.de:8885/API/0.2.3/getSubTypeList?accesstoken=" +
                response.data.data.AccessToken +
                "&typeID="
            )
            .then((sub) => {
              this.subtype = sub.data.data;
              //this.toFilter =sub.data.data
              //console.log(sub.data.data);
            });

          axios
            .get(
              "https://mercury.8gra.de:8885/API/0.2.3/listBranch?accesstoken=" +
                response.data.data.AccessToken
            )
            .then((br) => {
              this.branch = br.data.data;
              //this.toFilter =sub.data.data
              //console.log(br.data.data);
            });

          axios
            .get(
              "https://mercury.8gra.de:8885/API/0.2.3/getGoldType?accesstoken=" +
                response.data.data.AccessToken
            )
            .then((gd) => {
              this.goldType = gd.data.data;
              //this.toFilter =sub.data.data
              //console.log(gd.data.data);
            });
        })
        .catch(function (error) {
          console.log(error);
        });
    },
  },
  created: function () {
    this.begin();
  },
};
</script>
