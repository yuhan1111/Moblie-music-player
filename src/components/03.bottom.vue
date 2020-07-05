<template>
  <div class="bottom_container">
    <div class="info_wrap">
      <div class="info_wrap_poster">
        <img src="" alt />
      </div>
    </div>
    <div class="play_wrap">
      <div class="play_wrap_name">
        <div class="name_artist">李荣浩</div>
        <div class="name_song">麻雀</div>
      </div>
      <div class="play_wrap_player">
        <audio src controls autoplay loop>当前浏览器不支持播放</audio>
      </div>
    </div>
    <div class="conduct_wrap"></div>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped>
.bottom_container {
  position: fixed;
  bottom: 0;
  width: 7.5rem;
  height: 1rem;
  background-color: #ACF;
  box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
  .info_wrap {
    position: relative;
    float: left;
    width: 1.5rem;
    height: 1rem;
    .info_wrap_poster {
      position: absolute;
      bottom: 50%;
      left: 0.25rem;
      width: 1rem;
      height: 1rem;
      border-radius: 50%;
      overflow: hidden;
      background: url(../assets/musicposter.jpg) no-repeat center center;
      background-size:cover;
      box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
      img {
        width: 100%;
      }
    }
  }
  .play_wrap {
    box-sizing: border-box;
    height: 1rem;
    float: left;
    width: 4.5rem;
    height: 1rem;
    .play_wrap_name {
      display: flex;
      box-sizing: border-box;
      padding: 0 0.3rem;
      font-size: 0.2rem;
      width: 100%;
      height: 0.4rem;
      .name_artist {
        box-sizing: border-box;
        padding: 0 0.2rem;
        float: left;
        line-height: 0.4rem;
        color: white;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
      }
      .name_song {
        box-sizing: border-box;
        padding: 0 0.2rem;
        float: left;
        flex: 1;
        line-height: 0.4rem;
      }
    }
    .play_wrap_player {
      position: relative;
      width: 100%;
      height: 0.5rem;
      audio {
        position: absolute;
        height: 0.5rem;
        outline: none;
        width: 5rem;
      }
    }
  }
  .conduct_wrap {
    width: 1rem;
    height: 1rem;
    
  }
}
</style>