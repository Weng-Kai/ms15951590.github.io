<script>
export default {
  data() {
    return {
      msgSwitch:false,
      editMsg: '',
      switchBtn: 'all',
    };
  },

  mounted(){
    //網頁開啟後先執行裡面的JS
    //先把資料從LocalStorage特定key拿出資料，丟入msgArr裡面
    console.log(1234);
    if(localStorage.getItem('msg')){
      this.msgArr = JSON.parse(localStorage.getItem('msg'));
    };
  },

  //預先處理拿到的資料，他具有站存功能，因此我們可以拿整包資料，運用判斷是篩選我們的資料
  /*1.先在data宣告一個變數  
    2.在切換的button上利用v-model綁上這個變數
    3.利用computed (arr.filter)進行篩選
    4.再把原本v-for的地方，換上computed的fn上去
  */
  computed:{
    selectMsg(){
      return this.msgArr.filter((item)=>{
        if(this.switchBtn === 'all'){
          return this.msgArr;
        }else if(this.switchBtn === '已完成'){
          return item.checkBox == true;
        }else if(this.switchBtn === '未完成'){
          return item.checkBox == false;
        }
      }

      )
    }
  },

  methods:{
    addMsg(){
      const id = this.msgArr.length ?? 0;
      this.msgArr.push({
        id:id+1,
        msg: this.ToMsg,
        checkBox: false,
        //編輯模式
        msgSwitch: false,
        //編輯文字
        editMsg: '',
      });
    },
    editMsg(item){
      console.log(item);
      item.msgSwitch = !item.msgSwitch;
      if(item.editMsg !==''){
        item.msg = item.editMsg;
      }
      if(item.msgSwitch === true){
        item.editMsg = item.msg;
      }
      this.msg = '',
      localStorage.setItem('msg', JSON.stringify(this.msgArr));
    },

    deleteMsg(i){
      console.log(i);
      //方法一(帶入的是item.id)
      this.msgArr = this.msgArr.filter(deLeteData => deLeteData.id !==i);
      //方法二(帶入的是item.id)
      const deleteList = this.msgArr.filter(deLeteData => deLeteData.id !==i);
      console.log(deleteList);
      this.msgArr = deleteList;
      //方法三(帶入的是 ins+dex)
      this.msgArr.splice(i,1);
      localStorage.setItem('msg', JSON.stringify(this.msgArr));
    },
  },
};
</script>

<template>
  <div class="">
    <div class="box p-10">
      <div>
        <input type="text" value="123" class="border-2 mr-3 drop-shadow-sm">
        <button class="
          w-16
          bg-sky-300
          hover:bg-sky-400 
          active:bg-sky-600 
          text-base
          active:text-white
          drop-shadow-sm">新增</button>
      </div>

      <div class="mt-3">
        <button class="mr-3 
              bg-emerald-300
              hover:bg-emerald-400 
              active:bg-emerald-600 
              text-base
              active:text-white
              drop-shadow-sm
              w-16">全部</button>
              <button class="mr-3 
              bg-emerald-300
              hover:bg-emerald-400 
              active:bg-emerald-600 
              text-base
              active:text-white
              drop-shadow-sm
              w-16">已完成</button>
              <button class="mr-3 
              bg-emerald-300
              hover:bg-emerald-400 
              active:bg-emerald-600 
              text-base
              active:text-white
              drop-shadow-sm
              w-16">未完成</button>
      </div>

      <table class="mt-3 border-2 table-auto w-full">
        <thead class=" bg-orange-200">
          <tr class=" font-black">
            <th class=" font-black">執行</th>
            <th class=" font-black">事項</th>
            <th class=" font-black">功能</th>
          </tr>
        </thead>
        <tbody class="text-center">
          <tr class=" border border-slate-300 my-5">
            <td class="">
              <input v-model="item.checkBox" type="checkbox" class="">
            </td>

            <td class=" border border-slate-300">
              <span>{{ item.msg }}</span>
            </td>

            <td class="">
              <button class="
              bg-amber-300
              hover:bg-amber-400 
              active:bg-amber-600 
              text-base
              active:text-white
              drop-shadow-sm
              w-16
              mr-1 my-1">編輯</button>

              <button class="
              bg-red-300
              hover:bg-red-400 
          *:  active:bg-red-600 
              text-base
              active:text-white
              drop-shadow-sm
              w-16
              mr-1 my-1">刪除</button>
            </td>
          </tr>
        </tbody>
      </table>

    </div>
  </div>
</template>

<style scoped>
/* .tab {
  @apply bg-[#000080] text-white rounded-t-md py-2 px-4 font-bold;
}

.tab.active {
  @apply bg-[#f1e394] text-black;
} */
</style>
