<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <q-page class="row background items-center">
        <div class="row justify-center items-center q-pa-lg">
          <div class="q-mx-lg q-mb-md">
            <p
              class="text-center"
              v-show="codeLimitMsg"
              :class="{
                warningTextHide: !codeLimitMsg,
                warningTextShow: codeLimitMsg,
              }"
            >
              You Enter Only 4 digits
            </p>
            <q-input
              v-if="pinCodeWrong === false"
              v-model="pinCode"
              filled
              placeholder="ENTER PIN CODE"
              readonly
              input-class="text-center text-white"
              :class="{
                pinCode: pinCodeCorrect === false,
                pinCodeCorrect: pinCodeCorrect,
              }"
            />
            <q-input
              v-if="pinCodeWrong"
              v-model="pinCode"
              placeholder="WRONG PIN"
              filled
              readonly
              input-class="text-center text-white"
              class="pinCodeWrong"
            />
          </div>
          <q-btn
            v-for="i in 9"
            color="primary q-ma-sm"
            class="col-xs-3"
            key="i"
            @click="enterPinCode(i)"
            >{{ i }}</q-btn
          >
          <q-btn color="primary" class="col-xs-3 q-ma-sm" @click="clearInput"
            >C</q-btn
          >
          <q-btn
            color="primary"
            class="col-xs-3 q-ma-sm"
            @click="enterPinCode(0)"
            >0</q-btn
          >
          <q-btn color="primary" class="col-xs-3 q-ma-sm" @click="checkPinCode">
            <q-icon name="subdirectory_arrow_left" />
          </q-btn>
        </div>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref } from "vue";

let pinCode = ref("");
let pinCodeCorrect = ref(false);
let pinCodeWrong = ref(false);
let userName = ref("");
let codeLimit = ref(4);
let loginPinCode = ref("5412");
let codeLimitMsg = ref(false),
  enterPinCode = function (code) {
    pinCodeWrong.value = false;
    codeLimit.value = codeLimit.value - 1;
    if (codeLimit.value > -1) {
      pinCode.value = pinCode.value + code;
    } else {
      codeLimitMsg.value = true;
      setTimeout(() => (codeLimitMsg.value = false), 2000);
    }
  },
  checkPinCode = function () {
    if (loginPinCode.value === pinCode.value) {
      pinCodeCorrect.value = true;
      router.replace("/home/");
      pinCode.value = "";
    } else {
      pinCode.value = "";
      codeLimit.value = 4;
      pinCodeWrong.value = true;
    }
  },
  clearInput = function () {
    pinCode.value = "";
    codeLimit.value = 4;
  };
</script>
<style lang="scss">
.background {
  background: url("../assets/bg2.jpg");
  background-size: cover;
  background-position: center center;
}
.warningTextHide {
  opacity: 0;
  transition: all 0.25s;
}
.warningTextShow {
  color: darkorange;
  opacity: 1;
  transition: all 500ms ease-in-out;
}
.pinCode {
  color: #8e8e8e;
  font-weight: 200;
  text-align: center;
  font-size: 30px !important;
  text-shadow: 2px 2px 4px #000000;
}

.pinCodeCorrect {
  color: #8e8e8e;
  font-weight: 200;
  text-align: center;
  border: green solid 1px;
  font-size: 30px !important;
  text-shadow: 2px 2px 4px #000000;
}

.pinCodeWrong {
  color: red;
  font-weight: 200;
  text-align: center;
  border: red solid 1px;
  font-size: 30px !important;
  text-shadow: 2px 2px 4px #000000;
  animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  perspective: 1000px;
}

@keyframes shake {
  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}
</style>
