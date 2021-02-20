<template>
  <div class="container">
    <div class="card">
      <div class="icon">
        <img :src="imgs"/>
      </div>
      <div class="con">
        <div class="text">
          <p>{{ details1 }}</p>
        </div>
        <div class="details">
          <p class="value">{{ details2 }}</p>
        </div>
      </div>
    </div>
  </div>

</template>

<script>

export default {
  name: "forecastcard",
  props: {
    imgs: {
      required: true,
    },
    details1: {
      required: true,
      type: String,
    },
    details2: {
      required: true,
    },


  }
}
</script>

<style lang="scss" scoped>
@import '../assets/Colors/colors';
@import '../assets/Font/Font';

.container {


  .card {
    width: 95%;
    margin: 1vh auto;
    border-radius: 6px;
    text-align: center;
    display: flex;
    justify-content: space-between;
    //box-shadow: 0 5px 10px rgba(31, 32, 38, 0.05), 0 15px 40px rgba(103, 108, 125, 0.2);
    padding: 2px;
    background: #E1E6F3;

    .icon {
  img{
    padding-top: 15px;
  }
    }

    .con {
      width: 80%;
      border-radius: 5px;
      box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
      background: white;
      font-weight: bolder;
      text-align: start;
      padding: 10px;
      display: flex;
      justify-content: space-between;

      .text {
        color: $background_color;
      }

      .details {
        background: $highlight_color;
        width: 50px;
        text-align: center;
        padding: 5px;
      }

    }
  }


}
</style>