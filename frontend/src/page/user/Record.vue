<template>
  <div class="container">
    <h1 style="margin: 20px;">🏆 내 등급 🏆</h1>
    <div id="carousel">
      <carousel-3d class="carousel-3d-container" style="height: 550px !important; margin; width: 900px !important;">
        <span v-for="n in 7" :key="n">
          <slide :index="n-1">
            <span v-if="n-1 == 0">
              <div class="crs-bx" style="border: solid 7px #ffd700;">
                <h5><strong>현재 등급</strong></h5>
                <img class="grade-img" :src="require('../../assets/img/lv'+remainder(level(user.grade)+n-1)+'.png')"/>
                <div class="text-box">
                  <h2><strong>LV. {{remainder(level(user.grade)+n-1)+1}}</strong></h2>
                  <h3>{{animal(remainder(level(user.grade)+n-1))}}</h3>
                  <h5>{{user.grade}}점</h5>
                  <h4>답변 수: {{user.rpCnt}}개</h4>
                  <h4>좋아요 수: {{user.rpLike}}개</h4>
                </div>
              </div>
            </span>
            <span v-else>
              <div class="crs-bx">
                <img class="grade-img" :src="require('../../assets/img/lv'+remainder(level(user.grade)+n-1)+'.png')"/>
                <div class="text-box">
                  <br/>
                  <h2><strong>LV. {{remainder(level(user.grade)+n-1)+1}}</strong></h2>
                  <br/>
                  <h3>{{animal(remainder(level(user.grade)+n-1))}}</h3>
                  <br/>
                  <h3>{{boundary[remainder(level(user.grade)+n-1)]}}</h3>
                </div>
              </div>
            </span>
          </slide>
        </span>
      </carousel-3d>
    </div>
    <!-- 등급 가이드 -->
    <div class="container grade-guide">
      <img :src="require('../../assets/img/lv'+level(user.grade)+'.png')" style="width: 100px; display: inline-block;" />
      <span v-if="level(user.grade) == 6">
        <h3 style="display: inline-block;">당신은 최고 등급 백상아리 입니다. 명예의 전당에 도전하세요!</h3>
      </span>
      <span v-else>
        <h3 style="display: inline-block;">당신은 {{stage(level(user.grade))}} 입니다. 다음 등급 {{stage(level(user.grade)+1)}}까지 {{line[level(user.grade)+1]-user.grade}}점 남았습니다.</h3>
      </span>
    </div>
  </div>  
</template>

<script>
  import {Carousel3d, Slide}  from 'vue-carousel-3d'
  import axios from 'axios'
  export default {
    name: 'Record',
    components: {
      Carousel3d,
      Slide
    },
    data () {
      return {
        user : {'grade':null},
        boundary : [
          '0 ~ 99 점',
          '100 ~ 199 점',
          '200 ~ 299 점',
          '300 ~ 399 점',
          '400 ~ 499 점',
          '500 ~ 599 점',
          '600 점 이상'],
        line : [0,100,200,300,400,500,600]
      }
    },
    methods : {
      getuser() {
        axios.get(this.$store.state.base_url + '/info/mygrade',{
          params: {
          },
          headers: {
            "ACCESS-TOKEN" : this.$store.state.token
            }
        })
        .then((response) => {
          this.user = response.data.data.hof
        })
        .catch((error) => {
            swal('', '세션 만료.\n다시 로그인 해주세요.', 'warning')
            this.$cookies.remove('auth-token')
            this.$store.commit('checkToken',this.$cookies.get('auth-token'))
            this.$store.commit('checklogin',this.$cookies.isKey('auth-token'))
            this.$router.push('/login')
        })
      },
      level(grade){
        if (grade < 100){return 0}
        else if (grade>=100 && grade<200){return 1}
        else if (grade>=200 && grade<300){return 2}
        else if (grade>=300 && grade<400){return 3}
        else if (grade>=400 && grade<500){return 4}
        else if (grade>=500 && grade<600){return 5}
        else {return 6}
      },  
      animal(level){
        if (level==0){return '🦠 플랑크톤 🦠'}
        else if (level==1){return '🐟 멸치 🐟'}
        else if (level==2){return '🦐 새우 🦐'}
        else if (level==3){return '🦑 해파리 🦑'}
        else if (level==4){return '🐙 문어 🐙'}
        else if (level==5){return '🦈 돌고래 🦈'}
        else {return '🐳 백상아리 🐳'}
      },
      stage(level){
        if (level==0){return '플랑크톤'}
        else if (level==1){return '멸치'}
        else if (level==2){return '새우'}
        else if (level==3){return '해파리'}
        else if (level==4){return '문어'}
        else if (level==5){return '돌고래'}
        else {return '백상아리'}   
      },
      remainder(val){
        return val%7
      }
    },
    created() {
      this.getuser()
    }
  }
</script>

<style scoped>
  @font-face {
    font-family: 'CookieRun-Regular';
    src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2001@1.1/CookieRun-Regular.woff') format('woff');
    font-weight: normal;
    font-style: normal;
  }

  * {
      font-family: 'CookieRun-Regular';
  }
            

  body {
    background-size: cover;
  }

  .carousel-3d-container {
    width: 900px;
    position: relative;
    margin-bottom: 20px;
  }

  .carousel-3d-slide {
    height: 550px !important;
    margin-top: 20px;
  }
  
  .crs-bx {
    padding-top: 20px;
    text-align: center;
    vertical-align: middle;
    border: solid 2px #000;
    background-color: white;
    height: 530px;
  }

  .grade-img {
    padding: 30px;
    border: solid 3px #E2E2E2;
    width: 250px;
    height: 250px;
  }

  .text-box {
    margin: 20px;
    height: 186px;
  } 

  .grade-guide {
    position: relative;
    bottom: 10px;
    padding: 0px;
  }
</style>