<!-- 新訂單 -->
<script setup>
// 引入defineProps
import { defineProps } from 'vue';
//定義
defineProps(['inner-buycarList'])
//物件屬性要轉成小駝峰innerBuycarList
</script>
<template>
       <div class="card">
          <div class="card-body">
            <div class="card-title">
              <h5>訂單</h5>
              <table class="table">
                <thead>
                  <tr>
                    <th scope="col">品項</th>
                    <th scope="col">數量</th>
                    <th scope="col">小計</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="buytodo in innerBuycarList.carList" :key="buytodo.id">
                    <td>{{buytodo.name}}</td>
                    <td>{{buytodo.count}}</td>
                    <td>{{buytodo.price *buytodo.count}}</td>
                  </tr>
                </tbody>
              </table>
              <div class="text-end">備註: <span>{{innerBuycarList.demo}}</span></div>
              <div class="text-end">
                <h5>總計: <span>${{innerBuycarList.PayPrice}}</span></h5>
              </div>
            </div>
          </div>
        </div>
</template>