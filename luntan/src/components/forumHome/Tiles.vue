<template>
  <div class="tile is-ancestor">
    <div class="tile is-parent">
      <article class="tile is-child box">
        <p class="subtitle">热门板块</p>

        <b-carousel>
          <b-carousel-item v-for="(content, index) in contents" :key="index">
            <section>
              <div class="hero-body has-text-centered">
                <div class="card">
                  <header class="card-header">
                    <p class="card-header-title">
                      板块名:{{ content.typeName }}
                    </p>
                    <a
                      href="#"
                      class="card-header-icon"
                      aria-label="more options"
                    >
                      <span class="icon">
                        <i class="fas fa-angle-down" aria-hidden="true"></i>
                      </span>
                    </a>
                  </header>
                  <div class="card-content">
                    <div class="content">
                      板块简介:{{ content.typeDesc }}
                      <br />
                    </div>
                  </div>
                  <footer class="card-footer">
                    <a href="#" class="card-footer-item">关注</a>
                    <a href="#" class="card-footer-item">进入板块</a>
                  </footer>
                </div>
              </div>
            </section>
          </b-carousel-item>
        </b-carousel>
      </article>
    </div>
    <div class="tile is-parent is-8">
      <article class="tile is-child box">
        <p align="right">
          <b-button type="button is-info" outlined @click="allart"
            >全部帖子</b-button
          >
        </p>
        <p class="subtitle">热门帖子</p>

        <div class="box" v-for="(item, i) in $store.state.info" :key="i">
          <article class="media">
            <figure class="media-left">
              <p class="image is-64x64">
                <img
                  :src="require(`@/assets/${item.user.userImg}`)"
                  class="size"
                />
                <!-- <img src="../../assets/user1.jpg" alt /> -->
              </p>
            </figure>
            <div class="media-content">
              <div class="content">
                <p>
                  <strong>{{ item.user.userName }}</strong>
                  <br />
                  {{ item.article.artTitle }}
                </p>
              </div>
              <nav class="level is-mobile">
                <div class="level-left">
                  <a class="level-item">
                    <span class="icon is-small">
                      <i class="fab fa-hotjar"></i>
                    </span>
                    {{ item.article.artHotNum }}
                  </a>

                  <a class="level-item">
                    <span class="icon is-small">
                      <i class="fas fa-comment-dots"></i>
                    </span>
                    {{ item.article.artComNum }}
                  </a>

                  <a class="level-item">
                    <span class="icon is-small">
                      <i class="fas fa-heart"></i>
                    </span>
                    {{ item.article.artLikeNum }}
                  </a>
                </div>
              </nav>
            </div>
            <div class="media-right">
              <a class="navbar-item" slot="trigger" role="button">
                <b-button type="is-info" outlined @click="details(i)"
                  >查看详情</b-button
                >
              </a>
            </div>
          </article>
        </div>
        <Pageination />
      </article>
    </div>
  </div>
</template>

<script>
import { getHotArticleType } from "@/api";
// import { getPageMain } from "@/api";
import Pageination from "./Pageination.vue";
export default {
  data() {
    return {
      info: [
        {
          article: {
            artId: 0,
            artUserId: 1,
            artTitle: "",
            artTypeId: 0,
            artContent: "",
            artCommentId: 0,
            artCreTime: "",
            artView: "",
            artComNum: 0,
            artHotNum: 0,
            artLikeNum: 0
          },
          user: {
            userId: 0,
            userPassword: 0,
            userName: "",
            userEmail: "",
            userSex: "",
            userPhone: "",
            userStatus: 0,
            userTime: "",
            userShow: "",
            userBlog: "",
            userImg: "",
            userFans: 0,
            userConcern: 0
          }
        }
      ],
      contents: {
        typeId: 0,
        typeName: "",
        typeCreateTime: "",
        typeDesc: "",
        articleNum: 0
      }
    };
  },
  mounted() {
    getHotArticleType()
      .then(res => {
        const { data } = res.data;
        this.contents = data.content;
		console.log(this.contents)
      })
      .catch(() => {});

    this.$store.dispatch("getpagemain");
    //bug，应该用action异步处理，再提交commit状态，明日更新（已解决）
    // getPageMain()
    //   .then(res => {
    //     const { data } = res;
    //     this.info = data.content;
    //   })
    //   .catch(() => {});
  },
  methods: {
    allart() {
      this.$router.push("/allarticlehome");
    },
    details(i) {
      const detaildata = this.$store.state.info[i];
      console.log(detaildata);
      this.$router.push({
        path: "/details",
        query: {
          detaildata: JSON.stringify(detaildata)
        }
      });
    }
  },
  components: {
    Pageination
  }
};
</script>

<style scoped>
.subtitle {
  text-align: left;
}
.button {
  text-align: center;
  margin: 0 auto;
}
.size {
  width: 64px;
  height: 64px;
}
</style>
