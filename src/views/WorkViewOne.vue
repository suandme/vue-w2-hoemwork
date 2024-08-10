<script setup>
import { ref } from 'vue';
/*建立資料列 */
const MenuTodo=ref([
  {
    Id:1,
    Fooditems:'珍珠奶茶',
    Describe:'香濃奶茶搭配QQ珍珠',
    Price:50,
    Stock:20,
    Lockbut:false,
  }, {
    Id:2,
    Fooditems:'冬瓜檸檬',
    Describe:'清新冬瓜配上新鮮檸檬',
    Price:45,
    Stock:18,
    Lockbut:false,
  }, {
    Id:3,
    Fooditems:'翡翠檸檬',
    Describe:'綠茶與檸檬的完美結合',
    Price:55,
    Stock:34,
    Lockbut:false,
  }, {
    Id:4,
    Fooditems:'四季春茶',
    Describe:'香醇四季春茶，回甘無比',
    Price:45,
    Stock:10,
    Lockbut:false,
  }, {
    Id:5,
    Fooditems:'阿薩姆奶茶',
    Describe:'阿薩姆紅茶搭配香醇鮮奶',
    Price:50,
    Stock:25,
    Lockbut:false,
  }, {
    Id:6,
    Fooditems:'檸檬冰茶',
    Describe:'檸檬與冰茶的清新組合',
    Price:45,
    Stock:20,
    Lockbut:false,
  }, {
    Id:7,
    Fooditems:'芒果綠茶',
    Describe:'芒果與綠茶的獨特風味',
    Price:55,
    Stock:18,
    Lockbut:false,
  }, {
    Id:8,
    Fooditems:'抹茶拿鐵',
    Describe:'抹茶與鮮奶的絕配',
    Price:60,
    Stock:10,
    Lockbut:false,
  }
]);
/*錯誤訊息 */
const Errormsg=ref('');
/*加減庫存 */
const ClickStock=(Index,category)=>{
  Index.Lockbut=false;Errormsg.value="";
  if (category===0)
  {
    Index.Stock--;
    if (Index.Stock<=0)
    {
      Index.Stock=1;
      Index.Lockbut=true;
      Errormsg.value="庫存不能低於0"
    }
  }else  if (category===1) {Index.Stock++; }

  /*更新欄位 */
  MenuTodo.value
 .filter(item=>item.Id===Index.Id)
 .map(item=>item.Stock==Index.Stock);
 
}

// /*刪除資料 */
// const ClickDel=(Index)=>{
//   const DelIndex=  MenuTodo.value.findIndex(item=>item.Id===Index.Id)
//   MenuTodo.value.splice(DelIndex,1);
// console.log(DelIndex)
// }
/*編輯更欄位 */
const EditData=ref({});
const EditUpdataData=(todo)=>{
    EditData.value={ ...todo } ; //拷貝
}
const CheckUpdataData=()=>{
   Errormsg.value="";  EditData.value.Lockbut=false;
    const UdIndex=  MenuTodo.value.findIndex(item=>item.Id===EditData.value.Id)
    if (EditData.value.Stock<=0)
    {
      EditData.value.Lockbut=true;
      Errormsg.value="庫存不能低於0"
    }else{
      MenuTodo.value[UdIndex]=EditData.value;
      EditData.value={};
    }
   

}
</script>

<template>
  <h1>【題目】：餐點管理工具</h1>
  <div>
   <p>
  Level 1：將菜單轉為資料格式<br/>
  Level 2：可以重新設定菜單的庫存數量<br/>
  Level 3（挑戰）：可以重新設定品項名稱<br/>
  <br/>
</p> 
  </div>
  <div>
    <hr>
    <table>
    <thead>
      <tr>
        <th scope="col">品項</th>
        <th scope="col">描述</th>
        <th scope="col">價格</th>
        <th scope="col">庫存</th>
        <th scope="col">管理</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="todo in MenuTodo" :key="todo.id">
        <td>{{todo.Fooditems}}</td>
        <td><small>{{todo.Describe}}</small></td>
        <td>{{todo.Price}}</td>
        <td><button type="button" :disabled="todo.Lockbut" @click="ClickStock(todo,0)">-</button>
          {{todo.Stock}}
          <button type="button" @click="ClickStock(todo,1)">+</button>
        </td>
        <td><button type="button"  @click="EditUpdataData(todo)">編輯</button>
          <!-- <button type="button" @click="ClickDel(todo)">刪除</button> -->
        </td>
      </tr>
    </tbody>
  </table>
</div>
<div class="errormsg">{{ Errormsg}}</div>
<hr>
<div v-if="EditData.Id">
  <h2>編輯資料</h2>
  <span>品項:<input type="text"  maxlength="100" v-model="EditData.Fooditems"></span>
  <span>描述:<input type="text" maxlength="200"  v-model="EditData.Describe"></span>
  <span>價格:<input  type="text" maxlength="7"  v-model.number="EditData.Price"></span>
  <span>庫存:<input  type="text" maxlength="7"  v-model.number="EditData.Stock"></span>
  <hr>
  <span>
    <button type="button"  @click="CheckUpdataData">確定修改</button>
    <button type="button" @click="EditData={}">取消修改</button>
  </span>
</div>
</template>

<style scoped>
.errormsg{
  color: red;
  font-size: 16px;
}
input {
  width: 300px;
}
div{
  display: flex;
  flex-direction: column;
}
div>span{
  margin: 10px;
}
table{
  text-align: center;
}
</style>
