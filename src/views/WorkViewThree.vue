<script setup>
import { computed, ref} from 'vue';


//購物車清單
const carList=ref([]);
//購物車清單的備註
const cardemo=ref('');
//訂單完成
const buycarList=ref(
    {
        carList:undefined,
        demo:'',
        PayPrice:0
    });

//左邊選單加入購物車中
const addCar=(todos)=>{
    buycarList.value={ ...buycarList};//清空已購買清單
    const carTodos ={ ...todos};
    carTodos.count=1;
    const UdIndex=carList.value.findIndex(item=>item.id==carTodos.id);
    if (UdIndex>-1)
    {  //已存在商品直接加+1
        const newCount=carList.value[UdIndex].count;
        if (newCount>=10){alert('超過可選擇數量')}
        else{carList.value[UdIndex].count++;}
    }else{
        carList.value.push(carTodos);
    }
}
//小計
const totalPrice= computed(() => {
      return carList.value.reduce((a,b)=>  a + (b.price* b.count), 0)
  });
 //確定送出購物車
const SendCar=()=>{
    buycarList.value.carList=carList.value;
    buycarList.value.PayPrice=totalPrice.value;
    buycarList.value.demo=cardemo.value;
    carList.value=[];//清空購物車
    cardemo.value='';
    console.log(carList);
}
//清空購物車
const ClearCar=()=>{
    carList.value=[];//清空購物車
    cardemo.value='';
}
// 刪除資料 
const ClickDel=(Index)=>{
  const DelIndex=  carList.value.findIndex(item=>item.Id===Index.Id)
  carList.value.splice(DelIndex,1);

}
//todolist
 const data = [
  {
    "id": 1,
    "name": "珍珠奶茶",
    "description": "香濃奶茶搭配QQ珍珠",
    "price": 50,
    "count":0
  },
  {
    "id": 2,
    "name": "冬瓜檸檬",
    "description": "清新冬瓜配上新鮮檸檬",
    "price": 45,
    "count":0
  },
  {
    "id": 3,
    "name": "翡翠檸檬",
    "description": "綠茶與檸檬的完美結合",
    "price": 55,
    "count":0
  },
  {
    "id": 4,
    "name": "四季春茶",
    "description": "香醇四季春茶，回甘無比",
    "price": 45,
    "count":0
  },
  {
    "id": 5,
    "name": "阿薩姆奶茶",
    "description": "阿薩姆紅茶搭配香醇鮮奶",
    "price": 50,
    "count":0
  },
  {
    "id": 6,
    "name": "檸檬冰茶",
    "description": "檸檬與冰茶的清新組合",
    "price": 45,
    "count":0
  },
  {
    "id": 7,
    "name": "芒果綠茶",
    "description": "芒果與綠茶的獨特風味",
    "price": 55,
    "count":0
  },
  {
    "id": 8,
    "name": "抹茶拿鐵",
    "description": "抹茶與鮮奶的絕配",
    "price": 60,
    "count":0
  }
]
</script>

<template>
    <div id="root">
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-4">
        <div class="list-group" v-for="todos in data" :key="todos.id" >
          <a href="#" @click.prevent="addCar(todos)" class="list-group-item list-group-item-action">
            <div class="d-flex w-100 justify-content-between">
              <h5 class="mb-1">{{todos.name}}</h5>
              <small>${{ todos.price }}</small>
            </div>
            <p class="mb-1">{{todos.description}}</p></a>
        </div>
      </div>
      <div class="col-md-8">
        <table class="table">
          <thead>
            <tr>
              <th scope="col" width="50">操作</th>
              <th scope="col">品項</th>
              <th scope="col">描述</th>
              <th scope="col" width="90">數量</th>
              <th scope="col">單價</th>
              <th scope="col">小計</th>
            </tr>
          </thead>
          <tbody >
            <tr v-for="car in carList" :key="car.id">
              <td><button type="button" class="btn btn-sm" @click="ClickDel">x</button></td>
              <td>{{car.name}}</td>
              <td><small>{{car.description}}</small></td>
              <td>
                <select class="form-select" v-model="car.count">
                  <option value="1">1</option>
                  <option value="2">2</option>
                  <option value="3">3</option>
                  <option value="4">4</option>
                  <option value="5">5</option>
                  <option value="6">6</option>
                  <option value="7">7</option>
                  <option value="8">8</option>
                  <option value="9">9</option>
                  <option value="10">10</option>
                </select>
              </td>
              <td>{{car.price}}</td>
              <td>{{car.price *car .count}}</td>
            </tr>
          </tbody>
        </table>

        <div class="text-end mb-3"  v-if="carList.length>0">
          <h5>總計: <span>${{totalPrice}}</span></h5>
        </div>
        <textarea v-model="cardemo"  v-if="carList.length>0"
          class="form-control mb-3" rows="3"   placeholder="備註"  >
        </textarea>
        <div class="d-grid gap-2 d-md-flex justify-content-md-end" v-if="carList.length>0">
            <button type="button" class="btn btn-danger btn-lg"  @click="ClearCar">清空</button>
          <button type="button" class="btn btn-primary btn-lg" @click="SendCar">送出</button>
        </div>
        <div class="alert alert-dark" role="alert" v-else>請選擇商品</div>
      
      </div>
    </div>

    <hr />
    <div class="row justify-content-center" v-if="buycarList.carList">
      <div class="col-8">
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
                  <tr v-for="buytodo in buycarList.carList" :key="buytodo.id">
                    <td>{{buytodo.name}}</td>
                    <td>{{buytodo.count}}</td>
                    <td>{{buytodo.price *buytodo.count}}</td>
                  </tr>
                </tbody>
              </table>
              <div class="text-end">備註: <span>{{buycarList.demo}}</span></div>
              <div class="text-end">
                <h5>總計: <span>${{buycarList.PayPrice}}</span></h5>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</template>