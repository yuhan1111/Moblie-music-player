<template>
  <div class="top_container">
    <div class="top_container_left el-icon-user"></div>
    <div class="top_container_middle" >
      <input
        type="text"
        autocomplete="off"
        placeholder="搜索"
        class="search el-icon-search"
        v-model="inputValue"
        @keyup.enter="toResult"
      />
    </div>
    <div class="top_container_right el-icon-search" @click="toResult"></div>
  </div>
</template>

<script>
export default {
  name: "top",
  data() {
    return {
      inputValue:'',
    };
  },
  methods: {
    toResult() {
      if (this.inputValue == "") {
        this.$message.warning("请输入要搜索内容");
      } else {
        this.$router.push(`/result?q=${this.inputValue}`);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.top_container {
  width: 100%;
  height: 1rem;
  margin-bottom: 0.1rem;
  background-color: #aaccff;
  box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
  .top_container_left {
    float: left;
    width: 15%;
    line-height: 1rem;
    text-align: center;
    font-size: 0.5rem;
    font-weight: 700;
  }

  .top_container_middle {
    float: left;
    width: 70%;
    height: 1rem;
    .search {
      float: left;
      box-sizing: border-box;
      width: 100%;
      height: 0.7rem;
      border: none;
      outline: none;
      margin-top: 0.15rem;
      background-color: white;
      border-radius: 0.4rem;
      width: 100%;
      padding-left: 0.4rem;
      line-height: 0.7rem;
      font-size: 0.3rem;
      box-shadow: 0 0.1rem 0.15rem -0.05rem rgba(0, 0, 0, 0.5);
    }
  }
  .top_container_right {
    float: left;
    width: 15%;
    color: black;
    font-weight: 700;
    text-align: center;
    line-height: 1rem;
    font-size: 0.5rem;
  }
}
</style>