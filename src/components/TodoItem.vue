<template>
    <div>
        <el-card shadow="never" :class="priority" :style="finished ? 'opacity:0.5;' : ''" class="box-card">
            <div>{{ title }}</div>
            <el-button v-if="!finished" @click="finishTodo">Dokončit</el-button>
            <el-button @click="deleteTodo">Odstranit</el-button>
        </el-card>    
    </div>
</template>

<script>
export default {
    props: {
        id: Number,
        title: String,
        priority: String,
        finished: Boolean,
    },
    emits: ['finish', 'delete'],
    methods: {
        finishTodo() {
            this.$emit('finish', this.id);
        },
        deleteTodo() {
            this.$emit('delete', this.id);
        }
    },
}
</script>

<style scoped>
.box-card {
    margin-bottom: 2em;
}

.high {
    background-color: #dc3545;
}

.normal {
    background-color: #ffc107;
}

.low {
    background-color: #28a745;
}
</style>