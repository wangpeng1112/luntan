<template>
  <div class="tile is-ancestor">
    <div class="tile is-vertical is-8">
      <progress class="progress is-whrite is-small" value="100%" max="30"
        >30%</progress
      >
      <div class="tile">
        <div class="tile is-parent is-vertical">
          <article class="tile is-child box">
            <div class="fuck">
              <p class="title">最新发布的帖子</p>
              <div class="box" v-for="(item, i) in info" :key="i">
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

                        <a class="level-item" :class="{ liked: item.liked }" @click="toggleLike(i) ">
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
                      <b-button type="is-info" @click="detail(i)"
                        >查看详情</b-button
                      >
                    </a>
                  </div>
                </article>
              </div>
            </div>
            <hr />
          </article>
        </div>

        <div class="tile is-parent">
          <article class="tile is-child box">
            <p class="title">放学校风景图</p>
            <b-carousel>
              <b-carousel-item v-for="(carousel, i) in 6" :key="i">
                <section :class="`hero is-medium `">
                  <div class="hero-body has-text-centered">
                    <span class="image">
                      <img :src="getImgUrl(i)" />
                    </span>
                  </div>
                </section>
              </b-carousel-item>
            </b-carousel>
          </article>
        </div>
      </div>
      <div class="tile is-parent">
        <article class="tile is-child box">
          <p class="title">友情链接</p>
          <hr />
          <div class="content">
            <a href="https://stackoverflow.com/">StackOverflow</a>&nbsp;
            <a href="https://github.com/">GitHub</a>&nbsp;
            <a href="https://www.youtube.com/">YouTube</a>&nbsp;
            <a href="https://tuna.moe/">清华大学TUNA协会</a>
          </div>
        </article>
      </div>
    </div>
    <div class="tile is-parent">
      <article class="tile is-child box">
        <div class="content">
          <p class="title">用户人气榜</p>

          <div class="card" v-for="(user, k) in users" :key="k">
            <div class="card-content">
              <div class="media">
                <div class="media-left">
                  <figure class="image is-48x48">
                    <img
                      :src="require(`@/assets/${user.userImg}`)"
                      class="size"
                    />
                    <!-- <img src="../../assets/user1.jpg" alt /> -->
                  </figure>
                </div>
                <div class="media-content">
                  <p class="title is-4" v-if="user.userSex === '女'">
                    大名：{{ user.userName }}
                    <i class="fas fa-female"></i>
                  </p>

                  <p class="title is-4" v-else>
                    大名：{{ user.userName }}
                    <i class="fas fa-male"></i>
                  </p>
                </div>
              </div>

              <div class="content">
                <p>格言：{{ user.userShow }}</p>
                <i class="far fa-hand-point-right">个人主页</i>
                <br />粉丝数：
                <i class="fab fa-gratipay">{{ user.userFans }}</i>
                <br />
              </div>
            </div>
          </div>
        </div>
      </article>
    </div>
  </div>
</template>

<script>
import { getnew } from "@/api";
import { gethotuser } from "@/api";
import { Like } from "@/api";


export default {
  data() {
    return {
		
      users: [
        {
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
      ],
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
	   liked: false // 添加liked属性
    };
  },
  methods: {
	  toggleLike(i){
		  if(this.$store.state.isLogin){
	  	  const item = this.info[i];
		  // item.user.userId 这个是帖子发布者的id==artUserId,$store.state.user.userId
	  	  Like(this.$store.state.user.userId,item.article.artId,item.article.artUserId)
	  	  .then(res =>{
	  		 if(res.data.code==0){
	  			 alert(res.data.msg)
				 console.log(res.data.data)
				 item.article.artLikeNum=res.data.data
	  			 item.liked = !item.liked;
	  		 }
	  	  });
		  }else{
			  alert("请登录!!")
		  }
	  },
    getImgUrl(value) {
      return `https://picsum.photos/id/43${value}/576/400`;
    },
    detail(i) {
      const detaildata = this.info[i];
      this.$router.push({
        path: "/details",
        query: {
          detaildata: JSON.stringify(detaildata)
        }
      });
    }
  },
  mounted() {
    getnew().then(res => {
	  
	  console.log(res.data.data)
	  console.log(res.data.data.content)
      this.info = res.data.data.content;
	  
	  console.log(this.info)
    });
    gethotuser().then(res => {
      const { data } = res.data;

      this.users = data.content;
    });
  },
  components: {}
};
</script>

<style scoped>
.hero.is-medium .hero-body {
  padding-top: 0rem;
  padding-bottom: 3rem;
}
.fuck {
  height: 300px;

  overflow-y: auto;
}

</style>
