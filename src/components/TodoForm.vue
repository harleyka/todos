<template>
    <div>    
        <el-form :rules="rules">
            <el-form-item label="Název">
                <el-input type="text" v-model="form.title"></el-input>
            </el-form-item>

            <el-form-item label="Priorita">
                <el-radio-group v-model="form.priority">
                    <el-radio label="low">Nízká</el-radio>
                    <el-radio label="normal">Normální</el-radio>
                    <el-radio label="high">Vysoká</el-radio>
                </el-radio-group>
            </el-form-item>

            <el-form-item>
                <el-button :disabled="form.title === ''" type="primary" @click="createTodo">Přidat</el-button>
                <el-button @click="cancel">Cancel</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            form: {
                title: '',
                id: null,
                priority: 'normal',
                finished: false,
            }
        }
    },
    emits: ['add', 'cancel'],
    methods: {
        createTodo() {
            if (!this.form) return;
            if (this.form.title === '') {
                return;
            }
            this.$emit('add', this.form);
        },
        cancel() {
            this.$emit('cancel');
        },
    }
}
</script>