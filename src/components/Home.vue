<template>
    <div class="home_container">
        <section v-for="item in sortedPriorityByValue" v-bind:key="item.id">
            <p class="todo_contents">
                {{ item.title }}{{ item.deadLine }}
            </p>
        </section>
    </div>
</template>
<script>
export default {
    name: 'Home',
    // computed:{
    //     sortedPriorityByValue(){
    //         return this.priority.sort((a.b)=>{
    //             return a.priority-b.prioroty;
    //         });
    //     }
    // },
    data(){
        return{

        }
    }
}
</script>
