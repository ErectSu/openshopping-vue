<template>
<div>

    <headerNav title="我的地址"/>
    <van-address-list
  v-model="chosenAddressId"
  :class="isSelect?'':'hideselect'"
  :list="list"
  @add="onAdd"
  @edit="onEdit"
  @select="onSelect"
/>
</div>
</template>

<script>

import { GetAddressList } from "../../../api/user.js";
import { AddressList } from 'vant';
export default {
    components:{
        [AddressList.name]:AddressList,
    },
    data() {
        return {
        chosenAddressId: '1',
        isSelect:false,
        list: [],
        }
    },

    methods: {
        onAdd() {
            this.$router.push('/user/address/edit')
        },

        onEdit(item) {
            this.$router.push('/user/address/edit?id='+item.id);
        },
        onSelect(item){
            if(!this.isSelect){
                return;
            }
            this.$emit('selectAddress',item);
            this.$router.go(-1);
        }
    },
    created:function(){
        this.chosenAddressId=this.$route.query.id;
        this.isSelect=this.$route.query.id>0;
        GetAddressList().then(response=>{
            this.list=response;
        })
    }

}
</script>

<style lang="less">
.hideselect{
    .van-radio__input{
        display: none;
    }
}
</style>
