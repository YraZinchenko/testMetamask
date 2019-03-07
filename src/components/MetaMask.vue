<template>
  <div>
    <VueMetamask 
      userMessage="msg" 
      @onComplete="onComplete"
    >
    </VueMetamask>
    <template v-if="isEntrance">
      <div>
        <div>
          <h2>Adress:</h2> <h3 class="wrap-word">{{ adress }}</h3>
        </div>
        <div>
          <h2>Network id:</h2> <h3>{{ networkCode }}</h3>
        </div>
        <v-text-field
          label="Text"
          outline
          v-model="signature"
          hide-details
        ></v-text-field>
        <div>
          <v-btn color="success" @click="signatureMsg">Sign posts</v-btn>
        </div>
        <template v-if="signatureArray.length">
          <div>
            <h1>
              Signed messages:
            </h1>
          </div>
        </template>
        <template v-if="signatureArray.length">
          <div v-for="item in signatureArray" :key="item" >
            <div class="wrap-word">
              <h4>{{ item }}</h4>
            </div>
          </div>
        </template>
      </div>
    </template>
    <template v-if="!isEntrance && appReady">
      <div>
        <h1>
          Sign up or sign in for use app.
        </h1>
        <div>
          <v-btn color="info" @click="entrance">Sign up</v-btn>
        </div>
      </div>
    </template>
    <template v-if="!appReady">
      <div>
        <h1>
          The extension MetaMask is not installed, install to start using the application.
        </h1>
        <div>
          <v-btn color="warning" large @click="installMetaMask">Install</v-btn>
        </div>
      </div>
    </template>
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
      adress: null,
      networkCode: null,
      appReady: false
    }
  },
  methods:{
    onComplete(data){
      // console.log(data);
    },
    signatureMsg() {
      if(this.signature.length) {
        web3.personal.sign(web3.fromUtf8(this.signature), web3.eth.coinbase, (err, result) => {
          if(err){
            alert("cancel");
          } else {
            this.signatureArray.push(result);
            this.signature = '';
          }
        });
      }
    },
    async entrance() {
      await ethereum.enable();
      if(ethereum.selectedAddre) {
        this.adress = ethereum.selectedAddress;
        this.networkCode = ethereum.networkVersion;
        this.isEntrance = true;
      }
    },
    changeCode(code) {
      this.adress = code[0];
    },
    installMetaMask() {
      window.open("https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn");
    }
  },
  created() {
    if(typeof window.ethereum !== 'undefined') {
      this.appReady = true;
      if(ethereum.selectedAddress) {
      this.isEntrance = true;
      this.networkCode = ethereum.networkVersion;
      this.adress = ethereum.selectedAddress;
      const salfe = this;
      ethereum.on('accountsChanged', function (accounts) {
        // вотчер на изменение адреса аккаунта
        salfe.changeCode(accounts);
      })
      window.ethereum.on('networkChanged', function (netId) {
        // вотчер на изменение сети
        this.networkCode = netId;
      })
    }
    } else{
      console.log(1);
    }
  }

}
</script>
