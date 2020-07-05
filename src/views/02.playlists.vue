<template>
  <div class="playlists-container">
    <!-- 顶部精选 开始 -->
    <div class="header_wrap">
      <div class="header_inner_wrap">
        <div class="header_top_wrap">
          <div class="header_top_left_wrap">
            <img :src="topList.coverImgUrl" alt />
          </div>
          <div class="header_top_right_wrap">
            <div class="brand">《精品歌单》</div>
            <div class="title">{{topList.name}}</div>
          </div>
        </div>
        <div class="header_bottom_wrap">{{topList.description}}</div>
      </div>
      <div class="header_background">
        <img :src="topList.coverImgUrl" />
      </div>
    </div>
    <!-- 顶部精选 结束 -->
    <!-- tab栏顶部 -->
    <div class="body_wrap">
      <div class="tabs_wrap">
        <span class="item" :class="{active:tag=='全部'}" @click="tag='全部'">全部</span>
        <span class="item" :class="{active:tag=='欧美'}" @click="tag='欧美'">欧美</span>
        <span class="item" :class="{active:tag=='华语'}" @click="tag='华语'">华语</span>
        <span class="item" :class="{active:tag=='流行'}" @click="tag='流行'">流行</span>
        <span class="item" :class="{active:tag=='说唱'}" @click="tag='说唱'">说唱</span>
        <span class="item" :class="{active:tag=='摇滚'}" @click="tag='摇滚'">摇滚</span>
        <span class="item" :class="{active:tag=='民谣'}" @click="tag='民谣'">民谣</span>
        <span class="item" :class="{active:tag=='电子'}" @click="tag='电子'">电子</span>
        <span class="item" :class="{active:tag=='轻音乐'}" @click="tag='轻音乐'">轻音乐</span>
        <span class="item" :class="{active:tag=='影视原声'}" @click="tag='影视原声'">影视原声</span>
        <span class="item" :class="{active:tag=='ACG'}" @click="tag='ACG'">ACG</span>
        <span class="item" :class="{active:tag=='怀旧'}" @click="tag='怀旧'">怀旧</span>
        <span class="item" :class="{active:tag=='治愈'}" @click="tag='治愈'">治愈</span>
        <span class="item" :class="{active:tag=='旅行'}" @click="tag='旅行'">旅行</span>
      </div>
      <!-- tab的内容区域 -->
      <ul class="content_wrap">
        <li
          class="content_wrap_item"
          v-for="(item, index) in list"
          :key="index"
          @click="toPlayList(item.id)"
        >
          <div class="item_img_wrap">
            <img :src="item.coverImgUrl" alt />
          </div>
          <div class="item_playCount_wrap">{{item.playCount}}</div>
          <div class="item_title_wrap">{{item.name}}</div>
        </li>
      </ul>
      <div class="page_wrap">
        <el-pagination
          class="p-page"
          @current-change="handleCurrentChange"
          small
          layout="prev, pager, next"
          :total="total"
          :current-page="page"
          :page-size="10"
        ></el-pagination>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "recommand",
  data() {
    return {
      total: 0,
      page: 1,
      topList: {},
      list: [],
      tag: "全部"
    };
  },
  watch: {
    tag() {
      this.topData();
      this.listData();
      this.page = 1;
    }
  },
  created() {
    this.topData();
    this.listData();
  },
  methods: {
    topData() {
      axios({
        url: "https://autumnfish.cn/top/playlist/highquality",
        method: "get",
        params: {
          limit: 1,
          cat: this.tag
        }
      }).then(res => {
        // console.log(res);
        this.topList = res.data.playlists[0];
      });
    },

    listData() {
      axios({
        url: "https://autumnfish.cn/top/playlist/",
        method: "get",
        params: {
          limit: 10,
          offset: (this.page - 1) * 10,
          cat: this.tag
        }
      }).then(res => {
        // console.log(res);
        this.total = res.data.total;
        this.list = res.data.playlists;
      });
    },
    toPlayList(id) {
      this.$router.push(`/playlist?q=${id}`);
    },
    handleCurrentChange(val) {
      this.page = val;
      this.listData();
    }
  }
};
</script>

<style lang="scss" scoped>
.playlists-container {
  width: 6rem;
  .header_wrap {
    position: relative;
    width: 100%;
    height: 6rem;
    border-radius: 0.1rem;
    overflow: hidden;
    box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
    .header_inner_wrap {
      position: absolute;
      box-sizing: border-box;
      width: 6rem;
      padding: 0.1rem;
      top: 0;
      left: 0;
      .header_top_wrap {
        width: 100%;
        height: 1.5rem;
        display: flex;
        border-radius: 0.1rem;
        margin-bottom: 0.1rem;
        background-color: rgba(0,0,0,.5);
        .header_top_left_wrap {
          float: left;
          width: 1.5rem;
          height: 100%;
          box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
          border-radius: 0.1rem;
          overflow: hidden;
          img {
            width: 100%;
            height: 100%;
          }
        }
        .header_top_right_wrap {
          flex: 1;
          .brand {
            box-sizing: border-box;
            padding: 0.1rem;
            font-size: 0.3rem;
            font-weight: 700;
            line-height: 0.7rem;
            color: yellowgreen;
            width: 100%;
            height: 0.7rem;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
          }
          .title {
            box-sizing: border-box;
            padding: 0.1rem;
            font-size: 0.1rem;
            width: 100%;
            height: 0.8rem;
            color: #aec;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
          }
        }
      }
      .header_bottom_wrap {
        box-sizing: border-box;
        width: 100%;
        padding: 0.1rem;
        height: 4.2rem;
        font-size: 0.1rem;
        color: #aaf;
        border-radius: 0.1rem;
        background-color: rgba(0, 0, 0, 0.5);
        overflow: hidden;
        box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
      }
    }
    .header_background {
      width: 100%;
      img {
        width: 100%;
        border-radius: 0.1rem;
      }
    }
  }

  .body_wrap {
    width: 100%;
    .tabs_wrap {
      text-align: center;
      width: 100%;
      line-height: 0.4rem;
      font-size: 0.1rem;
      margin: 0.2rem auto;
      .item {
        padding: 0 0.12rem;
      }
      .active{
        background-color: yellow;
        font-weight: 700;
      }
    }
    .content_wrap {
      width: 100%;
      font-size: 0.2rem;
      overflow: hidden;
      .content_wrap_item {
        position: relative;
        float: left;
        width: 2.9rem;
        margin-bottom: 0.1rem;
        margin-right: 0.1rem;
        border-radius: 0.1rem;
        overflow: hidden;
        background-color: rgba(0, 0, 0, 0.2);
        box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
        .item_img_wrap {
          float: left;
          width: 1.5rem;
          height: 1.5rem;
          overflow: hidden;
          img {
            width: 100%;
            height: 100%;
          }
        }
        .item_playCount_wrap {
          position: absolute;
          top: 0;
          left: 0;
          color: white;
        }

        .item_title_wrap {
          box-sizing: border-box;
          padding: 0.1rem;
          float: left;
          width: 1.4rem;
          height: 1.5rem;
          font-size: 0.1rem;
          overflow: hidden;
        }
      }
    }
    .page_wrap {
      width: 6rem;
      margin-bottom: 0.8rem;
      text-align: center;
      box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
      overflow: hidden;
      .m-page {
        width: 6rem;
        height: 0.2rem;
      }
    }
  }
}
</style>