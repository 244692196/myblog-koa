<template>
  <div class="details">
    <h2 class="title">{{detail.title}}</h2>
    <!-- <div class="content" v-html="detail.text"></div> -->
    <div class="ql-snow">
      <div class="ql-editor" v-html="detail.text"></div>
    </div>
    <el-button
      v-if="loginUser == detail.author"
      type="primary"
      icon="el-icon-edit"
      circle
      class="btn-art"
      @click="edit"
    ></el-button>
    <div class="toTop-de" :style="{'height': (scrollV==true? '40px':'0')}" @click="toTop">
      <svg class="icon" aria-hidden="true">
        <use xlink:href="#icon-up-arrow" />
      </svg>
    </div>
  </div>
</template>

<script>
import Nav from "@/components/Nav";
import { mapState } from "vuex";
export default {
  components: {
    Nav
  },
  data() {
    return {
      detail: "",
      scrollV: false,
      mun: 0,
      scrollTop: ""
    };
  },
  computed: {
    ...mapState(["loginUser"])
  },
  methods: {
    edit() {
      this.$router.push({ path: `/edit/${this.$route.params.id}` });
    },
    getScrollPosition() {
      //监听滚轮事件触发
      // 滚动条距顶部距离
      let that = this;
      let top = document.documentElement.scrollTop || document.body.scrollTop;
      that.scrollTop = top;
      top >= 400 && (this.scrollV = true);
      top < 400 && (this.scrollV = false);
    },
    toTop() {
      let that = this;
      let timer = setInterval(() => {
        that.mun++;
        window.scrollBy(0, -that.mun);
        if (that.scrollTop == 0) {
          that.mun = 0;
          clearInterval(timer);
        }
      }, 100 / 12);
    }
  },
  created() {
    this.$axios
      .get(`http://ddiyy.cn:4000/article/${this.$route.params.id}`)
      .then(res => {
        this.detail = res.data;
      });
    this.screenH = window.screen.height;
  },
  mounted() {
    document.addEventListener("scroll", this.getScrollPosition);
  }
};
</script>

<style scoped>
.details {
  width: 65%;
  min-height: 600px;
  padding: 20px 60px 0;
  background-color: rgba(164, 177, 209, 0.8);
  margin: auto;
  box-shadow: 0px 10px 15px rgba(164, 177, 209);
}
.details .title {
  width: 100%;
  height: 40px;
  font: 24px/2 "微软雅黑";
  font-weight: bold;
  text-align: center;
}
.btn-art {
  position: fixed;
  bottom: 100px;
  right: 100px;
  width: 40px;
}
.content {
  font: 16px/1.5 "微软雅黑";
}
.content img {
  height: 300px;
}
>>> .ql-snow .ql-editor img {
  width: 100%;
}
.toTop-de {
  cursor: pointer;
  position: fixed;
  bottom: 90px;
  right: 218px;
  width: 40px;
  font: 35px/40px "微软雅黑";
  height: 0;
  transition: 0.5s;
  overflow: hidden;
}
@media screen and (max-width: 800px) {
  .toTop-de {
    right: 8%;
  }
  .btn-art {
    right: 8%;
    bottom: 80%;
  }
  .details {
    width: 90%;
    padding: 20px 10px 0;
  }
}
@media screen and (max-width: 400px) {
  .details {
    width: 360px;
  }
}
</style>