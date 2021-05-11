<template>
    <v-container>
        <v-row>
            <div class="home_banner home_banner_text"> 팀 정하기</div>
        </v-row>
        <v-row style="min-height: 150px">
            <div class="home_banner_text" style="height: fit-content;">팀원 선택 | 총 {{amenities.length}}명</div>
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
                    {{nickname}}
                </v-chip>
            </v-chip-group>
        </v-row>
        <v-spacer style="width:100%; height: 100px" />
        <v-row align-content="center" justify="center">
            <div class="home_banner_text">팀원 추가
            </div>
            <div class="home_add_team">
                <v-text-field v-model="newId" v-on:keyup.enter="addUser">
                </v-text-field>
            </div>
        </v-row>
        <v-row>
            <div class="home_banner_text">팀 개수</div>
            <ButtonCounter @team="val => this.teamLength = val"/>
        </v-row>
        <v-spacer style="width:100%; height: 100px" />
        <v-row>
            <div class="home_banner_text">
                <v-btn :color="'#95adc4'" @click="setTeam">팀 나누기</v-btn>
            </div>
        </v-row>
    </v-container>
</template>

<script>
import ButtonCounter from "./ButtonCounter";
export default {
    name: "LadderGame",
    data: () => ({
        amenities: [],
        list: ['프로파오후','Poltn','샤쓰','Kaco','DHA','댄디댄디','럿트','바둠랑이','욕먹기대회우승자'],
        listColorSet:['#fff','#fff','#fff','#fff','#fff','#fff','#fff','#fff','#fff'],
        colorSet:['#2b336b','#d20b0b','#ecc80f','#738934'],
        newId:'',
        teamLength:2,
    }),
    components:{
        ButtonCounter,
    },
    methods:{
        addUser(){
            if(!this.newId)
                return

            this.list.push(this.newId)
            this.listColorSet.push('#fff')
            this.amenities.push(this.list.length-1)
            this.resetNewId();
        },
        resetNewId(){
            this.newId = ''
        },
        setTeam(){
            if(!this.amenities.length){
                alert('팀원 클릭해서 고르셈')
                return;
            }

            if(this.amenities.length % this.teamLength !== 0){
                alert('선택된 팀원 숫자가 안맞음')
                return;
            }

            const teamEachMembers = this.amenities.length / this.teamLength;
            const teamNums = []
            for(let i = 0; i < this.teamLength; i++){
                teamNums.push(teamEachMembers)
            }

            this.amenities.forEach((l)=>{
                let random = this.getRandomInt(0,this.teamLength)
                while(!teamNums[random]){
                    random = this.getRandomInt(0,this.teamLength)
                }
                teamNums[random]--;
                this.listColorSet[l] = this.colorSet[random]
            })
            this.$forceUpdate()
            this.$nextTick(()=>{
                for(let i = 0 ; i < this.listColorSet.length; i++)
                    this.listColorSet[i] = '#fff'
            })
        },
        getRandomInt(min, max) {
            min = Math.ceil(min);
            max = Math.floor(max);
            return Math.floor(Math.random() * (max - min)) + min; //최댓값은 제외, 최솟값은 포함
        }
    },
}
</script>

<style scoped>
    .home_banner_text{
        width: 100%;
        text-align: center;
        vertical-align: center;
        margin:10px;
    }
    .home_banner {
        height: 300px;
        line-height: 300px;
        font-size: 1.4rem;
    }
    .home_chip_group{
        max-width: 80%;
        width: fit-content;
        margin: 0 auto;
    }
    .home_add_team{
        width: 100px;
    }
</style>