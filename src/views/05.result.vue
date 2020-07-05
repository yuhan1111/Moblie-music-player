<template>
  <div class="result-container">
    <div class="header_wrap">
      <div class="title_wrap">{{$route.query.q}}</div>
      <div class="subtitle_wrap">搜索到{{songCount}}个结果</div>
    </div>
    <!-- 列表 -->
    <div class="body_wrap">
      <el-tabs v-model="activeName" class="body_inner_wrap">
        <el-tab-pane label="单曲" name="songs" class="songs_wrap">
          <div class="el-table">
            <thead class="songs_title_items">
              <th class="item"></th>
              <th class="item">歌曲名</th>
              <th class="item">歌手</th>
              <th class="item">专辑</th>
            </thead>
            <tbody class="songs_content_items">
              <tr
                class="el-table__row item"
                v-for="(item,index) in songlist"
                :key="index"
                @click="playMusic(item.id,item.name,item.artists[0].name,item.picUrl)"
              >
                <td class="item_index_wrap">{{index+1}}</td>
                <td class="item_title_wrap">
                  <div class="main">{{item.name}}</div>
                  <div v-if="item.alias.length!=0" class="sub">{{item.alias[0].slice(0,8)}}</div>
                </td>
                <td class="item_artist_wrap">{{item.artists[0].name}}</td>
                <td class="item_album_wrap">{{item.album.name}}</td>
              </tr>
            </tbody>
          </div>
        </el-tab-pane>
        <el-tab-pane label="歌单" name="lists" class="lists_wrap">
          <div class="lists_items">
            <div
              class="item"
              v-for="(item, index) in playlists"
              :key="index"
              @click="toPlayList(item.id)"
            >
              <div class="item_img_wrap">
                <img :src="item.coverImgUrl" alt />
              </div>
              <div class="el-icon-headse item_playcount_wrap">{{item.playCount}}</div>
              <div class="item_title_wrap">{{item.name}}</div>
            </div>
          </div>
        </el-tab-pane>
        <el-tab-pane label="MV" name="mv" class="MV_wrap">
          <div class="MV_items">
            <div
              class="item"
              v-for="(item, index) in mvlists"
              :key="index"
              @click="toMV(item.id,item.name,item.artistName)"
            >
              <div class="item_img_wrap">
                <img :src="item.cover" alt />
              </div>
              <div class="item_duration_wrap">{{item.duration}}</div>
              <div class="item_playcount_wrap el-icon-view">{{item.playCount}}</div>
              <div class="item_title_wrap">{{item.name}}</div>
            </div>
          </div>
        </el-tab-pane>
      </el-tabs>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "result",
  data() {
    return {
      activeName: "songs",
      songlist: [],
      playlists: [],
      mvlists: [],
      songCount: "",
      keyword: this.$route.query.q
    };
  },
  watch: {
    activeName() {
      let type = 1;
      let limit = 10;
      switch (this.activeName) {
        case "songs":
          type = 1;
          limit = 30;
          break;
        case "lists":
          type = 1000;
          limit = 18;
          break;
        case "mv":
          type = 1004;
          limit = 12;
          break;
        default:
          break;
      }
      axios({
        url: "https://autumnfish.cn/search",
        method: "get",
        params: {
          keywords: this.$route.query.q,
          type: type,
          limit
        }
      }).then(res => {
        // console.log(res);
        if (type == 1) {
          // console.log(res);
          this.songlist = res.data.result.songs;
          this.songCount = res.data.result.songCount;
          for (let i = 0; i < this.songlist.length; i++) {
            let duration = this.songlist[i].duration;
            let min = parseInt(duration / 1000 / 60);
            min = min < 10 ? "0" + min : min;
            let sec = parseInt((duration / 1000) % 60);
            sec = sec < 10 ? "0" + sec : sec;
            // console.log(min +':'+ sec);

            this.songlist[i].duration = `${min}:${sec}`;
          }
        } else if (type == 1000) {
          // console.log(res);
          this.playlists = res.data.result.playlists;
          this.songCount = res.data.result.playlistCount;
          for (let i = 0; i < this.playlists.length; i++) {
            this.playlists[i].playCount =
              this.playlists[i].playCount > 10000
                ? parseInt(this.playlists[i].playCount / 10000) + "万"
                : this.playlists[i].playCount;
          }
        } else {
          // console.log(res);
          this.mvlists = res.data.result.mvs;
          this.songCount = res.data.result.mvCount;
          for (let i = 0; i < this.mvlists.length; i++) {
            let duration = this.mvlists[i].duration;
            let min = parseInt(duration / 1000 / 60);
            min = min < 10 ? "0" + min : min;
            let sec = parseInt((duration / 1000) % 60);
            sec = sec < 10 ? "0" + sec : sec;
            // console.log(min +':'+ sec);

            this.mvlists[i].duration = `${min}:${sec}`;
          }
          for (let i = 0; i < this.mvlists.length; i++) {
            this.mvlists[i].playCount =
              this.mvlists[i].playCount > 10000
                ? parseInt(this.mvlists[i].playCount / 10000) + "万"
                : this.mvlists[i].playCount;
          }
        }
      });
    },
    $route: function() {
      let type = 1;
      let limit = 10;
      switch (this.activeName) {
        case "songs":
          type = 1;
          limit = 20;
          break;
        case "lists":
          type = 1000;
          limit = 12;
          break;
        case "mv":
          type = 1004;
          limit = 12;
          break;
        default:
          break;
      }
      axios({
        url: "https://autumnfish.cn/search",
        method: "get",
        params: {
          keywords: this.$route.query.q,
          type: type,
          limit
        }
      }).then(res => {
        // console.log(res);
        if (type == 1) {
          // console.log(res);
          this.songlist = res.data.result.songs;
          this.songCount = res.data.result.songCount;
          for (let i = 0; i < this.songlist.length; i++) {
            let duration = this.songlist[i].duration;
            let min = parseInt(duration / 1000 / 60);
            min = min < 10 ? "0" + min : min;
            let sec = parseInt((duration / 1000) % 60);
            sec = sec < 10 ? "0" + sec : sec;
            // console.log(min +':'+ sec);

            this.songlist[i].duration = `${min}:${sec}`;
          }
        } else if (type == 1000) {
          // console.log(res);
          this.playlists = res.data.result.playlists;
          this.songCount = res.data.result.playlistCount;
          for (let i = 0; i < this.playlists.length; i++) {
            this.playlists[i].playCount =
              this.playlists[i].playCount > 10000
                ? parseInt(this.playlists[i].playCount / 10000) + "万"
                : this.playlists[i].playCount;
          }
        } else {
          // console.log(res);
          this.mvlists = res.data.result.mvs;
          this.songCount = res.data.result.mvCount;
          for (let i = 0; i < this.mvlists.length; i++) {
            let duration = this.mvlists[i].duration;
            let min = parseInt(duration / 1000 / 60);
            min = min < 10 ? "0" + min : min;
            let sec = parseInt((duration / 1000) % 60);
            sec = sec < 10 ? "0" + sec : sec;
            // console.log(min +':'+ sec);

            this.mvlists[i].duration = `${min}:${sec}`;
          }
          for (let i = 0; i < this.mvlists.length; i++) {
            this.mvlists[i].playCount =
              this.mvlists[i].playCount > 10000
                ? parseInt(this.mvlists[i].playCount / 10000) + "万"
                : this.mvlists[i].playCount;
          }
        }
      });
    }
  },
  methods: {
    playMusic(id, name, rname, purl) {
      // console.log(id);
      this.$parent.musicName = name;
      this.$parent.rName = rname;
      this.$parent.picUrl = purl;
      axios({
        url: "https://autumnfish.cn/song/url",
        method: "get",
        params: {
          id: id
        }
      }).then(res => {
        let url = res.data.data[0].url;
        // console.log(this.$parent.musicUrl);
        this.$parent.musicUrl = url;
      });
    },
    getList() {
      axios({
        url: "https://autumnfish.cn/search",
        method: "get",
        params: {
          keywords: this.$route.query.q,
          type: "1",
          limit: 20
        }
      }).then(res => {
        // console.log(res);
        this.songlist = res.data.result.songs;
        this.songCount = res.data.result.songCount;
        for (let i = 0; i < this.songlist.length; i++) {
          let duration = this.songlist[i].duration;
          let min = parseInt(duration / 1000 / 60);
          min = min < 10 ? "0" + min : min;
          let sec = parseInt((duration / 1000) % 60);
          sec = sec < 10 ? "0" + sec : sec;
          // console.log(min +':'+ sec);

          this.songlist[i].duration = `${min}:${sec}`;
        }
      });
    },

    toPlayList(id) {
      this.$router.push(`/playlist?q=${id}`);
    },
    toMV(id, name, rname) {
      this.$router.push(`/mv?q=${id}&n=${name}&r=${rname}`);
    }
  },
  created() {
    // console.log(this.$route);
    // console.log(this.$route.query.q);
    this.getList();
  }
};
</script>

<style lang="scss" scoped>
.result-container {
  display: none;
  position: absolute;
  padding: 0.3rem;
  box-sizing: border-box;
  left: -1.1rem;
  top: 0;
  width: 7.5rem;
  background-color: rgba(0, 0, 0, 0.7);
  .header_wrap {
    .title_wrap {
      width: 100%;
      font-size: 0.6rem;
      color: white;
    }
    .subtitle_wrap {
      width: 100%;
      font-size: 0.2rem;
      color: #aaccff;
    }
  }
  .body_wrap {
    .body_inner_wrap {
      width: 100%;
      .songs_wrap {
        .el-table {
          background-color: transparent;
          .songs_title_items {
            .item {
              background-color: transparent;
              color: yellowgreen;
            }
          }
          .songs_content_items {
            background-color: transparent;
            .item {
              color: white;
              background-color: transparent;
              td {
                box-sizing: border-box;
                border: none;
                padding-right: 0.2rem;
                font-size: 0.25rem;
              }
              .item_index_wrap {
                text-align: center;
              }
              .item_title_wrap {
                .main {
                  color: violet;
                }
                .sub {
                  color: yellowgreen;
                  white-space: nowrap;
                  overflow: hidden;
                  text-overflow: ellipsis;
                  font-size: 0.1rem;
                }
              }
            }
            .item:nth-child(2n) {
              background-color: rgba(0, 0, 0, 0.1);
            }
          }
        }
      }
      .lists_wrap {
        width: 100%;
        .lists_items {
          margin: 0.5rem auto;
          width: 6rem;
          .item {
            float: left;
            width: 2rem;
            height: 2rem;
            color: white;
            font-size: 0.2rem;
            position: relative;
            .item_img_wrap {
              width: 100%;
              img {
                width: 2rem;
                height: 2rem;
              }
            }

            .item_playcount_wrap {
              position: absolute;
              right: 0.1rem;
              bottom: 0.2rem;
            }

            .item_title_wrap {
              position: absolute;
              bottom: 0.1rem;
              width: 1.9rem;
              height: 1.9rem;
              font-size: 0.1rem;

              border: 0.1rem solid white;
            }
          }
        }
      }

      .MV_wrap {
        width: 100%;
        .MV_items {
          width: 100%;
          margin-bottom: 1rem;

          .item {
            position: relative;
            font-size: 0.2rem;
            color: white;
            width: 100%;
            height: 2.27rem;
            margin-bottom: 0.1rem;
            background-color: rgba(0, 0, 0, 0.5);
            .item_img_wrap {
              width: 4rem;
              height: 2.27rem;
              float: left;
              overflow: hidden;
              img {
                width: 100%;
                height: 100%;
              }
            }

            .item_duration_wrap {
              position: absolute;
              left: 0;
              bottom: 0;
            }

            .item_playcount_wrap {
              position: absolute;
              left: 0;
              top: 0;
            }

            .item_title_wrap {
              float: left;
              width: 2.9rem;
              box-sizing: border-box;
              padding: 0.4rem;
              font-size: 0.3rem;
              overflow: hidden;
            }
          }
        }
      }
    }
  }
}
</style>