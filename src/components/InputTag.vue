<template>

    <div class="inputTag">
        <div class="tags has-addons">
            <div class="tag is-success is-medium" v-for="(tag, index) in tags" :key="index">
                {{ tag }} <button @click="deleteTag(tag)" class="delete is-delete"></button>
            </div>
        </div>
        <!-- El evento tipo submit se activa cuando el formulario es enviado -->
        <!-- prevent para omitir el comportamiento por defecto del formulario, que proviene del nativo preventDefault() en js. Evita recargar la pagina-->
        <form @submit.prevent="handleSubmit">
            <input class="input" type="text" v-model="currentValue" @keydown="handleKeyDown"
                placeholder="Write your tags..." />
        </form>
    </div>
</template>

<script>

export default {
    // cada vez que haya un cambio (como que se agregue una new tag) vamos a devolver la lista de etiquetas
    // emits onTagsChange regresa la lista de resultados (tags) para poder reutilizarlo de forma externa. Tambien se puede hacer con props
    emits: ['onTagsChange'],
    data() {
        return { // regresamos nuestro objeto de estado
            currentValue: '',
            tags: [], // cada una de las etiquetas que vamos creando se van a agregar aqui y su vez, se van a renderizar dentro de la clase tags.
        }
    },

    methods: {
        handleKeyDown(e) {
            // permite que cada tag sea un elemento unico y que al borrar un tag en el input, se vayan eliminando el resto de tags tambien
            if (e.key === "Backspace" && this.currentValue === '') {
                this.tags.pop(); //eliminar el ultima etiqueta de nuestra lista 
                this.$emit('onTagsChange', this.tags);
            }
        },
        handleSubmit() {
            if (this.currentValue !== '' && this.currentValue !== ' ') { // si - currentValue (es el valor del elemento actual) no es una string vacia y no empieza con un espaciador
                // el metodo some comprueba si al menos un elemento del array cumple con la condicion implementada por la funcion proporcionada
                const exist = this.tags.some(item => item === this.currentValue) // exist = un item es igual a un item actual (tag con mismo nombre)
                if (!exist) { // si un tag NO ES IGUAL a otro tag...
                    this.tags.push(this.currentValue) // añadir el valor del elemento actual a la array tags.
                    this.currentValue = ''; // que se vacíe el input una vez enviado
                    this.$emit('onTagsChange', this.tags);
                }
            }
        },
        deleteTag(tag) {
            this.tags = this.tags.filter((item) => item !== tag);
            // this.tags es igual a this.tags.filter, item donde item no sea igual a tag
            // filter = filtra la array tags y le indica que el item no sea igual a un tag
            // regresará todas las etiquetas que sean diferentes al parametro tag
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