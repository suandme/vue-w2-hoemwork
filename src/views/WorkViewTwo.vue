<script setup>
import axios from 'axios';
import { ref } from 'vue';
//請求的API
const ApiUrl="https://todolist-api.hexschool.io";
//註冊
const SignUpResMsg=ref('');
const SignUpDataObject=ref({
    email: '',
    password: '',
    nickname:''
});
 const SignUpSend=()=>{
    axios.post(`${ApiUrl}/users/sign_up`,SignUpDataObject.value)
    .then((res)=>
        {SignUpResMsg.value="註冊失敗";
            if (res.data.status)
            {
                SignUpResMsg.value="註冊成功(uid:"+res.data.uid+")";
            }
        }).catch((error)=>{
            SignUpResMsg.value="註冊失敗"+error;
        })
      
}
//登入
const SignInResMsg=ref('');
const SignIntoken=ref('');
const SignInpDataObject=ref({
    email: '',
    password: '',
});
const SignInSend=()=>{
    axios.post(`${ApiUrl}/users/sign_in`,SignInpDataObject.value)
    .then((res)=>
        {SignInResMsg.value="登入失敗";
            if (res.data.status)
            {//exp,token,nickname
                SignIntoken.value=res.data.token;
                SignInResMsg.value=`登入成功<br/>token<br/>${SignIntoken.value}` ;
                alert(`歡迎 ${res.data.nickname} 登入成功`)
            }
        }).catch((error)=>{
            SignInResMsg.value="登入失敗"+error;
        })
      
}
//驗證
const CheckToken=ref('');
const CheckoutResMsg=ref('');
const CheckoutToken=async ()=>{
    try {
        const res = await axios.get(`${ApiUrl}/users/checkout`, {
        headers: {
            Authorization: CheckToken.value,
        },
        });
            const tomorrow = new Date();
            tomorrow.setDate(tomorrow.getDate() + 1);
            document.cookie = `hexschooltoken=${CheckToken.value}; expires=${tomorrow.toUTCString()}`;

            CheckoutResMsg.value = '驗證成功 UID: ' + res.data.uid;
  } catch (error) {
      CheckoutResMsg.value = '驗證失敗: ' + error.message;
  }
      
}
//登出
const hexschooltoken=ref('');
const SingOutResMsg=ref('');
const SingOutSend=async ()=>{
    try {
       
        const res = await axios.post(`${ApiUrl}/users/sign_out`,{}, {
        headers: {
            Authorization: hexschooltoken.value,
        },
        });
        if (res.data.status)
        {
            SingOutResMsg.value = '登出成功';
        }
        //  console.log( response.data);
    } catch (error) {
        SingOutResMsg.value = '登出失敗: ' + error.message;
    }
      
}
</script>

<template>
<h2>註冊</h2>
<p>
    Email: <input type="text" placeholder="Email" v-model="SignUpDataObject.email">
    Password:<input type="text" placeholder="Password" v-model="SignUpDataObject.password">
    Nickname: <input type="text" placeholder="Nickname" v-model="SignUpDataObject.nickname">
    <button type="button" @click="SignUpSend">送出</button>
 <br/>{{SignUpResMsg}}
</p>
<hr>
<h2>登入</h2>
<p>
    Email: <input type="text" placeholder="Email" v-model="SignInpDataObject.email">
    Password:<input type="text" placeholder="Password" v-model="SignInpDataObject.password">
    <button type="button" @click="SignInSend">登入</button>
    <br/><span v-html="SignInResMsg"></span>
</p>
<hr>
<h2>驗證</h2>
<p>
    Token: <input type="text" placeholder="Token"  v-model="CheckToken" >
    <button type="button" @click="CheckoutToken">驗證</button>
    <br/> {{CheckoutResMsg}}
</p>
<hr>
<h2>登出</h2>
<p>
    Token: <label for=""></label> <input type="text" placeholder="Token"  v-model="hexschooltoken" >
    <button type="button" @click="SingOutSend">登出</button>
    <br/> {{SingOutResMsg}}
</p>
<hr>
<h2>Todo list</h2>
</template>
