<template>
  <q-page class="bg-grey-4 column">
    <div class="row q-pa-sm bg-secondary">
      <q-input 
      class="col"
      rounded 
      v-model="newItem" 
      placeholder="Add item" 
      dense
      @keyup.enter="addItem()">
        <template v-slot:append>
          <q-btn round dense flat icon="add" 
            @click="addItem()" />
        </template>
      </q-input>

      <q-btn 
      unelevated 
      color="red-5" 
      label="Delete All"
      @click="deleteAll()" />
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item v-for="(item, index) in list" 
      :key="item.title" 
      @click="item.done = !item.done"
      clickable
      :class="{'done bg-red-1' : item.done}"
      v-ripple>
        <q-item-section avatar top>
          <q-checkbox 
          v-model="item.done"
          class="no-pointer-events" 
          color="cyan" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{item.title}}</q-item-label>
        </q-item-section>
        <q-item-section
        v-if="item.done"
        side>
        <q-btn
        @click.stop="deleteItem(index)" 
        flat
        round
        dense
        color="black"
        icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div 
    class="empty-list absolute-center"
    v-if="!list.length">
      <q-icon
      name="check"
      size="100px"
      color="secondary" />
      <div class="text-h5 text-primary text-center">
        No items
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data(){
    return{
      newItem: '',
      list:[]
    }
  },
  methods:  {
    deleteItem(index){
      this.$q.dialog({
        title: 'Confirm',
        message: 'Are you sure that you want to permanently delete this item?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.list.splice(index, 1) 
        this.$q.notify('Deleted')
      })
    },
    addItem(){
      if (!this.newItem) {
        
      }else{
        this.list.push({
          title:  this.newItem,
          done: false
        })
        this.newItem = ''
      }
    },
    deleteAll(){
      this.$q.dialog({
        title: 'Confirm',
        message: 'Are you sure that you want to permanently delete this items?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        for (let index = 0; index<=this.list.length; index++) {
          if (this.list[index].done) {
            this.list.splice(index, 1)
          } 
        }
        this.$q.notify('Deleted')
      })
    }
  }
}
</script>
<style lang="scss">
  .done{
    .q-item__label{
      text-decoration: line-through;
      color: rgb(187, 187, 187);
    }
  }
  .empty-list{
    opacity: 0.7;
  }
</style>