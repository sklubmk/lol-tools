<template>
    <v-container>
        <v-row justify="center" align-content="center">
            <v-col cols="2" align-self="center">
                <div class="home_banner_text">{{text}}
                </div>
                <div class="home_add_team">
                    <v-text-field v-model="newId" v-on:keyup.enter="addUser">
                    </v-text-field>
                </div>
            </v-col>
            <v-col cols="1"></v-col>
            <v-col style="min-height: 150px;" cols="9" align-self="center">
                <v-card-text class="home_banner_text" style="height: fit-content;">{{
                        amenities.length
                    }}명
                </v-card-text>
                <v-chip-group
                    v-model="amenities"
                    column
                    multiple
                    class="home_chip_group"
                >
                    <v-chip
                        v-for="(nickname, index) in list"
                        :key="index"
                        filter
                        outlined
                        :text-color="listColorSet[index] === '#fff'?'black':'white'"
                        :style="{'background-color':listColorSet[index]+' !important'}"
                    >
                        {{ nickname }}
                    </v-chip>
                </v-chip-group>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
export default {
    name: "LineChoicer",
    props: {
        text: String,
        list: Array,
        teamCount: Number,
        handleBtn: Boolean,
        resetBtn: Boolean,
    },
    data: () => ({
        amenities: [],
        listColorSet: ['#fff', '#fff', '#fff', '#fff', '#fff', '#fff', '#fff', '#fff', '#fff'],
        header: null,
        texts: [],
        colorSet: ['#2b336b', '#d20b0b', '#ecc80f', '#738934'],
        newId: '',
    }),
    watch: {
        'handleBtn': 'setTeam',
        'resetBtn': 'reset'
    },
    methods: {
        setTeam() {
            if(!this.errorCheck())
                return false;

            const teamEachMembers = this.amenities.length / this.teamCount;
            const teamNums = []
            for (let i = 0; i < this.teamCount; i++) {
                teamNums.push(teamEachMembers)
            }

            this.amenities.forEach((l) => {
                let random = this.getRandomInt(0, this.teamCount)
                while (!teamNums[random]) {
                    random = this.getRandomInt(0, this.teamCount)
                }
                teamNums[random]--;
                this.listColorSet[l] = this.colorSet[random]
            })
            this.$forceUpdate()
            this.$nextTick(() => {
                for (let i = 0; i < this.listColorSet.length; i++)
                    this.listColorSet[i] = '#fff'
            })
        },
        addUser() {
            if (!this.newId)
                return

            this.list.push(this.newId)
            this.listColorSet.push('#fff')
            this.amenities.push(this.list.length - 1)
            this.resetNewId();
        },
        resetNewId() {
            this.newId = ''
        },
        getRandomInt(min, max) {
            min = Math.ceil(min);
            max = Math.floor(max);
            return Math.floor(Math.random() * (max - min)) + min; //최댓값은 제외, 최솟값은 포함
        },
        errorCheck(){
            if (this.text === '팀원 추가') {
                if(!this.amenities.length) {
                    alert('팀원 클릭해서 고르셈')
                    return false
                }
                else if (this.amenities.length % this.teamCount !== 0) {
                    alert('선택된 팀원 숫자가 안맞음')
                    return false
                } else return true
            }
            else{
                return this.amenities.length % this.teamCount === 0
            }
        },
        reset(){
            this.amenities = [];
        }
    }
}
</script>

<style scoped>

</style>