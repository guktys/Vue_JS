<template>
  <div>
    <div class="post">
<div><strong>Tittle:</strong> About js</div>
      <div><strong>Text:</strong> About js</div>
    </div>

  </div>
</template>

<script>
export default {
  data(){
return{
  like:0, dislike:0
}
  },
  methods:{
    addLike(){
      this.like+=1;
    },
    addDislike(){
      this.dislike+=1;
    }
  }

}

</script>
<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
