<template>
  <v-container class="index-page">

    <h1 v-if="countHiddenPage >= 5"><span>D</span>
      means<span>
        Determination</span>
      to become an engineer and to challenge <span>Vue.js!</span></h1>


    <div class="top-img">
      <img src="../assets/haikei.png" />
      <h1 class="index-title"><span class="index-title-first">D</span>TODO</h1>
      <h2 class="index-sub-title mt-5">Gamefy your TODO.</h2>
    </div>


    <v-row class="justify-center mt-10">
      <v-col class="main-introduction" cols="12" sm="10" md="10" lg="10">
        <h1>やらなければならないことを楽しむ</h1>
        <h2 style="text-align:center;">
          <vue-typer style="font-family: dot; text-align:center;" :text="[
        '＊毎日のTODO',
        '＊メンドクサイですよね',
        '＊DTODOを使って',
        'タノシイに変えましょう！',
        ]" erase-style="clear" :type-delay="150" :erase-delay="300" :repeat="Infinity"></vue-typer>
        </h2>
      </v-col>
    </v-row>

    <v-row class="justify-center">
      <v-col cols="12" sm="12" md="8" lg="7">
        <div v-if="user"></div>
        <div class="guest mt-4" v-else>
          <v-hover v-slot:default="{ hover }">
            <v-btn class="guest-btn" @click="guestLogin">
              <v-icon v-text="hover ? 'mdi-heart' : ''"></v-icon>サッソク試してみる
            </v-btn>
          </v-hover>
          <h4>※ゲストはログイン後、毎回チュートリアルが再生されます。</h4>
          <h4>1回だけでいい場合は新規登録をお願いします。</h4>
        </div>
      </v-col>


      <v-col class="wrapper-skull" v-if="user" cols="12" sm="10" md="6" lg="6">
        <v-icon id="skull" :color="color" @click="countSkull()" class="mb-2 skull" :size="size">mdi-skull-outline
        </v-icon>
        <h1 v-if="countHiddenPage >= 15">押しすぎだ。</h1>
        <div class="instead-of-form">
          <vue-typer :text="[
              '＊俺が見えてるってことは',
              '＊ログインしているってことだ。',
              '＊わざわざログインしたまま\nトップ画面に来るなんて',
              '＊物好きだな。',
              '＊ところでアンタ\nこのアプリの「D」の意味は知っているか？',
              '＊フフフ・・・',
              '＊俺様の顔を５回クリックすると',
              '＊わかるかもな。',
              '＊スマホのやつは\n上にスライドしてくれよな。',
              '＊じゃあな。',
            ]" erase-style="clear" :type-delay="140" :erase-delay="400" :repeat="Infinity"></vue-typer>
        </div>
      </v-col>
      <div v-else></div>
    </v-row>

    <v-row justify="center">
      <v-col class="index-button-wrapper" cols="12" sm="12" md="8" lg="6">

        <v-hover v-slot:default="{ hover }">
          <div v-if="user"></div>
          <v-btn v-else class="bottom-btn" @click.stop="dialog = true">
            <v-icon v-text="hover ? 'mdi-heart' : ''"></v-icon>ログイン
          </v-btn>
        </v-hover>
        </v-col>
      <v-col class="index-button-wrapper" cols="12" sm="12" md="8" lg="6">
        <v-hover v-slot:default="{ hover }">
          <div v-if="user"></div>
          <v-btn v-else class="bottom-btn" v-on:click="moveToTop">
            <v-icon v-text="hover ? 'mdi-heart' : ''"></v-icon>新規登録
          </v-btn>
        </v-hover>
      </v-col>
    </v-row>


    <v-row class="introduction mb-0 justify-center">
      <v-col class="sub-introduction-1 mx-2 mb-2" cols="12" sm="9" md="3" lg="3">
        <img src="../assets/mon_259.gif" />
        <h2 class="index-subtitle-1 text-center">タスクポイントを<br>設定しよう！</h2>
        <h3 class="index-explain-1">
          DTODOは日々のやらなければならないこと「TODO」を作成する時に、にタスクポイント（TP）を設定することができます。
          TPはご褒美の解放に使用できます。
        </h3>
      </v-col>

      <v-col class="sub-introduction-2 mx-2 mb-2" cols="12" sm="9" md="3" lg="3">
        <img src="../assets/mon_237.gif" />
        <h2 class="index-subtitle-2 text-center">レベルアップを<br>目指そう！</h2>
        <h3 class="index-explain-2">
          TODOを完了するたびに経験値がたまります。より高いレベルを目指して頑張りましょう。
          レベルが高くなると、何かいいことが…？
        </h3>
      </v-col>

      <v-col class="sub-introduction-3 mx-2 mb-2" cols="12" sm="9" md="3" lg="3">
        <img class="pt-10" src="../assets/mon_278.gif" />
        <h2 class="index-subtitle-3 text-center">自分にごほうびを<br>あげよう！</h2>
        <h3 class="index-explain-3">
          DTODOを利用する際は、まずごほうびを作成しましょう。ご褒美の解放にはTPが必要です。
          たくさんのTODOをこなして、自分にごほうびをあげましょう。
        </h3>
      </v-col>
    </v-row>

    <v-dialog content-class="dialog" v-model="dialog" max-width="60%">
      <v-card>
        <v-card-title class="headline">
          <h3>Login</h3>
        </v-card-title>
        <v-card-text>
          <form>
            <v-text-field v-model="email" :counter="20" label="email" data-vv-name="email" required></v-text-field>
            <v-text-field v-model="password" label="password" data-vv-name="password" required
              :type="show1 ? 'text' : 'password'" :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
              @click:append="show1 = !show1"></v-text-field>
            <v-hover v-slot:default="{ hover }">
              <v-btn class="dialog-btn" @click="login">
                <v-icon v-text="hover ? 'mdi-heart' : ''"></v-icon>START
              </v-btn>
            </v-hover>
            <p v-if="error" class="errors">{{ error }}</p>
          </form>
        </v-card-text>
      </v-card>
    </v-dialog>

    <v-row justify="center">
      <iframe class="how-to-use" src="https://www.youtube.com/embed/HsJrTMvUQOw?rel=0" frameborder="0"
        allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </v-row>

    <v-row justify="center">
      <v-col class="side-monster" sm="4" md="2" lg="2">
        <img src="../assets/mon_199.gif" alt="">
        <p>共に楽しもう。</p>
      </v-col>
      <v-col class="index-button-wrapper" cols="12" sm="10" md="8" lg="8">
        <h1 id="index-signup" class="index-form-title text-center">
          新規登録
        </h1>
        <form>
          <v-text-field v-model="name" :counter="10" label="name" data-vv-name="name" required></v-text-field>
          <v-text-field v-model="email" :counter="30" label="email" data-vv-name="email" required></v-text-field>
          <v-text-field v-model="password" label="password" data-vv-name="password" required
            :type="show1 ? 'text' : 'password'" :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
            @click:append="show1 = !show1"></v-text-field>
          <v-text-field v-model="passwordConfirm" label="passwordConfirm" data-vv-name="passwordConfirm" required
            :type="show2 ? 'text' : 'password'" :append-icon="show2 ? 'mdi-eye' : 'mdi-eye-off'"
            @click:append="show2 = !show2"></v-text-field>

          <p v-if="error" class="errors">{{ error }}</p>
        </form>
        <v-hover v-slot:default="{ hover }">
          <v-btn class="index-button" @click="signup">
            <v-icon v-text="hover ? 'mdi-heart' : ''"></v-icon>
            START
          </v-btn>
        </v-hover>
      </v-col>
      <v-col class="side-monster pt-8" sm="4" md="2" lg="2">
        <img src="../assets/mon_284.gif" alt="">
        <p>待っているわ。</p>
      </v-col>
    </v-row>

  </v-container>
</template>

<script>
  import AddTodo from "@/components/AddTodo";
  import TodoList from "@/components/TodoList";
  import axios from "@/plugins/axios";
  import firebase from "@/plugins/firebase";
  export default {
    data() {
      return {
        email: "",
        name: "",
        level: "",
        point: "",
        experience_point: "",
        password: "",
        passwordConfirm: "",
        show1: false,
        show2: false,
        error: "",
        dialog: false,
        countHiddenPage: 0,
        color: "",
        size: 80,
        showContent: false
      };
    },
    components: {
      AddTodo,
      TodoList
    },
    computed: {
      user() {
        return this.$store.state.currentUser;
      }
    },
    methods: {
      signup() {
        if (this.password !== this.passwordConfirm) {
          this.error = "パスワード確認が一致していません";
          return;
        }
        if (this.name == "") {
          this.error = "名前を入力してください";
          return;
        }
        firebase
          .auth()
          .createUserWithEmailAndPassword(this.email, this.password)
          .then(res => {
            const user = {
              email: res.user.email,
              name: this.name,
              uid: res.user.uid
            };
            this.$store.commit("setLoading", true);
            axios
              .post("/v1/users", {
                user
              })
              .then(res => {
                this.$store.commit("setLoading", false);
                const param = {
                  user: res.data
                };
                this.$store.commit("setUser", param);
                this.$store.commit("setNotice", {
                  status: true,
                  message: "新規登録が完了しました"
                });
                setTimeout(() => {
                  this.$store.commit("setNotice", {});
                }, 2000);
                this.$router.push("/user");
              });
          })
          .catch(error => {
            this.error = (code => {
              switch (code) {
                case "auth/email-already-in-use":
                  return "既にそのメールアドレスは使われています";
                case "auth/wrong-password":
                  return "※パスワードが正しくありません";
                case "auth/weak-password":
                  return "パスワードは最低6文字以上にしてください";
                default:
                  return "メールアドレスとパスワードをご確認ください";
              }
            })(error.code);
          });
      },
      login() {
        firebase
          .auth()
          .signInWithEmailAndPassword(this.email, this.password)
          .then(() => {
            this.$store.commit("setLoading", true);
            this.$store.commit("setNotice", {
              status: true,
              message: "ログインに成功しました"
            });
            setTimeout(() => {
              this.$store.commit("setLoading", false);
            }, 3000);
            setTimeout(() => {
              this.$store.commit("setNotice", {});
            }, 2000);
            this.$router.push("/user");
          })
          .catch(error => {
            console.log(error);
            this.error = (code => {
              switch (code) {
                case "auth/user-not-found":
                  return "メールアドレスが間違っています";
                case "auth/wrong-password":
                  return "※パスワードが正しくありません";
                default:
                  return "※メールアドレスとパスワードをご確認ください";
              }
            })(error.code);
          });
      },
      guestLogin() {
        this.$store.commit("setLoading", true);
        firebase
          .auth()
          .signInWithEmailAndPassword("testtest777@gmail.com", "aaaaaa")
          .then(() => {
            this.$store.commit("setNotice", {
              status: true,
              message: "ログインに成功しました"
            });
            setTimeout(() => {
              this.$store.commit("setLoading", false);
            }, 1500);
            setTimeout(() => {
              this.$store.commit("setNotice", {});
            }, 2000);
            this.$router.push("/user");
          })
          .catch(error => {
            console.log(error);
            this.error = (code => {
              switch (code) {
                case "auth/user-not-found":
                  return "メールアドレスが間違っています";
                case "auth/wrong-password":
                  return "※パスワードが正しくありません";
                default:
                  return "※メールアドレスとパスワードをご確認ください";
              }
            })(error.code);
          });
      },
      moveToTop() {
        const element = document.getElementById("index-signup");
        element.scrollIntoView({
          behavior: "smooth",
          block: "center"
        });
        const title = document.getElementById("index-signup");
        const check = function (name) {
          title.classList.add(name);
        };
        setTimeout(check, 1000, "checked");
      },
      countSkull() {
        this.countHiddenPage++;
        this.size += 10;
        this.color = "green"
      },
      // handleScroll: function (evt, el) {
      //   if (window.scrollY > 50) {
      //     el.setAttribute(
      //       "style",
      //       "opacity: 1; transform: translate3d(0, -10px, 0)",
      //       "transform: translateY(0);"
      //     );
      //   }
      //   console.log(el)
      //   return window.scrollY > 100;
      // }
    },
    created() {
      this.$vuetify.lang = {
        t: () => {}
      };
      //testを実行する際に直接関係ないエラーを回避する為に記述してあります。
    }
  };
</script>

<style lang="scss">
  $main-color: #fc7b03;
  $sub-color: #33dddd;
  $accent-color: #f0353f;

  @mixin explain {
    color: $main-color;
    font-family: "ヒラギノ角ゴシック";
    margin-bottom: 10px;
  }

  @mixin index-bottom-btn {
    background-color: black !important;
    border: 2px solid $main-color;
    color: $main-color !important;
    display: inline-block;
    margin: 15px;
    width: 80%;
    font-weight: bold;
  }

  $pc: 2024px;
  $mid: 1024px;
  $tab: 680px;
  $sp: 480px;

  // レスポンシブデザイン用の指定です。
  @mixin pc {
    @media (max-width: ($pc)) {
      @content;
    }
  }

  @mixin mid {
    @media (max-width: ($mid)) {
      @content;
    }
  }

  @mixin tab {
    @media (max-width: ($tab)) {
      @content;
    }
  }

  @mixin sp {
    @media (max-width: ($sp)) {
      @content;
    }
  }

  @font-face {
    font-family: "dot";
    src: url("../assets/fonts/PixelMplus12-Regular.ttf") format("truetype");
  }

  .index-page {
    * {
      font-family: dot;
      letter-spacing: 5px;
      -webkit-text-stroke: thin;
    }

    .top-img {
      position: relative;
      text-align: center;

      img {
        margin: 0 auto;
        width: 70%;
        height: 70%;
        @include sp {
        width: 100%;
        height: 100%;
      }
      }
    }

    .mon {
      width: 9%;

      @include sp {
        width: 20%;
      }
    }

    .monster {
      text-align: center;
    }

    .index-title {
      text-align: center;
      font-size: 150px;
      font-family: "dot";
      text-shadow: 3px 5px #6b6b6b;
      margin-bottom: 0px;
      color: white !important;
      position: absolute;
      top: 10%;
      left: 20%;
      letter-spacing: 10px;

      @include sp {
        font-size: 60px;
        top: 5%;
        left: 15%;
      }

      @include tab {
        font-size: 60px;
        top: 5%;
        left: 15%;
      }

      .index-title-first {
        color: $main-color;
      }
    }

    .index-sub-title {
      font-size: 40px;
      letter-spacing: 10px;
      @include sp {
        font-size: 30px;
        margin-bottom: -20px;
      }
    }

    .main-introduction {
      margin: 2em 0;
      position: relative;
      padding: 0.5em 1.5em;
      border-top: solid 2px white;
      border-bottom: solid 2px white;
      font-size: 20px;
      font-family: "dot";
      letter-spacing: 10px;

      h1 {
        color: $main-color;
        font-family: 'ヒラギノ角ゴシック';
      }
    }

    .main-introduction:before,
    .main-introduction:after {
      content: '';
      position: absolute;
      top: -10px;
      width: 2px;
      height: -webkit-calc(100% + 20px);
      height: calc(100% + 20px);
      background-color: white;
    }

    .main-introduction:before {
      left: 10px;
    }

    .main-introduction:after {
      right: 10px;
    }

    .index-form-title {
      @include explain;
    }

    .index-subtitle-1 {
      color: rgb(255, 255, 255);
      background-color: rgb(62, 172, 35, 0.4);
    }

    .index-explain-1 {
      color: rgb(255, 255, 255);
      text-align: center;
      margin: 30px 0;
      background-color: rgb(62, 172, 35, 0.4);

    }

    .index-subtitle-2 {
      color: rgb(255, 255, 255);
      background-color: rgb(65, 46, 46, 0.4);
    }

    .index-explain-2 {
      color: rgb(255, 255, 255);
      text-align: center;
      margin: 30px 0;
      background-color: rgb(65, 46, 46, 0.4);

    }

    .index-subtitle-3 {
      color: rgb(255, 255, 255);
      background-color: rgba(46, 75, 204, 0.4);
    }

    .index-explain-3 {
      color: rgb(255, 255, 255);
      text-align: center;
      margin: 30px 0;
      background-color: rgba(46, 75, 204, 0.4);

    }

    .index-button-wrapper {
      .index-button {
        background-color: black !important;
        border: 2px solid $main-color;
        color: $main-color;
        width: 100%;

        &:hover {
          border: 2px solid yellow;
          color: yellow;
        }
      }
    }

    .skull {
      padding-left: 40%;
    }

    .instead-of-form {
      border: 2px white solid;
      padding: 20px 0;
      text-align: center;
    }

    .introduction {
      margin: 100px auto 50px;

      img {
        margin: 0 auto 30px;
        display: block;
      }

      .sub-introduction-1 {
        text-align: center;
        background-image: url("../assets/kusamura.jpg");
        border: 3px solid rgb(255, 255, 255);
      }

      .sub-introduction-2 {
        text-align: center;
        background-image: url("../assets/kojyou.jpg");
        border: 3px solid rgb(255, 255, 255);
      }

      .sub-introduction-3 {
        text-align: center;
        background-image: url("../assets/yoru.jpg");
        border: 3px solid rgb(255, 255, 255);
      }
    }

    h1 {
      text-align: center;
      margin: 30px 0;
      font-family: "dot" !important;
      letter-spacing: 10px;
    }

    .mdi-heart {
      color: red !important;
    }

    .checked {
      color: yellow;
    }

    .index-button-wrapper {
      text-align: center;

      .bottom-btn {
        @include index-bottom-btn;

        &:hover {
          border: 2px solid yellow;
          color: yellow;
        }
      }
    }

    .dialog {
      .bottom-btn {
        @include index-bottom-btn;

        &:hover {
          border: 2px solid yellow;
          color: yellow;
        }
      }
    }

    .errors {
      color: $accent-color;
    }
    .side-monster {
      text-align: center;
      p {
        border: 2px solid white;
        font-size: 15px;
      }
      margin-top: 300px;
      // @include pc {
      //   margin-top: 300px
      // }
      @include mid {
        margin-top: 0px
      }
      @include sp {
        margin-top: 0px
      }
    }
  }

  .dialog-btn {
    background-color: rgb(29, 29, 29) !important;
    border: 2px solid $main-color;
    color: $main-color !important;
    display: inline-block;
    margin: 15px;
    width: 45%;
    font-weight: bold;

    .mdi-heart {
      color: red !important;
    }

    &:hover {
      border: 2px solid yellow;
      color: yellow;
    }
  }

  .headline {
    color: $sub-color;
  }

  .guest {
    text-align: center;

    .guest-btn {
      @include index-bottom-btn;

      &:hover {
        border: 2px solid yellow;
        color: yellow !important;
      }

      @include sp {
        font-size: 20px;
        width: 80% !important;
        height: 70px !important;
      }
    }
  }

  .vue-typer {
    font-family: monospace;
    font-size: 20px;
  }

  .vue-typer .custom.char {
    color: #d4d4bd;
    // background-color: #1e1e1e;
  }

  // .vue-typer .custom.char.selected {
  //   background-color: #264f78;
  // }

  .vue-typer .custom.caret {
    width: 10px;
    background-color: #3f51b5;
  }

  .green {
    color: green;
  }

  .wrapper-skull {
    margin: 0 auto;

    #skull {
      margin: 0 auto;
    }
  }

  .how-to-use {
    width: 75%;
    height: 500px;

    @include tab {
      height: 300px
    }

    @include sp {
      height: 200px;
    }
  }
</style>
