<template>
    <option value="none">Выберите филиал</option>
    <option :key="index" v-for="(value, key, index) in filials" :value="key" class="filial">{{ value }}</option>
</template>
<script>
import axios from 'axios';

export default {   
    data() {
        return {
            filials: null,
        }
    },
    mounted() {
        axios
            .get('api/v1_0/Orient?type=getFilials')
            .then((respond, status, jqXHR) => {
                if (typeof respond.error === 'undefined') {
                    this.filials = respond.data;                 
                }
            });
    },

}
</script>
