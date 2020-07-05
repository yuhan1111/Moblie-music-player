<template>
  <div class="mvs-container">
    <!-- 筛选器 -->
    <div class="header_wrap">
      <!-- 分类切换-地区 -->
      <div class="filter_wrap_area">
        <span class="filter_wrap_area_title">地区：</span>
        <div class="filter_wrap_area_items">
          <span class="item" :class="{active:area=='全部'}" @click="area='全部'">全部</span>
          <span class="item" :class="{active:area=='内地'}" @click="area='内地'">内地</span>
          <span class="item" :class="{active:area=='港台'}" @click="area='港台'">港台</span>
          <span class="item" :class="{active:area=='欧美'}" @click="area='欧美'">欧美</span>
          <span class="item" :class="{active:area=='日本'}" @click="area='日本'">日本</span>
          <span class="item" :class="{active:area=='韩国'}" @click="area='韩国'">韩国</span>
        </div>
      </div>
      <div class="filter_wrap_type">
        <span class="filter_wrap_type_title">类型：</span>
        <div class="filter_wrap_type_items">
          <span class="item" :class="{active:type=='全部'}" @click="type='全部'">全部</span>
          <span class="item" :class="{active:type=='官方版'}" @click="type='官方版'">官方版</span>
          <span class="item" :class="{active:type=='原声'}" @click="type='原声'">原声</span>
          <span class="item" :class="{active:type=='现场版'}" @click="type='现场版'">现场版</span>
          <span class="item" :class="{active:type=='BA出品'}" @click="type='BA出品'">BA出品</span>
        </div>
      </div>
      <div class="filter_wrap_order">
        <span class="filter_wrap_order_title">排序：</span>
        <div class="filter_wrap_order_items">
          <span class="item" :class="{active:order=='上升最快'}" @click="order='上升最快'">上升最快</span>
          <span class="item" :class="{active:order=='最热'}" @click="order='最热'">最热</span>
          <span class="item" :class="{active:order=='最新'}" @click="order='最新'">最新</span>
        </div>
      </div>
    </div>
    <!-- 推荐MV -->
    <div class="body_wrap">
      <ul class="items_wrap">
        <li class="item" v-for="(item,index) in list" :key="index" @click="toMVPlayer(item.id)">
          <!-- 海报 -->
          <div class="item_img_wrap">
            <img :src="item.cover" />
            <div class="img_num_wrap">
              <span class="el-icon-view m-icon"></span>
              <span class="num">{{item.playCount}}</span>
            </div>
            <div class="img_duration_wrap">{{item.duration}}</div>
          </div>
          <!-- mv标题作者 -->
          <div class="item_msg_wrap">
            <div class="item_msg_title">{{item.name}}</div>
            <div class="item_msg_author">{{item.artists[0].name}}</div>
          </div>
          <!-- 播放量 -->

          <!-- 时长 -->
        </li>
      </ul>
    </div>
    <!-- 分页 -->
    <div class="footer_wrap">
      <el-pagination
        class="m-page"
        @current-change="handleCurrentChange"
        small
        layout="prev, pager, next"
        :total="total"
        :current-page="page"
        :page-size="12"
        :limit="limit"
      ></el-pagination>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "mvs",
  data() {
    return {
      list: [],

      total: 20,

      page: 1,

      limit: 12,

      area: "全部",

      type: "全部",

      order: "上升最快"
    };
  },
  watch: {
    area() {
      this.getList();
      this.page = 1;
    },
    type() {
      this.getList();
      this.page = 1;
    },
    order() {
      this.getList();
      this.page = 1;
    }
  },
  created() {
    this.getList();
  },
  methods: {
    getList() {
      axios({
        url: "https://autumnfish.cn/mv/all",
        method: "get",
        params: {
          area: this.area,
          type: this.type,
          arder: this.order,
          limit: this.limit,
          offset: (this.page - 1) * this.limit
        }
      }).then(res => {
        // console.log(res);
        this.list = res.data.data;
        for (let i = 0; i < this.list.length; i++) {
          this.list[i].playCount =
            this.list[i].playCount > 10000
              ? parseInt(this.list[i].playCount / 10000) + "万"
              : this.list[i].playCount;
        }
        for (let i = 0; i < this.list.length; i++) {
          let duration = this.list[i].duration;
          let min = parseInt(duration / 1000 / 60);
          min = min < 10 ? "0" + min : min;
          let sec = parseInt((duration / 1000) % 60);
          sec = sec < 10 ? "0" + sec : sec;
          // console.log(min +':'+ sec);
          this.list[i].duration = `${min}:${sec}`;
        }
        if (res.data.count) {
          this.total = res.data.count;
        }
        // console.log(this.total);
      });
    },
    handleCurrentChange(val) {
      // console.log(`当前页:${val}`);
      this.page = val;
      this.getList();
    },
    toMVPlayer(id) {
      this.$router.push(`/mv?q=${id}`);
    }
  }
};
</script>

<style lang="scss" scoped>
.mvs-container {
  width: 6rem;
  .header_wrap {
    box-sizing: border-box;
    width: 100%;
    margin: 0.2rem auto;
    background-color: #acf;
    border-radius: 0.1rem;
    box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
    font-size: 0.2rem;
    padding: 0.1rem;
    .filter_wrap_area {
      width: 100%;
      .filter_wrap_area_title {
        float: left;
      }

      .filter_wrap_area_items {
        .item {
          padding: 0.1rem 0.1rem;
        }
      }
      .active {
        color: yellow;
      }
    }

    .filter_wrap_type {
      .filter_wrap_type_title {
        float: left;
      }

      .filter_wrap_type_items {
        .item {
          padding: 0.1rem 0.1rem;
        }
        .active {
          color: yellow;
        }
      }
    }

    .filter_wrap_order {
      .filter_wrap_order_title {
        float: left;
      }
      .filter_wrap_order_items {
        .item {
          padding: 0.1rem 0.1rem;
        }
        .active {
          color: yellow;
        }
      }
    }
  }
  .body_wrap {
    .items_wrap {
      width: 100%;
      overflow: hidden;
      margin-bottom: 0.1rem;
      .item {
        display: flex;
        font-size: 0.2rem;
        width: 6rem;
        height: 2.247rem;
        overflow: hidden;
        margin-bottom: 0.2rem;
        background-color: rgba(0, 0, 0, 0.3);
        border-radius: 0.1rem;
        box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
        .item_img_wrap {
          position: relative;
          float: left;
          width: 4rem;
          box-sizing: border-box;
          img {
            width: 100%;
          }
          .img_num_wrap {
            position: absolute;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 0.15rem;
            padding: 0 0.1rem;
            font-size: 0.05rem;
            top: 0.05rem;
            left: 0;
            color: white;
            .m-icon {
              margin-right: 0.1rem;
            }
          }
          .img_duration_wrap {
            position: absolute;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 0.15rem;
            padding: 0 0.1rem;
            font-size: 0.05rem;
            bottom: 0.1rem;
            left: 0;
            color: white;
          }
        }
        .item_msg_wrap {
          flex: 1;
          overflow: hidden;
          width: 2rem;

          .item_msg_title {
            box-sizing: border-box;
            padding: 0.1rem;
            overflow: hidden;
            width: 100%;
          }

          .item_msg_author {
            width: 100%;
            box-sizing: border-box;
            padding: 0.1rem;
            color: #ffffaa;
          }
        }
      }
    }
  }

  .footer_wrap {
    text-align: center;

    .p-page {
      box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
      width: 6rem;
      margin-bottom: 0.2rem;
      width: 6rem;
      height: 0.4rem;
    }
  }
}
</style>