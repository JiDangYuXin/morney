<template>
  <Layout class-prefix="layout">
    {{record}}
    <NumberPad @update:value="onUpdateAmount" @submit="saveRecord"/>
    <Types :value.sync="recprd.type" />
    <Notes @update:value="onUpdateNotes"/>
    <Tags :data-source.sync="tags"  @update:value='onUpdateTags'/>
   </Layout>
</template>
<script lang="ts">
import Vue from 'vue'
import NumberPad from "@/components/Money/NumberPad.vue";
import Types from "@/components/Money/Types.vue";
import Notes from "@/components/Money/Notes.vue";
import Tags from "@/components/Money/Tags.vue";
import {Component, Watch} from "vue-property-decorator";




type Record ={
  tags: string[];
  notes: string;
  type: string;
  amount: number;
  createdAt?:Date;
}


@Component({
  components: {Tags, Notes, Types, NumberPad}
}
)
export default class Money extends Vue{
  tags=['衣','食','住','行','彩票'];
  recordList: Record[]=JSON.parse(window.localStorage.getItem('recordList') ||'[]');
  recprd: Record={
    tags:[],notes:'',type:'-',amount:0
  };

  onUpdateTags(value: string[]){
    this.recprd.tags=value;
  }
  onUpdateNotes(value: string){
    this.recprd.notes=value;
  }
 
  // onUpdateAmount(value:string){
  //   this.recprd.amount=parseFloat(value);
  // }

  saveRecord(){
    const record2:Record =JSON.parse(JSON.stringify(this.recprd)) ;
    record2.createdAt= new Date();
    this.recordList.push(record2);

  }
  @Watch('recordList')
  onRecordListChange(){
    window.localStorage.setItem('recordList',JSON.stringify(this.recordList))
  }
}
</script>

<style lang="scss">
.layout-content{
  display: flex;
  flex-direction: column-reverse;
}
.layout-wrapper{

}
</style>


<style lang="scss" scoped>
@import "~@/assets/style/helper.scss";

</style>