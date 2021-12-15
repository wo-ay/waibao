<template>
  <div class="home">
    <HeaderTop title="在线考试系统" style="background-color: #4ab8a1">
      <router-link class="header_login" slot="right" :to="userInfo.sno ? '/profile/info' : '/login'">
        <span class="header_login_text" v-if="!userInfo.sno">
          登录|注册
        </span>
        <span class="header_login_text" v-else>
          <img :src="userInfo.stuImgSrc ? userInfo.stuImgSrc : require('../../common/imgs/profile.jpg')" class="profile_img">
        </span>
      </router-link>
    </HeaderTop>

    <Swiper :lunbotuList="rotationImages" :isfull="true" class="swiper"></Swiper>

    <!--:class="{ 'already-clock': alreadyClock }"-->
    <div class="calendar" @click="toCalendar">
      <div class="calendar-left">
        <div style="float: left;width: 6px;height: 30px;background-color: #4ab8a1"></div>
        <i class="iconfont iconrili"></i>
        考试日历
      </div>
      <div class="calendar-right" v-if="userInfo.sno" >
        <i class="iconfont iconxiazai41"></i>
        最新考试信息：{{examCalendar[0].teaName}}{{examCalendar[0].noticeCreateTime | date-format('M')}}月{{examCalendar[0].noticeCreateTime | date-format('D')}}号发布
      </div>
    </div>
    <!--打卡栏-->
    <div class="clock-container already-clock" @click="clickClock">
      <div class="clock">
        <div class="clock-top">
          <i class="iconfont icondaqia"></i>
          {{currentDate | date-format('YYYY-MM-DD')}}
        </div>
        <div class="clock-bottom">
          打卡
        </div>
      </div>
    </div>

    <!--课程单元格    -->
    <div class="sudoku_row"  >
      <div class="sudoku_item " :class="{opacity:curSelect===item.langId, recommend:item.isRecommend == '1', 'first_recommend':item.isRecommend == '1' && index == 0}"
           v-for="(item,index) in languagesInfo"
           :key="index" @touchstart="touchstart(item.langId)" @touchend="touchend" @click="toPaper(item.langId)">
        <img :src=langImg[index].imgSrc  height="50" >
<!--        <img :src=langImg[index].imgSrc width="60" height="50" >-->
<!--        <img :src="item.langImgSrc" width="40" height="40" >-->
        {{item.langName}}
      </div>
    </div>

    <!--    <div id="app">-->

<!--      <iframe src="http://localhost:8090/#/profile/examcalendar" style="height:20%;width:20%;margin-left: 1200px;margin-top: -1400px"></iframe>-->

<!--    </div>-->
<!--    <div class="calendar" @click="toCalendar">-->
<!--      <div class="calendar-left">-->
<!--        <div style="float: left;width: 6px;height: 30px;background-color: #4ab8a1"></div>-->
<!--        <i class="iconfont iconrili"></i>-->
<!--        考试日历-->
<!--      </div>-->
<!--      <div class="calendar-right" v-if="userInfo.sno" >-->
<!--        <i class="iconfont iconxiazai41"></i>-->
<!--        最新考试信息：{{examCalendar[0].teaName}}{{examCalendar[0].noticeCreateTime | date-format('M')}}月{{examCalendar[0].noticeCreateTime | date-format('D')}}号发布-->
<!--      </div>-->
<!--    </div>-->
  </div>
</template>

<script>
import {Toast} from 'mint-ui'
import HeaderTop from '../../components/HeaderTop/HeaderTop.vue'
import Swiper from '../../components/Swiper/Swiper.vue'
import {mapState} from 'vuex'

export default {
  name: '',
  created () {
    this.$store.dispatch('getRotationImages')
    // this.$store.dispatch('getExamCalendar');
    this.$store.dispatch('getLanguagesInfo')
  },
  data () {
    return {
      currentDate: new Date(),
      curSelect: null,
      alreadyClock: false,
      // lunbo: [
      //   {'imgId': 1, 'imgTitle': '轮播图1', 'imgSrc': 'http://cms-bucket.ws.126.net/2021/1213/8cfb02b2p00r41fj700buc000ow009vc.png', 'imgCreateTime': '2019-03-04T17:48:22.000+0000', 'ano': 123456, 'admName': '小张'},
      //   {'imgId': 2, 'imgTitle': '轮播图2', 'imgSrc': 'http://edu-image.nosdn.127.net/7a6b20371fec4deb9749b23f51ec9844.png?imageView&quality=100&type=webp', 'imgCreateTime': '2019-03-04T17:48:45.000+0000', 'ano': 123456, 'admName': '小张'},
      //   {'imgId': 3, 'imgTitle': '轮播图3', 'imgSrc': 'http://edu-image.nosdn.127.net/ea6f481e3edf4a63bec94366ff3492df.png?imageView&quality=100&thumbnail=1522y720', 'imgCreateTime': '2019-03-04T17:49:03.000+0000', 'ano': 123456, 'admName': '小张'},
      //   {'imgId': 4, 'imgTitle': '轮播图4', 'imgSrc': 'https://images.ctfassets.net/18pi2fb74tmp/4xA4DyCYikCiqsuqe26cEG/e83b7f05388bf6600db0eb27e0fc8450/2dc9c25f-a29c-4ed6-9a02-1f424f85e450.jpg', 'imgCreateTime': '2019-03-04T17:49:17.000+0000', 'ano': 123456, 'admName': '小张'},
      //   // {'imgId': 5, 'imgTitle': '测试添加轮播图1', 'imgSrc': 'http://qiniu.maweitao.top/programImages/8f7a39f1-b732-4cf5-b26a-b9c5c70ff272', 'imgCreateTime': '2019-04-20T09:18:40.000+0000', 'ano': 123456, 'admName': '小张'},
      //   // {'imgId': 6, 'imgTitle': '测试添加轮播图2', 'imgSrc': 'http://qiniu.maweitao.top/programImages/b4bcbbbb-599c-4a6a-b055-4f8bef73e25a', 'imgCreateTime': '2019-04-20T09:19:01.000+0000', 'ano': 123456, 'admName': '小张'}
      // ],
      langImg: [
        {
          imgSrc: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg.qinxue365.com%2Fuploads%2Fallimg%2F1802%2F4092-1P226111645106.gif&refer=http%3A%2F%2Fimg.qinxue365.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1642047628&t=9b6f0b5e0faf6aff7abce99dae471a59'
        },
        {
          imgSrc: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fpic1.zhimg.com%2Fv2-3510f7f34b44d2a881f1d07da5ccb125_1440w.jpg%3Fsource%3D172ae18b&refer=http%3A%2F%2Fpic1.zhimg.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1642046091&t=29deeb6cca70b7ee7bb360f1261336d6'
        },
        {
          imgSrc: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20190823%2F9f01af7c421a46008cc9052c6dc710ed.png&refer=http%3A%2F%2F5b0988e595225.cdn.sohucs.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1642046620&t=cde86a3337beeea8ab20385df346aa0e'
        },
        {
          imgSrc: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimages.huishoubao.com.cn%2Fimages%2Fseo%2F20170707%2Fbfc86ab750a9178cb8d1bf2d7a027ff2.png&refer=http%3A%2F%2Fimages.huishoubao.com.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1642047838&t=dbf017c7e215a5dd00b2f475e93f74ac'
        },
        {
          imgSrc: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fmedia1.rrl360.com%2Fproduct%2F0004%2F06%2Fthumb_305321_default.png&refer=http%3A%2F%2Fmedia1.rrl360.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1642046936&t=d46b658b646bd2dd15795162ade964d3'
        },
        {
          imgSrc: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fm.zixuephp.net%2Fuploads%2Fimage%2F20180619%2F1529399147508674.jpg&refer=http%3A%2F%2Fm.zixuephp.net&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1642046964&t=f7fc8fe8148d1783bcf13191d21531a4'
        },
        {
          imgSrc: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fstatic.open-open.com%2Fnews%2FuploadImg%2F20121120%2F20121120195241_143.jpg&refer=http%3A%2F%2Fstatic.open-open.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1642047161&t=8ee76447c68408a559871b32e030f453'
        },
        {
          imgSrc: 'https://www.runoob.com/wp-content/uploads/2015/06/go128.png'
        },
        {
          imgSrc: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fku.90sjimg.com%2Felement_origin_min_pic%2F00%2F85%2F77%2F1956e9e3afe5091.jpg&refer=http%3A%2F%2Fku.90sjimg.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1642047072&t=2516a5a6723146f9819396bfd0be4374'
        }

      ]
    }
  },
  computed: {
    // 轮播图的数组
    ...mapState(['examCalendar', 'rotationImages', 'userInfo', 'languagesInfo'])
  },
  methods: {
    touchstart: function (e) {
      var list = this.$store.state.languagesInfo
      for (var i = 0, len = list.length; i < len; i++) {
        if (list[i].langId === e) {
          this.curSelect = i + 1
        }
      }
      setTimeout(() => {
        this.curSelect = null
      }, 500)
    },
    touchend: function () {
      this.curSelect = null
    },
    toCalendar () {
      if (!this.$store.state.userInfo.sno) {
        Toast({
          message: '请先登录系统',
          duration: 1000
        })
      } else {
        this.$router.push('/profile/examcalendar')
      }
    },
    toPaper (langId) {
      if (!this.$store.state.userInfo.sno) {
        Toast({
          message: '请先登录系统',
          duration: 1000
        })
      } else {
        this.$router.push('/home/paper/' + langId)
      }
    },
    clickClock () {
      if (!this.alreadyClock) {
        Toast({
          message: '恭喜您，打卡成功',
          iconClass: 'iconfont icondaqia1',
          duration: 1500
        })
        this.alreadyClock = true
      } else {
        Toast({
          message: '请勿重复打卡',
          iconClass: 'iconfont iconxinxi',
          duration: 1500
        })
      }
    }
  },
  components: {
    HeaderTop,
    Swiper
  }
}
</script>

<style lang="stylus" type="text/stylus" rel="stylesheet/stylus" scoped>
  @import "../../common/stylus/mixins.styl"
  .home  //首页
    padding-bottom 56px
    width 100%
    background-color #f5f5f5
    .swiper
      padding-top 45px
      width 58%
      height:100%;
      margin 5px auto
      border-radius 15px
      //box-shadow 0px 0px 1px rgba(0,0,0,.5)
    .clock-container
      background-color #fff
      //background-image url("../../common/imgs/clock.png"), url("../../common/imgs/stu-clock.png") ,url("../../common/imgs/good.png")
      //background-size 32px 32px, 24px 24px, 65px 32px
      background-repeat no-repeat, no-repeat, no-repeat
      background-position 100% 0%, 45% 50%, 63% 50%
      margin-top 0px
      height 60px
      color #fff
      &.already-clock
        background-image url("../../common/imgs/stu-clock.png")
        background-size  65px 32px
        background-repeat no-repeat, no-repeat, no-repeat, no-repeat
        background-position  98% 50%
      .clock
        height 100%
        width 33.5%
        display flex
        flex-direction column
        justify-content space-around
        align-items center
        background-color #4ab8a1
        border-radius 0 23px 23px 0
    //#app
    //float right
    .sudoku_row
      display flex
      align-items center
      width 98%
      height: 200%;
      flex-wrap wrap
      //background-color #fff
      margin 20px auto

      .recommend
        background url("../../common/imgs/corner-mark-recommend.png") no-repeat 0% 0%
        background-size 40px 40px
      .first_recommend
        background url("../../common/imgs/corner-mark-recommend.png") no-repeat 0% 0%, url("../../common/imgs/corner-mark-top-right.png") no-repeat 100% 0%
        background-size 40px 40px
      .sudoku_item
        display flex
        border-radius 10px
        justify-content center
        align-items center
        flex-direction column
        width 33%
        margin 2px
        padding-top 15px
        padding-bottom 15px
        box-shadow 0px 0px 1px rgba(0,0,0,.5)
        float left
        background-color #fff
        img
          margin-bottom 3px
          display block
    .opacity
      opacity 0.4
      background #e5e5e5
    .calendar
      display flex
      justify-content space-between
      align-items center
      background-color #fff
      margin-top  0px
      height 50px
      &:active
        opacity 0.4
        background #e5e5e5
      //box-shadow 0px 0px 1px rgba(0,0,0,.5)
      .calendar-left
        display flex
        align-items center
        font-size 12px
        .iconrili
          font-size 20px
          padding-left 10px
          padding-right 10px
      .calendar-right
        display flex
        align-items center
        font-size 12px
        padding-right 10px
        color lightcoral
        font-weight bold
        .iconxiazai41
          font-size 20px
          padding-right 10px
          font-weight bolder
</style>
