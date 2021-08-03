  
<template>
 <v-main>
<v-container fluid>
       <h1>Fine Jewelry</h1>
          <v-row align="center">
          <v-col  cols="3"
            xs="3"
            md="2">
            <v-subheader>
             Product Type
            </v-subheader>
          </v-col>

          <v-col class="d-flex"
            cols="8"
            xs="9"
            sm="3"
            md="3">
            <v-select
              v-model="selected"
              :hint="`${selected.id}, ${selected.name}`"
              :items="res"
              v-on:change="filter(`${selected.id}`)"
              item-text="name"
              item-value="id"
              label="Selete product type"
              persistent-hint
              return-object
              single-line
            ></v-select>
          </v-col>
           <v-col  cols="3"
            xs="3"
            md="2">
            <v-subheader>
              Product Sub-Type
            </v-subheader>
          </v-col>

           <v-col class="d-flex"
            cols="8"
            xs="9"
            sm="3"
            md="3">
            <v-select
              v-model="selected01"
              :hint="`${selected01.id}, ${selected01.name}`"
              :items="toFilter"
              item-text="name"
              item-value="id"
              label="Select Sub-Type"
              persistent-hint
              return-object
              single-line
            ></v-select>
          </v-col>
        </v-row>
       <v-row align="center">
          <v-col  cols="3"
            xs="3"
            md="2">
            <v-subheader>
             Branch
            </v-subheader>
          </v-col>

           <v-col class="d-flex" cols="8" xs="9" sm="3" md="3">
            <v-select
              v-model="selectedBranch"
              :hint="`${selectedBranch.id}, ${selectedBranch.name}`"
              :items="branch"
              
              item-text="name"
              item-value="id"
              label="Selete branch"
              persistent-hint
              return-object
              single-line
            ></v-select>
          </v-col>
           <v-col  cols="3"
            xs="3"
            md="2">
            <v-subheader>
              Gold Type
            </v-subheader>
          </v-col>

           <v-col class="d-flex"
            cols="8"
            xs="9"
            sm="3"
            md="3">
            <v-select
              v-model="selectedGold"
              :hint="`${selectedGold.id}, ${selectedGold.fullname}`"
              :items="goldType"
              item-text="fullname"
              item-value="id"
              label="Select Gold Type"
              persistent-hint
              return-object
              single-line
            ></v-select>
          </v-col>
        </v-row>
          
        <v-btn
          depressed
          color="primary"
        v-on:click="getPModelFilter()"
        >
          Search
           <v-icon
          dark
          right
        >
          mdi-magnify
        </v-icon>
        </v-btn>
         <v-btn
          depressed
          color="danger"
        v-on:click="resetFilter()"
        >
          reset
           <v-icon
          dark
          right
        >
          mdi-magnify
        </v-icon>
        </v-btn>
         <v-card align="center" width="85%">
    <v-card-title >
      Find Jewelry
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
        
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="info"
      :search="search"
    ></v-data-table>
  </v-card>
      </v-container>
  </v-main>
</template>

<script>

import axios from "axios";
  export default {
    data: () => ({ 
    
      search: '',
        headers: [
          {
            text: 'Product code',
            align: 'start',
            sortable: false,
            value: 'code',
          },
          { text: 'Name', value: 'name' },
          { text: 'Type|sub (g)', value: 'subname' },
          { text: 'ATS ', value: 'ATS' },
          { text: 'Min (Bath)', value: 'Min' },
          { text: 'Max (Bath)', value: 'Max' },
        ],
    
      select: { state: 'Florida', abbr: 'FL' },
        itemsState: [
          { state: 'Florida', abbr: 'FL' },
          { state: 'Georgia', abbr: 'GA' },
          { state: 'Nebraska', abbr: 'NE' },
          { state: 'California', abbr: 'CA' },
          { state: 'New York', abbr: 'NY' },
        ],
      token: {},
      res: [],
      subtype: [],
      selected: {"id":0, "name": "please selete "},
      selected01: {"id":0, "name": "please selete "},
      selectedBranch: {"id":0, "name": "please selete "},
      selectedGold: {"id":0, "fullname": "please selete "},
      toFilter: [],
      branch: [],
      goldType: [],
      info: [],
      }),
      
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
      console.log(this.toFilter);
    },
    getPModelFilter: function getPModelFilter() {
      //console.log("sdasdas");
      axios
        .get(
          "https://mercury.8gra.de:8885/API/0.2.3/getPModelFilter?accesstoken=" +
            this.token +
            "&modelCode&branch=" +
            this.selectedBranch.id +
            "&productType=" +
            this.selected.id +
            "&subType=" +
            this.selected01.id +
            "&goldType=" +
            this.selectedGold.id +
            "&forDiamondFrom&forDiamondTo&ringSizeFrom&ringSizeTo&priceFrom&priceTo"
        )
        .then((resInfo) => {
          this.info = resInfo.data.data;
         // console.log(resInfo.data.data); 
         // console.log("sdasdas");
        });
    },
      resetFilter: function resetFilter() {
      console.log("sdasdas");
      this.selected={"id":0, "name": "please selete "};
      this.selected01= {"id":0, "name": "please selete "};
      this.selectedBranch= {"id":0, "name": "please selete "};
      this.selectedGold= {"id":0, "fullname": "please selete "};
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
              this.toFilter =sub.data.data
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
  }
</script>

<style scoped>

</style>