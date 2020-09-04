<template>
  <div class="en-merchants">
    <div :class="mainSide" class="en-merchants-content">
      <deals  :items="deals" @updated="forceRerender" />
    </div>
    <div class="limit-control-container">
      <div class="control-btns">
        <button @click="seeMore()" class="btn btn-primary see-more-btn">See More</button>
        <!-- <button @click="showLess()" class="btn btn-primary see-more-btn">Show Less</button> -->
      </div>
      <!-- <select class="custom-select">
        <option selected>Open this select menu</option>
        <option value="1">One</option>
        <option value="2">Two</option>
        <option value="3">Three</option>
      </select> -->
    </div>
  </div>
</template>
<script>
  import deals from "~/components/en/Dashboard/deals.vue"

  export default {
    components: {
      deals,
    },
    head() {
      return {
        titleTemplate: '%s - Deals',
      }
    },
    props: {
      mainSide: {
        type: String,
        default: null,
      },
    },
    data () {
      return {
        merchantsKey: 0,
        deals: [],
        dealsLimit: 50,
        flatMerchants: [],
      }
    },
    layout: "adminlayout",

    mounted() {
      let config = {
        headers: {
          authorization: localStorage.getItem("token"),
          "Content-Type": `application/json`,
        },
      }
      // https://api.yalladealz.com/api/deals/?skip=20&limit=20  :: skip will skip ?? deals and limit displays limited deals
      this.$axios.get(`/deals/?limit=${this.dealsLimit}` , config).then((res) => {
        this.deals  = res.data.deals;
      });
    },

    methods: {
      forceRerender () {
        this.merchantsKey += 1
      },

      seeMore(){
        this.dealsLimit+= 50;
      },

      // showLess(){
      //   this.dealsLimit-= 50;
      // }
    },

    watch: {
      dealsLimit() {
        let config = {
          headers: {
            authorization: localStorage.getItem("token"),
            "Content-Type": `application/json`,
          },
        }
        this.$axios.get(`/deals/?limit=${this.dealsLimit}` , config).then((res) => {
          this.deals  = res.data.deals;
        });
      }
    }
  }
</script>

<style lang="scss" scoped>
  .en-merchants {
    width: 100%;
    display: flex;
    flex-flow: column;
    align-items: center;
    justify-content: center;
    &-content {
      margin-top: 5%;
      width: 100%;
      display: flex;
      flex-flow: row;
      justify-content: center;
    }
  }

  // .limit-control-container{
  //   display: flex;
  //   flex-direction: column;
  //   margin-top: 20px;
  // }

  .control-btns {
    margin: 10px;
  }
</style>
