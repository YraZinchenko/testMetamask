<template>
  <div>
    <VueMetamask 
      userMessage="msg" 
      @onComplete="onComplete"
    >
    </VueMetamask>
    <div v-if="isEntrance">
      <div>
        adress: {{ adress }}
      </div>
      <div>
        Инпут для сообщения подписи
      </div>
      <input type="text" v-model="signature">
      <div>
        <button @click="signatureMsg">
          подписать сообщения
        </button>
      </div>
      <div v-if="signatureArray.length">
        подписаные сообщения:
      </div>
      <template v-if="signatureArray.length">
        <div v-for="item in signatureArray" :key="item" >
          {{ item }}
        </div>
      </template>
    </div>
    <div v-if="!isEntrance">
      <div>
        Зарегестрируйтесь или войдите в аккаунт.
      </div>
      <div>
        <button @click="entrance">
          вход
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import VueMetamask from 'vue-metamask';
export default {
  components: {
    VueMetamask
  },
  name: 'HelloWorld',
  data(){
    return {
      msg: "This is demo net work",
      signature: '',
      signatureArray: [],
      isEntrance: false,
      adress: null
    }
  },
  methods:{
    onComplete(data){
      // console.log(data);
    },
    signatureMsg() {
      if(this.signature.length) {
        var result = web3.personal.sign(web3.fromUtf8(this.signature), web3.eth.coinbase, console.log);
      }
    },
    async entrance() {
      await ethereum.enable();
      if(ethereum.selectedAddress) {
        this.isEntrance = true;
      }
    }
  },
  created() {
    if(ethereum.selectedAddress) {
      this.isEntrance = true;
      this.adress = ethereum.selectedAddress;
      ethereum.on('accountsChanged', function (accounts) {
        // вотчер на изменение адреса аккаунта
        console.log(accounts);
      })
      window.ethereum.on('networkChanged', function (netId) {
        // вотчер на изменение сети
        console.log(netId);
      })
    }
  }

}
</script>
