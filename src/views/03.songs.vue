<template>
  <div class="songs-container">
    <!-- 顶部tab栏 开始 -->
    <div class="header_wrap">
      <span class="item" @click="tag=0" :class="{active:tag==0}">全部</span>
      <span class="item" @click="tag=7" :class="{active:tag==7}">华语</span>
      <span class="item" @click="tag=96" :class="{active:tag==96}">欧美</span>
      <span class="item" @click="tag=8" :class="{active:tag==8}">日本</span>
      <span class="item" @click="tag=16" :class="{active:tag==16}">韩国</span>
    </div>
    <!-- 顶部tab栏 结束 -->
    <!-- 底部table栏 开始 -->
    <table class="el-table body_wrap">
      <thead class="body_title_wrap">
        <th class="item"></th>
        <th class="item">歌曲名</th>
        <th class="item">歌手</th>
      </thead>
      <tbody class="body_content_wrap">
        <tr
          class="el-table__row item_wrap"
          v-for="(item, index) in lists"
          :key="index"
          @click="playMusic(item.id,item.album.name,item.artists[0].name,item.album.picUrl,index)"
        >
          <td class="item">{{index+1}}</td>
          <td class="item">{{item.album.name}}</td>
          <td class="item">{{item.artists[0].name}}</td>
        </tr>
      </tbody>
    </table>
    <!-- 底部table栏 开始 -->
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "songs",
  data() {
    return {
      lists: [],
      tag: "0"
    };
  },
  watch: {
    tag() {
      this.getList();
    }
  },
  created() {
    this.getList();
  },
  methods: {
    getList() {
      axios({
        url: "https://autumnfish.cn/top/song",
        method: "get",
        params: {
          type: this.tag,
          limit: 100
        }
      }).then(res => {
        // console.log(res);
        this.lists = res.data.data;
        for (let i = 0; i < this.lists.length; i++) {
          let duration = this.lists[i].duration;
          let min = parseInt(duration / 1000 / 60);
          min = min < 10 ? "0" + min : min;
          let sec = parseInt((duration / 1000) % 60);
          sec = sec < 10 ? "0" + sec : sec;
          // console.log(min +':'+ sec);
          this.lists[i].duration = `${min}:${sec}`;
        }
      });
    },
    playMusic(id, name, rname, purl, index) {
      this.$parent.musicName = name;
      this.$parent.rName = rname;
      this.$parent.picUrl = purl;
      this.$parent.currentIndex = index;

      axios({
        url: "https://autumnfish.cn/song/url",
        method: "get",
        params: {
          id: id
        }
      }).then(res => {
        // console.log(res);
        let url = res.data.data[0].url;
        // console.log(url);
        // console.log(this.$parent);
        this.$parent.musicUrl = url;
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.songs-container {
  width: 6rem;
  .header_wrap {
    display: flex;
    box-sizing: border-box;
    width: 100%;
    text-align: center;
    margin: 0.2rem auto;
    background-color: #acf;
    border-radius: 0.1rem;
    box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
    .item {
      font-size: 0.3rem;
      padding: 0.2rem;
      font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    .active {
      background-color: #ccc;
      font-weight: 700;
      color: white;
      border-radius: 0.1rem;
    }
  }

  .body_wrap {
    border-collapse: collapse;
    background-color: #aac2e2;
    .body_title_wrap {
        width: 5.5rem;
      .item {
        color: darkblue;
      }
    }

    .body_content_wrap {
      .item_wrap {
        font-size: 0.3rem;
        .item {
          border: none;
        }
        .item:nth-child(1) {
          text-align: center;
        }
      }
      .item_wrap:nth-child(2n + 1) {
        background-color: #bbffff;
      }
    }
  }
}
</style>