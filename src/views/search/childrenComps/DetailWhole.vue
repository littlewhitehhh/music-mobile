<template>
  <div class="DetailWhole">
    <!-- 歌曲 -->
    <div class="songs bbox" v-if="songs.length !== 0">
      <div class="title">单曲</div>
      <song-item :shouLeft="false" :songList="songs"></song-item>
      <whole-more
        v-if="songText !== undefined"
        :moreText="songText"
      ></whole-more>
    </div>
    <!-- 专辑 -->
    <div class="album bbox" v-if="album.length !== 0">
      <div class="title">专辑</div>
      <div
        class="albumItem"
        @click="toAlbum(item.id)"
        v-for="(item, index) in album"
        :key="index"
      >
        <div class="left">
          <img class="img" :src="item.picUrl" alt="" />
          <img class="black" src="https://img.coolcr.cn/2021/03/08/2b7d972a3ad72.png" alt="" />
        </div>
        <div class="right">
          <div class="box">
            <div class="name">{{ item.name }}</div>
            <div class="detail">
              {{ item.artist }} {{ item.publishTime | getTime("YYYY.MM.DD") }}
            </div>
          </div>
        </div>
      </div>
      <whole-more
        v-if="albumText !== undefined"
        :moreText="albumText"
      ></whole-more>
    </div>
    <!-- 歌单 -->
    <div class="sheet bbox" v-if="sheets.length !== 0">
      <div class="title">歌单</div>
      <div
        class="sheetItem"
        @click="toSheet(item.id)"
        v-for="(item, index) in sheets"
        :key="index"
      >
        <div class="sleft">
          <img :src="item.coverImgUrl" alt="" />
        </div>
        <div class="sright">
          <div class="box">
            <div class="stitle">{{ item.name }}</div>
            <div class="sheetdetail">
              {{ item.trackCount + "首 " }}{{ "by " + item.creator + "，"
              }}{{ "播放" + item.playCount + "次" }}
            </div>
          </div>
        </div>
      </div>
      <whole-more
        v-if="sheetText !== undefined"
        :moreText="sheetText"
      ></whole-more>
    </div>
    <!-- 视频 -->
    <div class="video bbox" v-if="videos.length !== 0">
      <div class="title">视频</div>
      <div class="videoItem" v-for="(item, index) in videos" :key="index">
        <div class="vleft">
          <img :src="item.coverUrl" alt="" />
          <div class="count">▷ {{ item.playTime }}</div>
        </div>
        <div class="vright">
          <div class="vbox">
            <div class="vtitle">{{ item.title }}</div>
            <div class="videoDetail">
              {{ item.durationms + " by " }}{{ item.creator }}
            </div>
          </div>
        </div>
      </div>
      <whole-more
        v-if="videosText !== undefined"
        :moreText="videosText"
      ></whole-more>
    </div>
    <!-- 相关搜索 -->
    <div class="sim_query bbox" v-if="sim_query.length !== 0">
      <div class="title">相关搜索</div>
      <div class="simItem" v-for="(item, index) in sim_query" :key="index">
        {{ item }}
      </div>
      <div style="clear:both"></div>
    </div>
    <!-- 主题 -->
    <div class="talk bbox" v-if="talks.length !== 0">
      <div class="title">主题</div>
      <div class="talkItem" v-for="(item, index) in talks" :key="index">
        <div class="tleft">
          <img :src="item.coverUrl" alt="" />
        </div>
        <div class="tright">
          <div class="tbox">
            <div class="ttitle"># {{ item.talkName }}</div>
            <div class="talkDetail">
              {{ item.follows + "人关注 "
              }}{{ " " + item.participations + "人参与" }}
            </div>
          </div>
        </div>
      </div>
      <whole-more
        v-if="talksText !== undefined"
        :moreText="talksText"
      ></whole-more>
    </div>
    <!-- 歌手 -->
    <div class="artist bbox" v-if="artist.length !== 0">
      <div class="title">歌手</div>
      <div
        class="artistItem"
        @click="toDetail(item.accountId)"
        v-for="(item, index) in artist"
        :key="index"
      >
        <div class="aleft">
          <img :src="item.picUrl" alt="" />
        </div>
        <div class="aright">
          {{ item.name
          }}<span class="a1" v-if="item.alia.length !== 0">
            ({{ item.alia[0] }})</span
          >
        </div>
      </div>
      <whole-more
        v-if="artistText !== undefined"
        :moreText="artistText"
      ></whole-more>
    </div>
    <!-- 电台 -->
    <div class="djRadio bbox" v-if="djRadio.length !== 0">
      <div class="title">电台</div>
      <div
        class="djItem"
        @click="toDjradio(item.id)"
        v-for="(item, index) in djRadio"
        :key="index"
      >
        <div class="dleft">
          <img :src="item.picUrl" alt="" />
        </div>
        <div class="dright">
          <div class="dbox">
            <div class="dname">{{ item.name }}</div>
            <div class="dnickname">{{ item.nickname }}</div>
          </div>
        </div>
      </div>
      <whole-more
        v-if="djRadioText !== undefined"
        :moreText="djRadioText"
      ></whole-more>
    </div>
  </div>
</template>

<script>
import songItem from "components/context/songItem/SongItem";
import WholeMore from "./WholeMore";
import { searchResult } from "network/search";
import { toStringNum, durationTime } from "common/common";
export default {
  name: "DetailWhole",
  data() {
    return {
      offset: 0,
      album: [], // 专辑数据
      songs: [], // 歌曲数据
      sheets: [], // 歌单数据
      videos: [], // 视频数据
      talks: [], // 主题
      artist: [], // 歌手
      sim_query: [], // 相关搜索
      djRadio: [], // 电台数据
      songText: "",
      albumText: "",
      sheetText: "",
      videosText: "",
      talksText: "",
      artistText: "",
      djRadioText: "",
    };
  },
  components: {
    songItem,
    WholeMore,
  },
  methods: {
    // 歌单跳转
    toSheet(id) {
      this.$router.push("/playDetail/" + id + "&" + false);
    },
    // 专辑跳转
    toAlbum(id) {
      this.$router.push("/playDetail/" + id + "&" + true);
    },
    // 歌手跳转
    toDetail(id) {
      if (id !== null) {
        this.$router.push("/Information/" + id);
      } else {
        this.$toast.show("暂时获取不到歌手信息", 1900);
      }
    },
    // 电台跳转
    toDjradio(id) {
      this.$router.push("/stationDetail/" + id);
    },

    getSearchResult() {
      // 获取专辑信息
      searchResult(
        this.$route.params.keywords,
        30,
        this.offset * 30,
        1018
      ).then((res) => {
        if (Object.keys(res.data.result.album).length !== 0) {
          this.albumText = res.data.result.album.moreText;
          for (const item of res.data.result.album.albums) {
            this.album.push({
              picUrl: item.picUrl,
              id: item.id,
              name: item.name,
              artist: item.artist.name,
              publishTime: item.publishTime,
            });
          }
        }
        if (Object.keys(res.data.result.song).length !== 0) {
          this.songText = res.data.result.song.moreText;
          for (const item of res.data.result.song.songs) {
            this.songs.push({
              id: item.id,
              songName: item.name,
              yuanc: [],
              singer: item.ar,
              zhuanji: item.al.name,
              mv: item.mv,
            });
          }
        }

        if (Object.keys(res.data.result.playList).length !== 0) {
          this.sheetText = res.data.result.playList.moreText;
          for (const item of res.data.result.playList.playLists) {
            this.sheets.push({
              id: item.id,
              name: item.name,
              trackCount: item.trackCount,
              playCount: toStringNum(item.playCount),
              coverImgUrl: item.coverImgUrl,
              creator: item.creator.nickname,
            });
          }
        }

        if (Object.keys(res.data.result.video).length !== 0) {
          this.videosText = res.data.result.video.moreText;
          for (const item of res.data.result.video.videos) {
            this.videos.push({
              id: item.id,
              title: item.title,
              durationms: durationTime(item.durationms),
              playTime: toStringNum(item.playTime),
              coverUrl: item.coverUrl,
              creator: item.creator[0].userName,
            });
          }
        }
        if (Object.keys(res.data.result.talk).length !== 0) {
          this.talksText = res.data.result.talk.moreText;
          for (const item of res.data.result.talk.talks) {
            this.talks.push({
              talkName: item.talkName,
              follows: toStringNum(item.follows), // 关注
              participations: toStringNum(item.participations), // 参与
              coverUrl: item.shareCover.url,
            });
          }
        }
        if (Object.keys(res.data.result.artist).length !== 0) {
          this.artistText = res.data.result.artist.moreText;
          for (const item of res.data.result.artist.artists) {
            this.artist.push({
              name: item.name,
              alia: item.alias,
              accountId: item.accountId || null,
              picUrl: item.picUrl,
            });
          }
        }
        for (const item of res.data.result.sim_query.sim_querys) {
          this.sim_query.push(item.keyword);
        }
        if (Object.keys(res.data.result.djRadio).length !== 0) {
          this.djRadioText = res.data.result.djRadio.moreText;
          for (const item of res.data.result.djRadio.djRadios) {
            this.djRadio.push({
              id: item.id,
              name: item.name,
              picUrl: item.picUrl,
              nickname: item.dj.nickname,
            });
          }
        }
      });
    },
  },
  created() {
    this.$loading.loadingShow();
    this.getSearchResult();
  },
  mounted() {
    this.$nextTick(() => {
      this.$loading.loadingNo();
    });
  },
};
</script>
<style scoped>
.bbox {
  margin-bottom: 0.532623rem;
}
.DetailWhole {
  width: 100%;
  min-height: 13.315579rem;
  margin-bottom: 0.133156rem;
  margin-top: 2.396804rem;
}
.title {
  width: 100%;
  height: 0.665779rem;
  line-height: 0.665779rem;
  font-size: .4rem;
  margin-bottom: 0.133156rem;
  clear: both;
}
.albumItem {
  width: 100%;
  height: 2.130493rem;
  display: flex;
  margin-bottom: 0.213049rem;
}
.sheetItem {
  height: 2.130493rem;
  display: flex;
  margin-bottom: 0.213049rem;
  width: 100%;
}
.left {
  flex: 3;
  position: relative;
}
.left .img {
  width: 2.130493rem;
  height: 2.130493rem;
  border-radius: 0.133156rem;
}
.box {
  margin-left: 0.106525rem;
}
.black {
  position: absolute;
  width: 80%;
  height: 100%;
  right: .266667rem;
  z-index: -1;
}
.right {
  flex: 7;
  display: flex;
  align-items: center;
}
.name {
  font-size: 0.399467rem;
  line-height: .48rem;
  margin-bottom: .133333rem;
}
.detail {
  font-size: 0.319574rem;
  color: #a7a7a8;
}
.item {
  display: flex;
  height: 1.198402rem;
  width: 100%;
  margin-bottom: 0.399467rem;
}
.sleft {
  flex: 3;
}
.sright {
  flex: 7;
  display: flex;
  align-items: center;
}
.sleft img {
  width: 2.130493rem;
  height: 2.130493rem;
  border-radius: 0.133156rem;
}
.stitle {
  width: 7.190413rem;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  font-size: 0.372836rem;
  margin-left: 0.133156rem;
  margin-bottom: .133333rem;
}
.sheetdetail {
  width: 7.190413rem;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  font-size: 0.319574rem;
  color: rgb(150, 150, 150);
  height: 0.399467rem;
  line-height: 0.399467rem;
  margin-left: 0.133156rem;
}
.videoItem {
  display: flex;
  height: 1.997337rem;
  margin-bottom: 0.213049rem;
}
.vleft {
  flex: 4;
  position: relative;
}
.count {
  position: absolute;
  right: 0.319574rem;
  top: 0;
  color: #fff;
  font-size: 0.319574rem;
}
.vleft img {
  width: 3.462051rem;
  height: 100%;
  border-radius: 0.133156rem;
}
.vright {
  flex: 6;
  display: flex;
  align-items: center;
}
.vtitle {
  text-overflow: -o-ellipsis-lastline;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  font-size: 0.346205rem;
  margin-bottom: .133333rem;
}
.videoDetail {
  font-size: 0.319574rem;
  color: #a7a7a8;
}
.talkItem {
  display: flex;
  height: 1.864181rem;
  margin-bottom: 0.213049rem;
}
.tleft {
  flex: 2.3;
}
.tleft img {
  width: 1.864181rem;
  height: 1.864181rem;
  border-radius: 0.133156rem;
}
.tright {
  flex: 7.7;
  display: flex;
  align-items: center;
}
.ttitle {
  font-size: 0.399467rem;
  margin-bottom: .133333rem;
}
.talkDetail {
  font-size: 0.319574rem;
  color: #a7a7a8;
}
.artistItem {
  display: flex;
  height: 2.130493rem;
  margin-bottom: .213333rem;
}
.aleft {
  flex: 2.5;
}
.aleft img {
  width: 2.130493rem;
  height: 2.130493rem;
  border-radius: 50%;
}
.aright {
  flex: 7;
  line-height: 2.130493rem;
  font-size: 0.45273rem;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  word-break: break-all;

}
.a1 {
  color: #a7a7a8;
}
.sim_query {
  margin-bottom: 0.532623rem;
}
.simItem {
  float: left;
  padding: 0.133156rem .266667rem;
  font-size: .32rem;
  border-radius: 0.798935rem;
  margin-right: 0.186418rem;
  margin-bottom: 0.186418rem;
  background-color: rgb(253, 253, 253);
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.158);
}
.djItem {
  width: 100%;
  height: 2.130493rem;
  display: flex;
  margin-bottom: 0.213049rem;
}
.dleft {
  flex: 2;
}
.dleft img {
  width: 2.130493rem;
  height: 2.130493rem;
  border-radius: 0.133156rem;
}
.dright {
  flex: 8;
  display: flex;
  align-items: center;
}
.dbox {
  margin-left: 0.266312rem;
}
.dname {
  width: 6.391478rem;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  font-size: 0.399467rem;
  margin-bottom: .133333rem;
}
.dnickname {
  font-size: 0.319574rem;
  color: #a7a7a8;
}
</style>