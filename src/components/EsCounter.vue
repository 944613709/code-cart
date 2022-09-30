<template>
    <div class="counter-container">
    <!-- 数量 -1 按钮 -->
    <button type="button" class="btn btn-light btn-sm"
    @click="onSubClick">-</button>
    <!-- 输入框 -->
    <input type="number" class="form-control form-control-sm ipt-num" v-model.number.lazy='number'/>
    <!-- 数量 +1 按钮 -->
    <button type="button" class="btn btn-light btn-sm"
    @click="onAddClick">+</button>
    </div>
   </template>



   <script>
   export default {
    name: 'EsCounter',
    emits:['numChange'],
    watch:{
        number(newVal){
            const parseRes = parseInt(newVal);
            if(isNaN(parseRes)||parseRes<1)
                {
                    this.number=1;
                    return;
                }
            if(String(newVal).indexOf('.')!=-1)
            {
                this.number=parseRes;
                return;
            }
            this.$emit('numChange',this.number)
        }
    },
    props:{
        num:{
            type : Number,
            default: 0,
        },
        min:{
            type: Number,
            default: 1
        }
    },
    data(){
        return{
            number: this.num
        }
    },
    methods:{
        onAddClick(){
            this.number++;
        },
        onSubClick(){
            if (!isNaN(this.min) && this.number - 1 < this.min) return
            this.number -= 1
        }
    }
   }
   </script> 
   
   
<style lang="less" scoped>
    .counter-container {
 display: flex;}

 .btn {
 width: 25px;
 }

 .ipt-num {
 width: 34px;
 text-align: center;
 margin: 0 4px;
 }
   </style>