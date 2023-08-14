<template>
    <div>
        <Message :msg="msg" v-show="msg" />
        <form id="burger-form" @submit="createBurger" >
            <div class="input-container">
                <label for="name">Client Name</label>
                <input type="text" id="name" name="name" v-model="name" placeholder="Type your name">
            </div>

            <div class="input-container">
                <label for="bread">Choose the bread: </label>
                <select name="bread" id="bread" v-model="bread">
                    <option value="">Select your bread</option>
                    <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">{{ bread.tipo }}</option>
                </select>
            </div>

            <div class="input-container">
                <label for="meat">Choose the meat: </label>
                <select name="meat" id="meat" v-model="meat">
                    <option value="">Select the meat type</option>
                    <option v-for="meat in meats" :key="meat.id" :value="meat.tipo">{{ meat.tipo }}</option>
                </select>
            </div>

            <div id="optional-container" class="input-container">
                <label for="optional" id="optional-title">Optional: </label>
                <div class="checkbox-container" v-for="optionalOne in optionalData" :key="optionalOne.id">
                    <input type="checkbox" name="optional" v-model="optional" :value="optionalOne.tipo">
                    <span>{{ optionalOne.tipo }}</span>
                </div>
            </div>

            <div class="input-container">
                <input type="submit" class="submit-btn" value="Create my burger">
            </div>
        </form>
    </div>
</template>

<script>
    import Message from './Message.vue'

    export default {
        name: 'BurgerForm',
        data() {
            return {
                breads: null,
                meats: null,
                optionalData: null,
                name: null,
                bread: null,
                meat: null,
                optional: [],
                status: "Requested",
                msg: null
            }
        },
        components: {
            Message
        },
        methods: {
            async getIngredients() {
                const req = await fetch("http://localhost:3000/ingredientes")
                const data = await req.json()

                this.breads = data.paes;
                this.meats = data.carnes;
                this.optionalData = data.opcionais;
            },
            async createBurger(e) {
                e.preventDefault();

                const data = {
                    name: this.name,
                    meat: this.meat,
                    bread: this.bread,
                    optional: Array.from(this.optional),
                    status: "Requested"
                }

                const dataJson = JSON.stringify(data)

                const req = await fetch("http://localhost:3000/burgers", {
                    method: "POST",
                    headers: {"Content-Type": "application/json"},
                    body: dataJson
                })

                const res = await req.json()

                this.msg = `Order N° ${res.id} requested successfully`

                setTimeout(() => this.msg = "", 3000)

                this.name = ""
                this.meat = ""
                this.bread= ""
                this.optional = ""
            }
        },
        mounted() {
            this.getIngredients()
        }

    }
</script>

<style scoped>
    #burger-form {
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }

    label {
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #FCBA03;
    }

    input, select {
        padding: 5px 10px;
        width: 300px;
    }

    #optional-container {
        flex-direction: row;
        flex-wrap: wrap;
    }

    #optional-title {
        width: 100%;
    }

    .checkbox-container {
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }

    .checkbox-container span,
    .checkbox-container input {
        width: auto;
    }

    .checkbox-container span {
        margin-left: 6px;
        font-weight: bold;
    }

    .submit-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding:  10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .submit-btn:hover {
        background-color: transparent;
        color: #222;
    }

</style>