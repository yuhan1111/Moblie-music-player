<template>
  <div class="discovery_wrap">
    <div class="swiper_wrap">
      <el-carousel :interval="3000" type="card" height="1.2rem" class="swiper_inner">
        <el-carousel-item v-for="(item, index) in banners" :key="index" class="swiper_inner_item">
          <img :src="item.imageUrl" class="swiper_img" />
        </el-carousel-item>
      </el-carousel>
    </div>
    <div class="playlists_wrap">
      <div class="playlists_title">推荐歌单</div>
      <ul class="playlists_items">
        <li
          class="playlists_item"
          v-for="(item, index) in list"
          :key="index"
          @click="toPlayList(item.id)"
        >
          <div class="playlists_img_wrap">
            <img :src="item.picUrl" />
          </div>
          <p class="playlists_msg">{{item.name}}</p>
        </li>
      </ul>
    </div>
    <div class="songs_wrap">
      <div class="songs_title">每日新歌</div>
      <ul class="songs_items">
        <li
          class="songs_item"
          v-for="(item, index) in songs"
          :key="index"
          @click="PlayMusic(item.id)"
        >
          <div class="songs_img_wrap">
            <img :src="item.picUrl" />
          </div>
          <p class="songs_msg">{{item.name}}</p>
          <p class="songs_artist">{{item.song.artists[0].name}}</p>
        </li>
      </ul>
    </div>
    <div class="mvs_wrap">
      <div class="mvs_title">最新MV</div>
      <ul class="mvs_items">
        <li class="mvs_item" v-for="(item, index) in mvs" :key="index">
          <div class="mvs_img_wrap">
            <img :src="item.picUrl" />
          </div>
          <p class="mvs_msg">{{item.name}}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "discovery",
  data() {
    return {
      banners: [],
      list: [],
      songs: [],
      mvs: []
    };
  },
  created() {
    axios({
      url: "https://autumnfish.cn/banner",
      method: "get",
      params: {
        limit: 6
      }
    }).then(res => {
      // console.log(res);
      this.banners = res.data.banners.slice(0, 6);
    });

    axios({
      url: "https://autumnfish.cn/personalized",
      method: "get",
      params: {
        limit: 12
      }
    }).then(res => {
      // console.log(res);
      this.list = res.data.result;
    });
    axios({
      url: "https://autumnfish.cn/personalized/newsong",
      method: "get"
    }).then(res => {
      // console.log(res);
      this.songs = res.data.result;
    });
    axios({
      url: "https://autumnfish.cn/personalized/mv",
      method: "get"
    }).then(res => {
      console.log(res);
      this.mvs = res.data.result;
    });
  },
  methods: {
    PlayMusic(id) {
      axios({
        url: "https://autumnfish.cn/song/url",
        method: "get",
        params: {
          id: id
        }
      }).then(res => {
        // console.log(res);
        let url = res.data.data[0].url;
        // console.log(this.$parent.musicUrl);
        this.$parent.musicUrl = url;
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.discovery_wrap {
  width: 100%;

  .swiper_wrap {
    width: 100%;
    .swiper_inner {
      .swiper_inner_item {
        height: 1.2rem;
        width: 3rem;
        background-color: yellow;
        .swiper_img {
          width: 100%;
          height: 100%;
        }
      }
    }
  }

  .playlists_wrap {
    width: 100%;
    overflow: hidden;
    margin-bottom: 0.5rem;
    box-sizing: border-box;
    .playlists_title {
      width: 100%;
      height: 0.5rem;
      line-height: 0.5rem;
      text-align: left;
      font-size: 0.3rem;
      font-weight: 700;
    }
    .playlists_items {
      width: 100%;
      .playlists_item {
        position: relative;
        float: left;
        height: 2rem;
        width: 2rem;
        .playlists_img_wrap {
          width: 2rem;
          height: 2rem;
          img {
            width: 100%;
            height: 100%;
          }
        }
        .playlists_msg {
          box-sizing: border-box;
          position: absolute;
          top: 0;
          width: 2rem;
          color: white;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          padding: 0 0.1rem;
          font-size: 0.16rem;
          height: 0.5rem;
          line-height: 0.5rem;
        }
      }
    }
  }

  .songs_wrap {
    width: 100%;
    box-sizing: border-box;
    margin-bottom: 0.5rem;
    overflow: hidden;
    .songs_title {
      width: 100%;
      height: 0.5rem;
      line-height: 0.5rem;
      text-align: left;
      font-size: 0.3rem;
      font-weight: 700;
    }
    .songs_items {
      width: 100%;
      .songs_item {
        float: left;
        box-sizing: border-box;
        height: 1.5rem;
        width: 3rem;
        margin-bottom: 0.2rem;
        background-color: rgba(0, 0, 0, 0.1);
        border-left: 0.1rem solid white;
        border-radius: 0.1rem;
        .songs_img_wrap {
          float: left;
          width: 1.5rem;
          height: 1.5rem;
          img {
            width: 100%;
            height: 100%;
          }
        }
        .songs_msg {
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          padding: 0 0.1rem;
          font-size: 0.16rem;
          height: 0.75rem;
          line-height: 0.75rem;
        }
        .songs_artist {
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          padding: 0 0.1rem;
          font-size: 0.12rem;
          color: #aaccff;
          height: 0.75rem;
          line-height: 0.75rem;
        }
      }
    }
  }

  .mvs_wrap {
    width: 100%;
    overflow: hidden;
    margin-bottom: 0.5rem;
    box-sizing: border-box;
    .mvs_title {
      width: 100%;
      height: 0.5rem;
      line-height: 0.5rem;
      text-align: left;
      font-size: 0.3rem;
      font-weight: 700;
    }
    .mvs_items {
      width: 100%;
      .mvs_item {
        height: 3.9rem;
        width: 6rem;
        margin: 0 auto;
        .mvs_img_wrap {
          width: 100%;
          height: 3.4rem;
          border-radius: 0.1rem;
          overflow: hidden;
          img {
            width: 100%;
            height: 100%;
          }
        }
        .mvs_msg {
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          font-size: 0.16rem;
          padding: 0 0.1rem;
          height: 0.5rem;
          line-height: 0.5rem;
          width: 100%;
        }
      }
    }
  }
}
</style>