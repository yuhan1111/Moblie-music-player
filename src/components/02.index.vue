<template>
  <div class="index_container">
    <!-- 左侧导航 -->
    <div class="index_nav_wrap">
      <ul class="index_nav_inner">
        <li class="discovery">
          <router-link to="/discovery" class="el-icon-mobile"></router-link>
        </li>
        <li class="playlist">
          <router-link to="/playlists" class="el-icon-document"></router-link>
        </li>
        <li class="songs">
          <router-link to="/songs" class="el-icon-headset"></router-link>
        </li>
        <li class="mvs">
          <router-link to="/mvs" class="el-icon-video-play"></router-link>
        </li>
      </ul>
    </div>
    <!-- 右侧容器 -->
    <div class="index_main_wrap">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: "index",
  data() {
    return {
      musicUrl: ""
    };
  },
  components: {}
};
</script>

<style lang="scss" scoped>
.index_container {
  display: flex;
  width: 100%;
  z-index: 999;
  .index_nav_wrap {
    width: 1rem;
    background: #fff2e2;
    font-size: 0.6rem;
    margin-right: 0.1rem;
    border-radius: 0.1rem;
    box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
    .index_nav_inner {
      margin-top: 0.3rem;
      margin-bottom: 0.3rem;
      .discovery {
        width: 100%;
        text-align: center;
        height: 1.2rem;
        line-height: 1.2rem;
        font-weight: 700;
      }

      .playlist {
        width: 100%;
        text-align: center;
        height: 1.2rem;
        line-height: 1.2rem;
        font-weight: 700;
      }

      .songs {
        width: 100%;
        text-align: center;
        height: 1.2rem;
        line-height: 1.2rem;
        font-weight: 700;
      }

      .mvs {
        width: 100%;
        text-align: center;
        height: 1.2rem;
        line-height: 1.2rem;
        font-weight: 700;
      }
      .router-link-active {
        color: red;
      }
    }
  }
  .index_main_wrap {
    flex: 1;
    background-color: #fff;
    padding: 0.2rem;
    box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
    padding-bottom: 20px;
    position: relative;
  }
}
</style>