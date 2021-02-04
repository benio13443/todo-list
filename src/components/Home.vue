<template>
    <div class="home_container">
        <section v-for="item in sortedItemsByAmount" v-bind:key="item.id">
            <p class="todo_contents">
                {{ item.title }}{{ item.deadLine }}{{ item.expectedTime }}
            </p>
        </section>
    </div>
</template>