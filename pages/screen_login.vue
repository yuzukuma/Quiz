<template>
  <!-- <section class="section"> -->
      <div class="field">
        <div class="is-mobile" id="padding_u_100">
          <p style="text-align: center; font-size: 30px;">スクリーンログイン</p>
          <div style="text-align: center; font-size: 40px;">
          </div>
        </div>
        <div class="columns is-mobile" id="padding_u_100">
          <div :class="box">
            <input class="input" type="text" placeholder="groupId" v-model="groupId">
            <p v-if='Login[0]' style="color: red;">※そのグループIDは存在しません</p>
          </div>
        </div>
        <div class="columns is-mobile">
          <div :class="box">
            <a class="button is-primary" style="width: 100%;" @click="login()">スクリーンへログイン</a>
          </div>
        </div>
      </div>
  <!-- </section> -->
</template>

<script>
import io from 'socket.io-client'

export default {
  components: {
  },
  data() {
    return {
      groupId: '',
      isLogin: [false],
      Login: [false],
      box: 'column is-10 is-offset-1',
    }
  },
  mounted() {
    // this.isLogin_groupId = this.$route.query.groupId
    this.socket = io()
  },
  methods: {
		login(){
      this.socket.emit('screen', this.groupId)
      sessionStorage.setItem('groupId', this.groupId);

      // ログイン可否の受け取り
      this.socket.on('screenLogin', Login => {
        this.Login.push(Login)
        this.Login.shift()
        if(this.Login[0] == true){
        }
        else{
          this.$router.push({path: '/screen?login=true'})
        }
      })
		}
  },
}
</script>

<style scoped>
#padding_u_100 {
  padding: 100px 0px 0px;
  font-family: 'Kosugi Maru', sans-serif;
}
.rot1 {
	animation: font-anime 0.8s infinite;
}
.rot1 {
	animation: font-anime 1.2s infinite alternate;
}
.rot2 {
  transform: rotate(-5deg);
}
.rot3 {
  transform: rotate(5deg);
}
@keyframes font-anime {
	0% {
    transform: rotate(-10deg);
	}
	100%{
    transform: rotate(10deg);
	}
}
</style>