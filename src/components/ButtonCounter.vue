<template>
    <v-row justify="center">
        <v-btn rounded @click="minus" small class="button_counter_btn">
            -
        </v-btn>
        <div class="button_counter_div">
            <v-text-field class="button_counter_value" v-model="value" outline>
            </v-text-field>
        </div>
        <v-btn rounded @click="add" small class="button_counter_btn">
            +
        </v-btn>
    </v-row>
</template>

<script>
export default {
    name: "ButtonCounter",
    data: ()=>({
        value : 2
    }),
    methods:{
        add(){
            if(this.value < 4)
               this.value++;
            this.$emit('team',this.value)
        },
        minus(){
            if(this.value > 2)
                this.value--;
            this.$emit('team',this.value)
        }
    }
}
</script>

<style scoped>
    .button_counter_div{
        width: 80px !important;
    }
    .button_counter_value >>> input {
        text-align: center !important;
    }
    .button_counter_btn{
        margin: 20px 15px 0 15px;
    }
</style>