<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button v-on:click="connectWallet">连接Fluent钱包，获取信息</button>
    <h2>用户钱包地址:{{ address }} </h2>
    <h2>用户CFX余额:{{ userCFXAmount }}</h2>

    <button v-on:click="sendContract">与合约交互</button>

    <p>
      代办事项:{{todo_list}}
    </p>
  </div>
</template>

<script>
// import { getNetWork } from "../util/base.js"

export default {

  props: {
    msg: String
  },
  data() {
    return {
      name: 'HelloWorld',
      address: '0000000000000000000',
      userCFXAmount: -1,
      todo_list:''
    }
  },
  computed: {
    // a computed getter
    getNetWorkComputed() {
      // `this` points to the component instance
      return "conflux"
    }
  },
  methods: {
    async sendContract() {
     const rst = await window.ContractTodo.tasks(2)
     this.todo_list = rst[1];
    },
    async connectWallet() {

      // 获取用户地址
      const userAddress = await window.confluxJS.provider.request({ method: "cfx_requestAccounts" }).then(rst => { return rst[0] }).catch(e => { console.log(e) })
      console.log(userAddress)
      this.address = userAddress

      let params = [
        this.address,
        "latest_state"
      ]
      const balance = await window.confluxJS.provider.request({ method: "cfx_getBalance", params })

      this.userCFXAmount = Number(balance) / Math.pow(10, 18)
    }


  },

}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
