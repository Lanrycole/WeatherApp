<template>
<div class="container">
  <div class="forecaset_info">

    <p> {{moment(day).format('dddd')}}<br>{{moment(date).format("MMM Do YY")}}</p>
    <p><img :src="icon" width="24px"/><br>{{condition}}</p>
    <p>{{max_temp}}<br><span class="feels">Feels Like {{min_temp }}</span></p>
  </div>
</div>
</template>

<script>
export default {

  name: "forecast",

  props:{
    icon:{
      required:true,
    },
    day:{
      required:true,

    } ,
    date:{
      required:true,

    } ,
    condition:{
      required:true,

    } ,
    max_temp:{
      required:true,

    }
    ,
    min_temp:{
      required:true,

    }
  }
}
</script>

<style lang="scss">
@import '../assets/Colors/colors';
@import '../assets/Font/Font';

.forecaset_info {
  padding: 10px;
  display: flex;
  justify-content: space-between;
  color: $text_color;
  box-shadow: rgba(0, 0, 0, 0.19) 0px 10px 20px, rgba(0, 0, 0, 0.23) 0px 6px 6px;

p {
  font-weight: 400;
margin: 1vh auto;
.feels {
  background: $secondary_color;
  padding: 5px;
  border-radius: 2rem;
}
}
}
</style>