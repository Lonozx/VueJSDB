<template>

    <div class="backdrop" @click.self="closeModal">
        <div class="modal" :class="{sale: theme==='sale'}">
            <h1>{{header}}</h1>
            <h1>{{last_name}}</h1>
            <p>{{text}}</p>
            
            <img :src="avatar" alt="">
            <p>{{number}}</p>
           
        </div>
    </div>


</template>

<script>
export default {
    props: ['header', 'text', 'avatar', 'last_name', 'address', 'number'],

    methods:{
        closeModal(){
            this.$emit('close');
        }
    }
    }

</script>
<style scoped>

.modal{
    width: 400px;
    position: fixed;
    top: 20px;
    left: 20px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background: #beb1ff;
    border-radius: 10px;
    border: 2px solid;
}
.backdrop{
    top:0;
    left: 0;
    position: fixed;
    width: 100%;
    height: 100%;
}

.modal.sale{
    background: crimson;
    color: white;

}
.modal.sale h1{
    color: white;
}

</style>