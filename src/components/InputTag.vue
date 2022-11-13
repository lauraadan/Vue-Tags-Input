<template>

    <div class="inputTag">
        <div class="tags has-addons">
            <div class="tag is-success is-medium" v-for="(tag, index) in tags" :key="index">
                {{ tag }} <button @click="deleteTag(tag)" class="delete is-delete"></button>
            </div>
        </div>
        <form @submit.prevent="handleSubmit">
            <input class="input" type="text" v-model="currentValue" @keydown="handleKeyDown"
                placeholder="Write your tags..." />
        </form>
    </div>
</template>

<script>

export default {
    emits: ['onTagsChange'],
    data() {
        return {
            currentValue: '',
            tags: [],
        }
    },

    methods: {
        handleKeyDown(e) {
            if (e.key === "Backspace" && this.currentValue === '') {
                this.tags.pop();
                this.$emit('onTagsChange', this.tags);
            }
        },
        handleSubmit() {
            if (this.currentValue !== '' && this.currentValue !== ' ') {
                const exist = this.tags.some(item => item === this.currentValue)
                if (!exist) {
                    this.tags.push(this.currentValue)
                    this.currentValue = '';
                    this.$emit('onTagsChange', this.tags);
                }
            }
        },
        deleteTag(tag) {
            this.tags = this.tags.filter((item) => item !== tag);
            this.$emit('onTagsChange', this.tags);
        }
    }
}
</script>



<style scoped>
.inputTag {
    display: flex;
    flex-direction: column;
    height: 100vh;
    align-items: center;
    justify-content: center;
    padding: 0px 100px;
}

.tags {
    align-items: center;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.tags .tag {
    padding: 20px;
    margin: 10px;
    display: flex;
}

.tag:not(body).is-success {
    background-color: #96BE8C;
    color: #393E46;
}

.inputTag form {
    display: inline-flex;
    width: 500px;
}

.inputTag .input {
    padding: 0px 10px;
    background-color: #fff;
    border-color: #C9F2C7;
    border-radius: 5px;
}

.tag button {
    border: none;
    cursor: pointer;
}

.tag:not(body) .delete {
    margin-left: 1.25rem;
}

.tag button:hover {
    background-color: rgb(174, 45, 45);
}

.tag:not(body) {
    border-radius: 0px;
}
</style>