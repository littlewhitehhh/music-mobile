<template>
  <div class="mvDynamic">
    <div class="left">
      <div class="useImg">
        <img :src="userImg" alt="" />
      </div>
    </div>
    <div class="right">
      <div class="title">
        <span>{{ nickName }} </span>分享MV:
      </div>
      <div class="time">{{ time | getTime }}</div>
      <div class="content" v-if="events.msg !== ''">{{ events.msg }}</div>
      <div class="video" v-if="events.mv.id !== ''">
        <div class="logo">
          <img :src="events.mv.imgurl16v9" alt="" />
        </div>
        <div class="box" @click="toPlay(events.mv.id)">
          <div class="play">
            <i class="iconfont icon-bofang"></i>
          </div>
          <div class="playDetail">
            <div class="detailLeft">
              <span>{{ playCount }}</span>
            </div>
            <div class="detailRight">
              <div class="rightdetail">
                <span>{{ events.mv.duration | mvTime }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="novideo" v-else>该视频已删除</div>
      <events-count
        @sComment="sComment"
        :commentLength="commentLength"
        :likedCount="likedCount"
        :shareCount="shareCount"
      ></events-count>
    </div>
  </div>
</template>

<script>
import { toStringNum, durationTime } from "common/common";
import eventsCount from "./eventsCount"; // 底部动态数据
export default {
  props: [
    "events",
    "userImg",
    "nickName",
    "time",
    "commentLength",
    "likedCount",
    "shareCount",
    "threadId",
  ],
  components: {
    eventsCount,
  },
  methods: {
    toPlay(id) {
      this.$router.push("/mvplay/" + id);
    },

    sComment() {
      this.$store.state.commentId = this.threadId;
    },
  },
  name: "mvDynamic",
  data() {
    return {
      timeMV: "",
    };
  },
  created() {
    if (this.events.mv.status !== -1) {
      console.log(this.events.mv.duration);
      this.playCount = toStringNum(this.events.mv.playCount);
    }
  },
};
</script>
<style scoped>
.mvDynamic {
  width: 100%;
  display: flex;
  padding-bottom: 0.399467rem;
  margin-top: 0.399467rem;
  border-bottom: 1px solid #e6e6e6;
}
.left {
  flex: 1.5;
}
.useImg {
  width: 1.065246rem;
  height: 1.065246rem;
  border-radius: 50%;
  overflow: hidden;
}
.useImg img {
  width: 100%;
  height: 100%;
}
.right {
  flex: 8.5;
}
.title {
  margin-top: .133333rem;
  font-size: .32rem;
  color: #000;
  height: .4rem;
}
.title span {
  font-size: .32rem;
  color: rgb(34, 110, 197);
}
.time {
  font-size: .32rem;
  color: rgb(158, 158, 158);
  height: .4rem;
}
.content {
  font-size: .36rem;
  line-height: .48rem;
  margin-top: 0.213049rem;
}
.video {
  width: 100%;
  height: 3.994674rem;
  margin-top: 0.213049rem;
  border-radius: 5px;
  overflow: hidden;
  position: relative;
}
.novideo {
  width: 100%;
  height: 1.065246rem;
  margin-top: 0.213049rem;
  line-height: 1.065246rem;
  text-align: center;
  border-radius: 0.213049rem;
  background-color: #f1f0f0;
  color: rgb(170, 170, 170);
  font-size: 0.372836rem;
}
.logo {
  width: 100%;
  height: 100%;
  position: absolute;
}
.logo img {
  width: 100%;
  height: 100%;
}
.box {
  width: 100%;
  height: 100%;
  position: relative;
  z-index: 0;
  background-color: rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
}
.playDetail {
  width: 100%;
  height: 0.532623rem;
  position: absolute;
  bottom: 0;
  color: #fff;
  font-size: 0.372836rem;
  display: flex;
}
.play {
  margin: auto;
}
.play .iconfont {
  font-size: .8rem;
  color: rgba(255, 255, 255, 0.692);
}
.detailLeft {
  flex: 1;
  line-height: 0.532623rem;
  text-indent: 0.213049rem;
  font-size: .32rem;
  float: left;
}
.detailRight {
  flex: 1;
  line-height: 0.532623rem;
  text-indent: 0.213049rem;
}
.rightdetail {
  float: right;
  font-size: .32rem;
}
.rightdetail img {
  width: 0.372836rem;
  margin-right: 0.133156rem;
}
.rightdetail span {
  margin-right: 0.213049rem;
}
</style>