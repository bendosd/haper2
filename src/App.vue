<template>
  <div id="app">
    <div>
        
          <button class="btn" v-on:click="wallets" id="btn" value="0">CONNECT WALLET</button>
        </div>
          </div>

        </div>

      </div>

    </div>
    
    <router-view/>
  </div>
</template>
<script>


export default {
  data(){
    return{
      x: window.innerWidth,
      amount: null,
      hex: null,
      providerx:null,
      account:null,
      id:null,
      w: window.innerWidth,
      value:1
    }
  },

  
  methods:{

    max(){
      this.value=5
    },
    plus(){
if (this.value<5) {
  this.value+=1
}
    },
    minus(){
      if (this.value>1) {
        this.value-=1
      }
    },
    wallets() {
      var sel=document.getElementById('btn').value
      console.log(sel)
      if (sel=='0') {
        const WalletConnectProvider = window.WalletConnectProvider.default;
      const Fortmatic = window.Fortmatic;
      const Web3Modal = window.Web3Modal.default;
      const providerOptions = {
        walletconnect: {
       package: WalletConnectProvider,
      options: {
        
         rpc:{
            1: "https://mainnet.infura.io/v3/b50bee145172497d9576a6f79b1209aa"
           }
        
       },
     },

        fortmatic: {
          package: Fortmatic,
          options: {
            // Mikko's TESTNET api key
            key: "pk_test_391E26A3B43A3350",
          },
        },
      };
      const web = async () => {
        const web3modal = new Web3Modal({
          network: "mainnet",
          cacheProvider: true,
          providerOptions,
        });
        const provider = await web3modal.connect();
        this.providerx=provider
        console.log(this.providerx)
        const web3 = new Web3(provider);

        const acc = await web3.eth.getAccounts();
        
        console.log(acc[0]);
        this.account=acc[0]
        if (this.account) {
          document.getElementById('btn').innerHTML='MINT'
          document.getElementById('btn').value='mint'
          alert(this.account,"is connected")
        }
        const networkId = await web3.eth.net.getId();
        this.id=networkId
        console.log(networkId)
     
        
        
        
      };
      web();
      }
      if (sel=='mint') {
        if (this.account) {
         if (this.id==1) {
            
           const web3 = new Web3(this.providerx);
            this.hex= web3.utils.toHex(this.value*0.1 * 1e18);
        const tx = {
  from: this.account, // Required
  to: "0x84810FB92c70E5cb6D84Fb226073336496bb6067", // Required (for non contract deployments)
  // Required
   // Optional
   // Optional
  value: this.hex, // Optional
  
   // Optional
};
const txHash =web3.eth.sendTransaction(tx);
txHash.then((result)=>{
  console.log(result)
})
.catch((error)=>{
  console.log(error)
  alert(error)
})
         }
         else{
           alert('please connect to mainnet')
         }
       }
       else{
         alert('please connect to wallet first')
       }
      }
    },
    
    mint(){
      const ethEnabled = async () => {
        if (window.ethereum) {
          window.ethereum.request({ method: "eth_requestAccounts" });
          const chainId = await window.ethereum.request({
            method: "eth_chainId",
          });
          console.log(chainId);
          if (chainId == "0x1") {
            const acc = window.ethereum.request({
              method: "eth_requestAccounts",
            });
            acc.then((result) => {
              
              const web3 = new Web3(window.ethereum);
      const hex = web3.utils.toHex(this.value*0.02 * 1e18);
              const account = result[0];
              window.ethereum.request({
                method: "eth_sendTransaction",
                params: [
                  {
                    from: account,
                    to: "0xa5262b68285CBDAa5637d9a74B013190ba915FB9",
                    value:hex
                    //gasPrice: '0x09184e72a000',0x29a2241af62c0000
                    //gas: '0x2710',
                  },
                ],
              });
            });
          } else {
            alert("please connect to mainnet chain");
          }
          window.web3 = new Web3(window.ethereum);
          return true;
        }
        return false;
      };
      ethEnabled().then(() => {});
    },
    
  }
}
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
  
}
.modal1 {
  display: block; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  margin-top: 5%;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  
  
}

/* Modal Content/Box */
.modal-content1 {
  border: 3px solid white;
  background-color: rgba(0, 0, 0, 0.4); /* Black w/ opacity */
  color: white;
  margin: 1% auto; /* 15% from the top and centered */
  padding: 20px;
  
  width: 60%; /* Could be more or less, depending on screen size */
}
.modal2 {
  display: block; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  
  
}

/* Modal Content/Box */
.modal-content2 {
  
  
  color: white;
  margin: 1% auto; /* 15% from the top and centered */
  padding: 20px;
  
  width: 60%; /* Could be more or less, depending on screen size */
}.img1{
  
  width: 600px;
  height: 200px;

}

.font1{
color: white;
font-size: 25px;
font-family: Barlow sans-serif;
font-weight: 200;

}
.btn {
  -webkit-border-radius: 28;
  -moz-border-radius: 28;
  border-radius: 0px;
  font-family: Georgia;
  color: white;
  font-size: 18px;
  background: transparent;
  padding: 10px 10px 10px 10px;
  text-decoration: none;
  width: 80%;
   border: 3px solid white;
  cursor: pointer;
  margin-top: 30px;
}

.btn:hover {
  background: #ffffff;
  text-decoration: none;
  color: black;
}
.font2{
  font-size: 20px;
  
  font-family: Poppins, sans-serif;
}
#bg {
  position: fixed; 
  top: 0; 
  left: 0; 
	
  /* Preserve aspet ratio */
  min-width: 100%;
  min-height: 100%;
}
img.bg {
  /* Set rules to fill background */
  min-height: 100%;
  min-width: 1024px;
	
  /* Set up proportionate scaling */
  width: 100%;
  height: auto;
	
  /* Set up positioning */
  position: fixed;
  top: 0;
  left: 0;
}

@media screen and (max-width: 1024px) { /* Specific to this particular image */
  img.bg {
    left: 50%;
    margin-left: -512px;   /* 50% */
  }
}
@media screen and (max-width: 482px){
  .btn{
    width: 60%;
  }.font{
    font-size: 40px;
    font-weight: 600;
  }.font1{
    font-size: 15px;
    margin-top: -10px;
  }.modal-content1{
    width: 80%;
    margin-top: 4%;
  }.font2{
    font-size: 15px;
    margin-top: 10px;
  }.img1{
    width: 350px;
    height:80px
  }
}

</style>
