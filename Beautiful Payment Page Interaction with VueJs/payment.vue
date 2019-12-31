<template>
  <div class="containing-body flex-col a-c w100 h100">

      <div class="flip-card w30">
        <div :class="{'flip-card-inner': true, rotateCard: showBack}">
          <div class="flip-card-front">
            <span class="card-logo flex-row a-c-n space-btw w100p">
              <span></span>
              <img :src="'/card-assets/'+cardTypeLogo" alt="" class="card-logo">
            </span>
            <p class="poppins card-number">{{hashing}}</p>
            <p class="card-detail-bottom flex-row space-btw a-c-n">
              <span class="card-name poppins">{{cardName}}</span>
              <span class="expiry-date poppins">{{expiryDate}}</span>
            </p>
          </div>

          <div class="flip-card-back flex-row a-c">
            <p class="cvv poppins">
              {{CVV}}
            </p>
          </div>
        </div>
      </div>

      <form action="" class="white-card flex-col a-c w40 bgNeutral">
        <div class="w90p input-container">
          <label class="poppins lil-bold">Card Number</label><br>
          <input type="text" name="" id="" class="w100p poppins" v-model="cardNumber" @keyup="validateEtRenderCardNumber()" @blur="starCardNumber()">
        </div>
        <div class="w90p input-container">
          <label class="poppins lil-bold">Card Name</label><br>
          <input type="text" name="" id="" class="w100p poppins" v-model="cardName" >
        </div>

        <div class="w90p input-container flex-row space-btw a-c-n">
          <div class="w45p">
            <label class="poppins lil-bold">Expiry Date</label><br>
            <input type="text" name="" id="" class="w100p poppins" placeholder="MM/YY" v-model="expiryDate" @keyup="validateEtRenderExpiry()">
          </div>
          <div class="w45p">
            <label class="poppins lil-bold">CVV</label><br>
            <input type="text" name="" id="" class="w100p poppins" v-model="CVV" @focus="showCardBack()" @blur="showCardFront()">
          </div>
        </div>

        <div class="w90p input-container">
          <button type="button" :class="{'poppins white w100p point': true, 'btn001 anime-btn-001': processing}" @click="sendPaymentRequest()">{{buttonText}}</button>
        </div>
      </form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      cardNumber: '',
      realCardNumber: '',
      cardName: '',
      cardTypeLogo: 'visa.png',
      expiryDate: '',
      CVV: '',
      hashing: '####  ####  ####  ####',
      star: '****  ****  ****  ',
      showBack: false,
      buttonText: 'Submit',
      processing: false
    }
  },
  methods: {
    validateEtRenderCardNumber () {
      this.realCardNumber = this.cardNumber.trim()
      if (this.realCardNumber[0] === '5') {
        this.cardTypeLogo = 'mastercard.png'
      }
      if (this.realCardNumber[0] === '4') {
        this.cardTypeLogo = 'visa-white.png'
      }
      if ((this.realCardNumber[0] !== '4') && (this.realCardNumber[0] !== '5')) {
        this.cardTypeLogo = 'visa.png'
      }

      if ((this.cardNumber !== '') && (this.realCardNumber !== '')) {
        if ((this.realCardNumber.length === 4) || (this.realCardNumber.length === 10) || (this.realCardNumber.length === 16)) {
          this.cardNumber += '  '
        }
      }
      this.hashing = this.realCardNumber + Array((22 - this.realCardNumber.length) + 1).join('#')
    },
    starCardNumber () {
      if (this.realCardNumber.length >= 22) {
        this.hashing = '****  *****  ****  ' + this.realCardNumber.slice(16, this.realCardNumber.length)
      }
    },
    validateEtRenderExpiry () {
      if (this.expiryDate !== 0) {
        if (this.expiryDate.length === 2) {
          this.expiryDate += '/'
        }
      }
    },
    showCardBack () {
      this.showBack = true
    },
    showCardFront () {
      this.showBack = false
    },
    sendPaymentRequest () {
      this.processing = true
      this.buttonText = 'Processing...'
    }
  }
}
</script>

<style scoped>
.card-logo{
  width: 3.5rem;
  height: 2.2rem;
  position: relative;
  left: -1.2rem;
  top: .2rem;
}
.cvv{
  font-weight: 600;
  font-size: 1.3rem;
  color: black;
  position: relative;
  left: -4rem;
}
.expiry-date{
  font-weight: 500;
  font-size: 1.05rem;
}
.card-detail-bottom{
  width: 85%;
  padding: 0 0 0 1rem;
  margin-top: 4.65rem;
}
.card-number{
  color: white;
  font-weight: 500;
  font-size: 1.2rem;
  text-align: center;
  margin-top: 3.2rem;
  box-sizing: border-box;
  width: 60%;
  margin-left: 2.5rem;
}

button{
  padding: .5rem 0;
  text-align: center;
  margin-top: .5rem;
  color: white;
  border: 0;
  background: rgb(76, 76, 236);
  border-radius: 3px;
  font-size: 1.1rem;
  letter-spacing: 1px;
  font-family: 'Gilroy-norm', 'Josefin Sans', Raleway, Arial;
}
button:hover{
  opacity: .75;
}
.lil-bold{
  font-weight: 600;
  color: rgb(61, 61, 61);
  font-size: .85rem;
}
.input-container{
  margin-bottom: 1.7rem;
}
input{
  margin-top: .5rem;
  padding: .5rem 1rem;
  box-sizing: border-box;
  border-radius: 3px;
  border: 1.2px solid rgba(145, 145, 145, 0.6);
  font-size: 1.1rem;
  color: rgb(99, 99, 99);
}
.containing-body{
  background: rgba(177, 177, 252, .7);
}
.bgNeutral{
  background: white;
}
.w45p{
  width: 46%;
}
.w100p{
  width: 100%;
}
.w90p{
  width: 90%;
}
.white-card{
  border-radius: 5px;
  padding: 7rem 0 1rem 0;
  box-shadow: 0 12px 32px rgba(0,0,0,.15);
}

.flip-card {
  background-color: transparent;
  height: 220px;
  border-radius: 10px;
  perspective: 1000px; /* Remove this if you don't want the 3D effect */
  margin-bottom: -4.5rem;
  z-index: 50;
  position: relative;
  left: 1rem;
  animation-name: cardbounce;
  animation-duration: 8s;
  animation-delay: 2s;
  animation-iteration-count: infinite;
}

@keyframes cardbounce {
  0%{transform: translateY(0);}
  30%{transform: translateY(-20px);}
  60%{transform: translateY(0);}
  90%{transform: translateY(20px);}
  100%{transform: translateY(0);}
}

/* This container is needed to position the front and back side */
.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 0.8s;
  transform-style: preserve-3d;
  border-radius: 10px;
}

/* Do an horizontal flip when you move the mouse over the flip box container */
.rotateCard {
  transform: rotateY(180deg);
}

/* Position the front and back side */
.flip-card-front, .flip-card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 10px;
  background-size: contain;
  background-repeat: no-repeat;
}

/* Style the front side (fallback if image is missing) */
.flip-card-front {
  background-image: url('~@/assets/card-bg/card-front.png');
  color: white;
}

/* Style the back side */
.flip-card-back {
  background-image: url('~@/assets/card-bg/card-back.png');
  color: white;
  transform: rotateY(180deg);
}

.btn001{
    color: white;
  }
  .anime-btn-001 {
    z-index: 1;
    position: relative;
    font-size: 1.1rem;
    outline: none;
    border: none;
    border-radius: 6px;
    background-color: rgb(44, 135, 240);
    overflow: hidden;
    transition: color 0.4s ease-in-out;
    padding: .5rem 1rem;
    margin-right: 1.5rem
  }

  .anime-btn-001::before {
    content: '';
    z-index: -1;
    position: absolute;
    top: 100%;
    right: 100%;
    width: 3em;
    height: 3em;
    border-radius: 50%;
    background-color: #fee715;
    transform-origin: center;
    transform: translate3d(50%, -50%, 0) scale3d(0, 0, 0);
    transition: transform 0.7s ease-in-out;
  }

  .anime-btn-001:hover {
    cursor: pointer;
    color: #000;
  }

  .anime-btn-001:hover::before {
    transform: translate3d(50%, -50%, 0) scale3d(15, 15, 15);
  }
</style>
