<template>
  <v-img  src="./baita.jpg" class="v-imgs" />
  <v-img  src="./bah.gif" class="v-imgz" />
</template>

<style scoped>
.v-imgs {
    position: absolute;
    top: 0;
    left: 0;
    width: 100vw;        
    height: 100vh;        
    object-fit: cover;    
    overflow: hidden;     
    opacity: 0.7;         
    z-index: 1;         
}

.v-imgz {
    position: absolute;  
    top: 0;
    left: 0;
    width: 190%;         
    height: 100%;        
    object-fit: cover;    
    overflow: hidden;      
    opacity: 0.7;       
    z-index: 0;         
}
</style>