<template>
  <!-- 요기요 컬러 #ff7799 하지만 tailwind는 따로 세팅 해줘야 합니다. 최대한 비슷한 색깔로 했습니다. -->
  <div class="bg-[#ff2972]">
  <div class="w-full h-[84px] mx-auto max-w-[1400px] flex items-center justify-between px-4">
      <div class="flex items-center space-x-10">
          <div class="grid-cols-4 col-span-4">
            <router-link to="/" v-show="userRole !== 'ROLE_OWNER'">
                <!-- <h1 class="text-slate-50 font-sans text-2xl py-4 ml-2"><a herf="/">포장의민족</a></h1> -->
                <img src="@/assets/logo.png" class="h-[58px]">
            </router-link>
            <router-link to="/my-stores" v-show="userRole === 'ROLE_OWNER'">
                <!-- <h1 class="text-slate-50 font-sans text-2xl py-4 ml-2"><a herf="/">포장의민족</a></h1> -->
                <img src="@/assets/logo.png" class="h-[58px]">
            </router-link>
          </div>
      </div>
      <div class="flex items-center space-x-4">
        <router-link v-bind:to="`/login`"><button v-if="!isLogin" class="hover:bg-rose-400 rounded-md text-slate-50 border-2 border-slate-50 p-2 px-8" >로그인</button></router-link>
        <button v-if="isLogin" @click="Rolecheck" class="rounded-md hover:bg-rose-400 text-slate-50 border-2 border-slate-50 p-2 px-8">마이페이지</button>
        <button v-if="isLogin" @click="doLogout" class="rounded-md hover:bg-rose-400 text-slate-50 border-2 border-slate-50 p-2 px-8" >로그아웃</button>
        <a v-if="isLogin && userRole === 'ROLE_USER'" href="/cart" class="rounded-md hover:bg-orange-400 text-slate-50 bg-orange-500 p-2 px-8">주문표({{ getTotalQuantity }})</a>
      </div>
  </div>
</div>



</template>

<script>
import {mapGetters} from 'vuex';
export default {
    computed: {
        ...mapGetters(['getTotalQuantity'])
    },
    data(){
        return {
            isLogin: false, 
            userRole: null
        }
    },
    created() {
        if(localStorage.getItem("token")){
            this.isLogin = true;
            this.userRole = localStorage.getItem("role");
        }
    },
    methods: {
        doLogout(){
            localStorage.clear();
            window.location.href = "/";
        },
        Rolecheck(){
            if(this.userRole == "ROLE_USER" || this.userRole == "ROLE_ADMIN"){
                this.$router.push({name : 'MyInfo'});
            } else{
                this.$router.push({name : 'MyInfoOwner'});
            }
        }
    }
}
</script>

<style lang="scss" scoped>

</style>