<template>
    <div class="box">
        <div class="a">
            <select name="year" id="" v-model="year">
                <option v-for="x in 2077" :value="x">{{ x }}</option>
            </select>
            <select name="month" id="" v-model="month">
                <option v-for="x in 12" :value="x">{{ x }}</option>
            </select>
        </div>
        <div class="b">
            <table border="1">
                <caption>
                    <th v-for="x in 7">周{{ x - 1 == 0 ? '日' : x - 1 }}</th>
                </caption>
                <tr v-for="x in 6" style="display: flex; justify-content: space-around;">
                    <td v-for="y in 7">{{ day.length?Object.values(day[x*y])[0]:'' }}</td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    components: {},
    props: {},
    data() {
        return {
            year: null,
            month: null,
            day: [],
        };
    },
    watch: {
        year() {
            this.setDay();
        },
        month() {
            this.setDay()
        }
    },
    computed: {
        
    },
    methods: {
        setDay() {
            this.day = [];
            for (let i = 1; i <= new Date(this.year, this.month, 0).getDate(); i++) {
                if (i == 1) {
                    if (new Date(this.year + '-' + this.month + '-' + i + ' 8:00:00').getDay() != 0) {
                        this.setDay_b(new Date(this.year + '-' + this.month + '-' + i + ' 8:00:00').getDay());
                    }
                }
                this.day.push({
                    // '星期': (new Date(this.year + '-' + this.month + '-' + i + ' 8:00:00').getDay()),
                    '日期': this.bu(i),
                });

            }
            if(this.day.length<45){
                this.setDay_c(45-this.day.length);
            }
        },
        setDay_b(num) {         // 头补丁
            for (let i = 1; i <= num; i++) {
                this.day.unshift({
                    // '星期': num-i,
                    '日期': new Date(this.year, this.month-1, 0).getDate()-i,
                });
            }
        },
        setDay_c(num){          // 尾补丁
            for (let i = 0; i < num; i++) {
                this.day.push({
                    // '星期': (this.day.at(-1)['星期']+1)%7,
                    '日期': this.bu(i+1),
                });
            }
        },
        bu(num){   // 补0
            if(num<=9){
                return '0'+num;
            }else{
                return num;
            }
        }
    },
    created() {

    },
    mounted() { }
};
</script>
<style lang="scss" scoped>
div.box {
    width: 400px;
    height: 400px;
    border: 1px solid black;

    div.a {
        text-align: center;
    }
}
</style>